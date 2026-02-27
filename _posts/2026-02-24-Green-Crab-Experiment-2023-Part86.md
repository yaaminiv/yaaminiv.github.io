---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 86
tags: green-crab-wc RNA-Seq EnTAP
---

## Transcriptome annotation

I now want to annotate my cleaned transcriptome. To do this, I will use [`EnTAP`](https://entap.readthedocs.io/en/latest/index.html), which seems to be made specifically for non-model eukaryotic organisms! I used [Zac's script](https://github.com/tepoltlab/RhithroLoxo_DE/blob/master/Snakefile#L405) as a model since this program is new to me. My EnTAP script can be found [here](https://github.com/yaaminiv/wc-green-crab/blob/main/code/06e-entap.sh).

### 2026-02-24

My first step was to download the databases:

```
# Make EnTap database
# First download reference databases for use in EnTAP: UniProt's uniref90, trembl, and sprot, and NCBI's nr and refseq databases.

wget ftp://ftp.uniprot.org/pub/databases/uniprot/uniref/uniref90/uniref90.fasta.gz
wget ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_trembl.fasta.gz
wget ftp://ftp.ncbi.nih.gov/blast/db/FASTA/nr.gz
wget ftp://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/complete/uniprot_sprot.fasta.gz
wget ftp://ftp.ncbi.nlm.nih.gov/refseq/release/complete/complete.nonredundant_protein.*.protein.faa.gz
cat complete.nonredundant_protein.*.protein.faa.gz > refseq_complete.faa.gz
rm complete.nonredundant_protein.*.protein.faa.gz
```

Before I could move on, I needed to install EnTAP. I created a new `conda` environment [based on Zac's file](https://raw.githubusercontent.com/tepoltlab/RhithroLoxo_DE/refs/heads/master/envs/EnTAP.yaml). I used `wget` to bring the file onto Poseidon, then ran `conda env create --file=EnTAP.yaml`. I got the following error:

> conda env create --file=EnTAP.yaml
Collecting package metadata (repodata.json): - WARNING conda.models.version:get_matcher(544): Using .* with relational operator is superfluous and deprecated and will be removed in a future version of conda. Your spec was 1.9.0.*, but conda is ignoring the .* and treating it as 1.9.0
WARNING conda.models.version:get_matcher(544): Using .* with relational operator is superfluous and deprecated and will be removed in a future version of conda. Your spec was 1.8.0.*, but conda is ignoring the .* and treating it as 1.8.0
WARNING conda.models.version:get_matcher(544): Using .* with relational operator is superfluous and deprecated and will be removed in a future version of conda. Your spec was 1.7.1.*, but conda is ignoring the .* and treating it as 1.7.1
WARNING conda.models.version:get_matcher(544): Using .* with relational operator is superfluous and deprecated and will be removed in a future version of conda. Your spec was 1.6.0.*, but conda is ignoring the .* and treating it as 1.6.0
done
Solving environment: failed
ResolvePackageNotFound:
  - openssl==1.1.1=h7b6447c_0

Based on [my previous experience loading an environment](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part85/), I removed the `=h7b6447c_0` tag and tried again. This is the same package that gave me an issue last time.

## 2026-02-25

### Downloading databases

I noticed my job had failed after 1.5 hours. I looked at the tail of the log file:

> (base) [yaamini.venkataraman@poseidon-l1 ~]$ tail /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/yrv_blast1874746.log
Rejecting ‘complete.1815.bna.gz’.
Rejecting ‘complete.1815.genomic.gbff.gz’.
Rejecting ‘complete.1815.protein.faa.gz’.
Rejecting ‘complete.1815.protein.gpff.gz’.
Rejecting ‘complete.1815.rna.fna.gz’.
Rejecting ‘complete.1815.rna.gbff.gz’.
Rejecting ‘complete.1816.10.genomic.fna.gz’.
Rejecting ‘complete.1816.11.genomic.fna.gz’.
No matches on pattern ‘complete.nonredundant_protein.*.protein.faa.gz’.
cat: complete.nonredundant_protein.*.protein.faa.gz: No such file or directory

Hm, something had happened where it cannot find certain files. I looked at the files present in my output directory:

> (base) [yaamini.venkataraman@poseidon-l1 06e-entap]$ ls -la
total 293528973
drwxrwxr-x 2 yaamini.venkataraman domain users         4096 Feb 24 20:08 .
drwxrwxr-x 2 yaamini.venkataraman domain users         4096 Feb 24 18:24 ..
-rw-rw-r-- 1 yaamini.venkataraman domain users 200016146013 Feb 24 20:07 nr.gz
-rw-rw-r-- 1 yaamini.venkataraman domain users            0 Feb 24 20:08 refseq_complete.faa.gz
-rw-rw-r-- 1 yaamini.venkataraman domain users     93457057 Feb 24 20:07 uniprot_sprot.fasta.gz
-rw-rw-r-- 1 yaamini.venkataraman domain users  52638486746 Feb 24 19:25 uniprot_trembl.fasta.gz
-rw-rw-r-- 1 yaamini.venkataraman domain users  47346635469 Feb 24 18:59 uniref90.fasta.gz
-rw-rw-r-- 1 yaamini.venkataraman domain users    478940808 Feb 24 20:08 yrv_blast1874746.log

Comparing the file list to the error message, it seems like there was an issue downloading the non-redundant protein information from NCBI. When I lood at the [FTP link](https://ftp.ncbi.nlm.nih.gov/refseq/release/complete/), it seems that there are no more files starting with `complete.nonredundant_protein`! So there's my issue. There are files that follow the naming convention `complete.*.protein.faa.gz` but it is unclear to me if they are non-redundant or not. Since that's the only piece of information I have, I'll just need to go with it. I modified the code as follows:

```
wget ftp://ftp.ncbi.nlm.nih.gov/refseq/release/complete/complete.*.protein.faa.gz
cat complete.*.protein.faa.gz > refseq_complete.faa.gz
rm complete.*.protein.faa.gz
```

I also added in information from [python script Zac made](https://github.com/yaaminiv/wc-green-crab/blob/main/code/06e-reformat_uniref.py) to fix the headers:

```
# EnTAP parses taxonomic information from reference database headers for contaminant filtering and taxonomic weighting.
# The existing uniref header format is incompatible with EnTAP. Using Zac's script to reformat the headers so they can be parsed by EnTAP

gunzip -c uniref90.fasta.gz > uniref90.fasta
python 06e-reformat_uniref.py uniref90.fasta uniref90_rf.fasta
gzip uniref90_rf.fasta
rm uniref90.fasta
```

### `EnTAP` environment and installation

While that was running, I activated my `conda` environment! I wanted to see if I could update the various packages to match the latest versions in the [`EnTAP` documentation](https://entap.readthedocs.io/en/latest/Getting_Started/Installation/Installation_From_Source_Code/installation_from_source_code.html#installing-pipeline-software). I also saw that there is a new `conda` version that I potentially should update to. I started by running `conda update -n base -c conda-forge conda` in my `EnTAP` `conda` session and decided to update all potential packages. That failed because I don't have write permissions for the cluster. Totally fine! I instead decided to look at the different packages associated with `EnTAP`. When I looked at my `conda` environment, I realized that I didn't have any EnTAP packages or dependencies installed!! I then realized I probably need to install a clean environment with `mambaforge`, create the environment, and install packages using `mambaforge` [similar to what I did with `trinity`](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part76/).

I loaded the `mamba` module and created the environment based on the YAML file:

`mamba env create -f EnTAP.yaml`

## 2026-02-27

### `EnTAP` environment and installation

Once the environment was created, I activated it and set to work installing packages based on the [`EnTAP` dependency list](https://entap.readthedocs.io/en/latest/Getting_Started/Installation/Installation_From_Source_Code/installation_from_source_code.html#downloading-entap-source-code. It seems like all dependencies except for EggNOG-mapper are already installed as modules on Poseidon.

The list says that `diamond` version 2.0.10 is the minimum required dependency, so I'll need to update `diamond` in addition to installing EggNOG-mapper. RSEM and TransDecoder are versions compatible with EnTAP. InterProScan is a more current version than what is tested to be compatible with EnTAP, so I'll have to see how that shakes out. When I tried to install `diamond`, there was a conflict with the `python` version! I went through my old Gemini log to see if I could figure out what to do. I removed the conda environment (`conda env remove -n EnTAP`), changed `python` to version 3.9 in the YAML file, and remade the environment. This again led to a conflict issue! I then created a clean environment with the following specifications:

Once that was done, I started loading a bunch of modules:

```
#Load modules with EnTAP dependencies
module load bio #Load bio module
module load rsem/1.3.0 #Load RSEM
module load blast/2.7.1
module load transdecoder/5.3.0 #Load TransDecoder
module load interproscan/5.51-85.0 #Load InterProScan

#Load modules to compile EnTAP
module load cmake/3.18.6
module unload gcc
module load gcc/9.3.1
module load boost/gcc9/1.79
```

Finally, I followed the instructions on the [EnTAP installation page](https://entap.readthedocs.io/en/latest/Getting_Started/Installation/Installation_From_Source_Code/installation_from_source_code.html) to actually install EnTAP. I added the program path to my script.

Some things I've learned from this entire process:

- Have a minimal YAML file that installs dependencies or packages that aren't already modules
- Load modules on the cluster
- When needed, I can load complier modules from the cluster

### Modifying databases

Of course, my previous script failed. But for some reason, I keep getting an error stating that the `scratch` directory no longer exists when I try and navigate to it? Rayna wasn't having that issue, and gave me the heads up that `/vortexfs1` is symbolic and I can access `scratch` directly. I confirmed that worked, and made the necessary changes to paths to the scratch directory in my script.

Alright, now to the troubleshooting. I looked at the `tail` of the log file:

> (base) [yaamini.venkataraman@poseidon-l1 06e-entap]$ tail  yrv_blast1875162.log
 97350K .......... .......... .......... .......... .......... 99% 8.27M 0s
 97400K .......... .......... .......... .......... .......... 99% 8.12M 0s
 97450K .......... .......... .......... .......... .......... 99% 8.66M 0s
 97500K .......... .......... .......... .......... .......... 99% 8.21M 0s
 97550K .......... .......... .......... .......... .......... 99% 8.95M 0s
 97600K ......                                                100% 8.03M=12s
2026-02-25 19:07:56 (8.05 MB/s) - ‘complete.wp_protein.9.protein.faa.gz’ saved [99948889]
python: can't open file '/scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/06e-reformat_uniref.py': [Errno 2] No such file or directory
(base) [yaamini.venkataraman@poseidon-l1 06e-entap]$ ls
nr.gz  refseq_complete.faa.gz  uniprot_sprot.fasta.gz  uniprot_trembl.fasta.gz  uniref90.fasta  uniref90.fasta.gz  yrv_blast1874746.log  yrv_blast1875162.log

Seems like it's just a file naming issue! I forgot to specify that the script is in the home directory. I made the necessary change:

```
python ${HOME_DIR}/06e-reformat_uniref.py uniref90.fasta uniref90_rf.fasta
gzip uniref90_rf.fasta
rm uniref90.fasta
```

I also added this next chunk of code to configure EnTAP:

```
# Configure EnTAP. this indexes the reference databases into diamond format
${ENTAP}/EnTAP --config -d nr.gz \
-d uniprot_trembl.fasta.gz \
-d refseq_complete.faa.gz \
-d uniprot_sprot.fasta.gz \
-d uniref90_rf.fasta.gz \
-t 35
```

I started running the script.

### Going forward

1. Annotate transcriptome with `EnTAP`
3. Quantify transcripts with `salmon`
4. Identify differentially expressed genes
5. Additional strand-specific analysis in the supergene region
2. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://the-responsible-grad-student.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>

{% endif %}

<script id="dsq-count-scr" src="//the-responsible-grad-student.disqus.com/count.js" async></script>
