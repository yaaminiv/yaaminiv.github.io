---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 76
tags: green-crab-wc RNA-Seq trinity
---

## Transcriptome assembly

The first step in my RNA-Seq analysis now that I have trimmed reads is to assemble a de novo transcriptome. I was initially going to try and figure out how to use `snakemake` based off Zac and Rayna's code........but that just seemed like way too much effort. Instead, I'm just going to run scripts separately. I can take on the challenge of improving reproducibility with `snakemake` at a later time.

### Program installation

I needed to install `trinity` on the cluster. To do so, I got the latest `trinity` version from Github and attempted to install the program using the command `make`. At this point, I got an error message saying that the Cmake version on the cluster was older than what was needed to install `trinity`. So, I had to install a newer version of Cmake. This was more confusing to figure out than I thought. Eventually, I read the README close enough to figure out what command would update the Cmake version on the cluster. I ran this command and just.......waited. AND THEN I MISSED WHEN IT ASKED ME FOR MY PASSWORD SO I HAD TO WAIT AGAIN. AND THEN I REALIZED I DON'T HAVE `sudo` PERMISSIONS SO I CAN'T ACTUALLY INSTALL CMAKE.

The next morning I reached out to Rayna to see how she got `trinity` installed, and she said she used `bioconda`. I ran the following code:

`conda install -c bioconda trinity`

> The following packages will be downloaded:
    package                    |            build
    ---------------------------|-----------------
    ca-certificates-2025.11.12 |       hbd8a1cb_0         149 KB  conda-forge
    certifi-2025.8.3           |     pyhd8ed1ab_0         155 KB  conda-forge
    collectl-4.0.4             |                2         501 KB  bioconda
    fastool-0.1.4              |       h5bf99c6_5          12 KB  bioconda
    htslib-1.3                 |                0         4.3 MB  bioconda
    java-jdk-7.0.91            |                1        35.7 MB  bioconda
    jellyfish-2.2.10           |       h6bb024c_1         393 KB  bioconda
    jemalloc-4.5.0             |                0         6.2 MB  bioconda
    libgcc-15.2.0              |       h767d61c_7         803 KB  conda-forge
    libgcc-ng-15.2.0           |       h69a702a_7          29 KB  conda-forge
    openssl-1.1.1w             |       hd590300_0         1.9 MB  conda-forge
    parafly-r2013_01_21        |                1         154 KB  bioconda
    perl-threaded-5.32.1       |       hdfd78af_1           5 KB  bioconda
    samtools-1.3.1             |                0         1.5 MB  bioconda
    slclust-02022010           |                2          27 KB  bioconda
    trimmomatic-0.40           |       hdfd78af_0         193 KB  bioconda
    trinity-2.1.1              |                6        61.4 MB  bioconda
    ------------------------------------------------------------
                                           Total:       113.2 MB
The following NEW packages will be INSTALLED:
  collectl           bioconda/linux-64::collectl-4.0.4-2
  fastool            bioconda/linux-64::fastool-0.1.4-h5bf99c6_5
  htslib             bioconda/linux-64::htslib-1.3-0
  java-jdk           bioconda/linux-64::java-jdk-7.0.91-1
  jellyfish          bioconda/linux-64::jellyfish-2.2.10-h6bb024c_1
  jemalloc           bioconda/linux-64::jemalloc-4.5.0-0
  libgcc             conda-forge/linux-64::libgcc-15.2.0-h767d61c_7
  parafly            bioconda/linux-64::parafly-r2013_01_21-1
  perl-threaded      bioconda/noarch::perl-threaded-5.32.1-hdfd78af_1
  samtools           bioconda/linux-64::samtools-1.3.1-0
  slclust            bioconda/linux-64::slclust-02022010-2
  trimmomatic        bioconda/noarch::trimmomatic-0.40-hdfd78af_0
  trinity            bioconda/linux-64::trinity-2.1.1-6
The following packages will be UPDATED:
  ca-certificates    conda-forge/linux-64::ca-certificates~ --> conda-forge/noarch::ca-certificates-2025.11.12-hbd8a1cb_0
  certifi                            2022.12.7-pyhd8ed1ab_0 --> 2025.8.3-pyhd8ed1ab_0
  libgcc-ng                              11.2.0-h1d223b6_12 --> 15.2.0-h69a702a_7
  openssl                                 1.1.1n-h166bdaf_0 --> 1.1.1w-hd590300_0

Using `bioconda` installed `trinity`, as well as dependencies `jellyfish` and `samtools`. All of these programs were now in my `miniconda3` directory: `/vortexfs1/home/yaamini.venkataraman/miniconda3/bin`

I installed `salmon` and `bowtie2` using `conda` as well. I can't believe I forgot that `conda` existed but so glad it does.

### Draft code

While the terminal was doing its thing I figured I would read the `trinity` manual to determine what parameters to use. Ideally, I was looking for a way to run `trinity` on a subset of my data to determine the optimum parameters, or figure out if `trinity` produced multiple potential transcriptomes that I could evaluate later. I also noticed that `trinity` [could be run in chunks to compensate for run time restrictions](https://github.com/trinityrnaseq/trinityrnaseq/wiki/Running-Trinity#running-trinity-in-multiple-sequential-stages). Another thing that jumped out to me is that I could provide a file list to `trinity`, instead of pasting file names separated by commas. I created [this file]() to do just that. I messed around with the idea of making this in `bash`, but Excel was just quicker. I also need to [specify what each file in a pair is (forward and reverse)](https://www.cureffi.org/2012/12/19/forward-and-reverse-reads-in-paired-end-sequencing/). Cool, but I didn't see anything about running the program on a subset of data or trying multiple parameters.

I found [this video](https://www.broadinstitute.org/videos/trinity-how-it-works) to be helpful when trying to figure out what all the commands meant. The wiki has the all of the options, but I wouldn't say they do a good job listing what they actually mean or how changing them can influence the output. When I dug a bit more into the `trinity` Google Group and Github posts, I saw that the recommendation was to just use defaults, and then iterate based on the default output. Zac's transcriptome ExN50 ended up being the best when just using `trinity` defaults anyways.

The last discrepancy I wanted to hash out was using `salmon` to index the transcriptome vs. not. I will end up using `salmon` as a pseudoaligner since that is a best practice. I noticed that Zac used `salmon` to index his transcriptome and get the count matrix. Indexing through `salmon` allowed him to get ExN50 statistics. Grace's script uses `samtools` to generate a index. I needed to actually read some manuals to figure this out, but just getting the `trinity` code running was a bit more important than figuring out the best indexing and assembly statistics steps. I'll probably end up going the `salmon` indexing route since I'll be using `salmon` for the pseudoalignment anyways.

### Running `trinity` assembly (or at least trying to)

I decided to run the steps to assemble the transcriptome, get baseline assembly statistics, and get a gene map file on the cluster first before figuring out the next step:

```
#Exit script if any command fails
set -e

#Program paths
TRINITY=/vortexfs1/home/yaamini.venkataraman/miniconda3/bin/Trinity
CUTADAPT=/vortexfs1/home/yaamini.venkataraman/miniconda3/bin/cutadapt
FASTQC=/vortexfs1/home/yaamini.venkataraman/miniconda3/bin/fastqc
python=/vortexfs1/home/yaamini.venkataraman/miniconda3/bin/python
JELLYFISH=/vortexfs1/home/yaamini.venkataraman/miniconda3/bin/jellyfish
SALMON=/vortexfs1/home/yaamini.venkataraman/miniconda3/bin/salmon
SAMTOOLS=/vortexfs1/home/yaamini.venkataraman/miniconda3/bin/samtools

#Directory and file paths
DATA_DIR=/vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA
OUTPUT_DIR=/vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trimgalore
assembly_stats=assembly_stats.txt
trinity_file_list=/vortexfs1/home/yaamini.venkataraman/trinity-samples.txt

# Run Trinity to assemble de novo transcriptome. Using primarily default parameters.
${TRINITY}/Trinity \
--seqType fq \
--max_memory 100G \
--samples_file ${trinity_file_list} \
--SS_lib_type FR \
--min_contig_length 200 \
--full_cleanup \
--CPU 28

# Assembly stats
${TRINITY}/util/TrinityStats.pl \
${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
> ${assembly_stats}

# Create gene map files
${TRINITY}/util/support_scripts/get_Trinity_gene_to_trans_map.pl \
${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
> ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta.gene_trans_map
```

I gave it the set time limit of 24 hours. I figured I'd see just how far trinity got in 24 hours (assuming my code worked), and based on that I can split it into the different `inchworm`, `chrysallis`, and `butterfly` sections to checkpoint my analysis.

Immediately, I got an error (duh):

>ERROR, don't recognize parameter: --samples_file
Please review usage info for accepted parameters.

This was weird to me, because the `trinity` manual had this parameter! I suspected it was a version issue:

> (base) [yaamini.venkataraman@poseidon-l1 ~]$ /vortexfs1/home/yaamini.venkataraman/miniconda3/bin/Trinity --version
Trinity version: v2.1.1
** NOTE: Latest version of Trinity is Trinity-v2.15.2, and can be obtained at:
	https://github.com/trinityrnaseq/trinityrnaseq/releases

Alright, time to do some updates. I first started by trying to update `trinity`:

```
conda update trinity
```

This, alas, did not work. But I also saw this message during the attempted update:

>==> WARNING: A newer version of conda exists. <==
  current version: 23.5.2
  latest version: 25.9.1
Please update conda by running
    $ conda update -n base -c conda-forge conda
Or to minimize the number of packages updated during conda update use
     conda install conda=25.9.1

I tried running `conda update -n base -c conda-forge conda` but that didn't work! So then I tried the other option, `conda install conda=25.9.1`. THAT WORKED! I applied the same logic to updating `trinity`: `conda install trinity=2.15.2`. That also worked! I checked the version to triple check:

> (base) [yaamini.venkataraman@poseidon-l2 ~]$ miniconda3/bin/Trinity --version
Trinity version: Trinity-v2.15.2
-currently using the latest production release of Trinity.

Back to our regularly scheduled programming of trying to run `trinity` instead of just trying to install `trinity`:

- Once I had the latest version of `trinity` installed, I was able to use the `--samples_file` argument! Almost immediately, the run errored out because I used a wildcard in the path. Changed to absolute paths and I was good!
- `Error, cannot determine salmon version installed from (salmon: error while loading shared libraries: libboost_iostreams.so.1.60.0: cannot open shared object file: No such file or directory) at /vortexfs1/home/yaamini.venkataraman/miniconda3/bin/Trinity line 4111.`
  - `(base) [yaamini.venkataraman@poseidon-l1 ~]$ salmon -help
salmon: error while loading shared libraries: libboost_iostreams.so.1.60.0: cannot open shared object file: No such file or directory`
  - Based on this, I first tried to install the newest version of `salmon`: `conda install salmon=1.10.2`
  - Connection got lost in the middle of this.....and then `conda` just straight up stopped working. Started a whole side quest of having to re-install `conda` and any packages and fix a bunch of conflicts. Used Gemini to figure out how to do this since it was something I'd never encountered before. The eventual solution was to load the `mambaforge` module from Poseidon, create a new `conda` environment for transcriptome assembly, then install the latest versions of `trinity`, `salmon`, and any other dependencies. This way, I'd sidestep any dependency compatibility issues from `miniconda`.
- Updated my code to include specific instructions to load the `mambaforge` module and my specific `conda` environment.

### Going forward

1. Index, get advanced transcriptome statistics, and pseudoalign with `salmon`
4. Clean transcriptome with `EnTap` and `blastn`
4. Identify differentially expressed genes
5. Additional strand-specific analysis in the supergene region
2. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper
6. Start methods and results of 2023 paper

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
