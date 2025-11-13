---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 71
tags: green-crab-wc RNA-Seq fastqc
---

## QC for RNA-Seq data

I'm starting in on the 2023 data analysis! First step is to check quality and trim all my files. I'm running the code on the WHOI Poseidon HPC since my data is stored in the Tepolt Lab folder there (and I don't have the capacity to figure out the SCU cluster just yet). A recap on the data:

- NEBNext UltraExpress RNA kit with Poly A mRNA isolation
- Sequenced on Illumina NovaSeq X Plus
- Included 1% PhiX spike-in
- Got demulitplexed raw data delivered

### FastQC

My code can be found [here](https://github.com/yaaminiv/wc-green-crab/blob/main/code/06a-fastqc.sh). Generating the checksums and FastQC reports for all the files took over 5 hours! At the end, my script actually failed because I couldn't get a MultiQC report (some `python` issue). It seems that this is directly related to `multiqc`:

> (base) [yaamini.venkataraman@poseidon-l2 06a-fastqc]$ multiqc *fastqc.html
Python path configuration:
  PYTHONHOME = '/vortexfs1/apps/intel-python/intelpython3'
  PYTHONPATH = '/vortexfs1/apps/intel-python/intelpython3'
  program name = '/vortexfs1/home/yaamini.venkataraman/miniconda3/bin/python3.9'
  isolated = 0
  environment = 1
  user site = 1
  import site = 1
  sys._base_executable = '/vortexfs1/home/yaamini.venkataraman/miniconda3/bin/python3.9'
  sys.base_prefix = '/vortexfs1/apps/intel-python/intelpython3'
  sys.base_exec_prefix = '/vortexfs1/apps/intel-python/intelpython3'
  sys.platlibdir = 'lib'
  sys.executable = '/vortexfs1/home/yaamini.venkataraman/miniconda3/bin/python3.9'
  sys.prefix = '/vortexfs1/apps/intel-python/intelpython3'
  sys.exec_prefix = '/vortexfs1/apps/intel-python/intelpython3'
  sys.path = [
    '/vortexfs1/apps/intel-python/intelpython3',
    '/vortexfs1/apps/intel-python/intelpython3/lib/python39.zip',
    '/vortexfs1/apps/intel-python/intelpython3/lib/python3.9',
    '/vortexfs1/apps/intel-python/intelpython3/lib/python3.9/lib-dynload',
  ]
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'
Current thread 0x00002aaaaaaeed80 (most recent call first):
<no Python frame>

It seems like the issue is that I'm loading the `python` module, but I also have `python` from `miniconda3`. So, I tried running just the `multiqc` portion of the code without the `python` module and while specifying a path to the `miniconda3 python`:

>   /// MultiQC <U+1F50D> | v1.11
|           multiqc | MultiQC Version v1.32 now available!
|           multiqc | Search path : /scratch/yaamini.venkataraman/wc-green-crab/output/06a-fastqc
|         searching | ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100% 353/353  
|            fastqc | Found 174 reports
|           multiqc | Compressing plot data
|           multiqc | Report      : multiqc_report.html
|           multiqc | Data        : multiqc_data
|           multiqc | MultiQC complete
|           multiqc | 8 flat-image plots used in the report due to large sample numbers
|           multiqc | To force interactive plots, use the '--interactive' flag.
See the documentation.
cp: cannot stat ‘/vortexfs1/home/yaamini.venkataraman/.multiqc_config.yaml’: No such file or directory

Looks like I need to update `miniconda3` on Poseidon. That will be a later task. But my script works! I made sure to update it to reflect the troubleshooting. I then transferred the files from the `scratch` directory to my `home` directory on Poseidon, which allowed me to transfer the files to my Github repository by locally mounting `vortex`. I opened up the [MultiQC report](https://github.com/yaaminiv/wc-green-crab/blob/main/output/06a-fastqc/multiqc_report.html) to check the overall raw sequence quality. Overall quality looks pretty okay considering that the adapter sequences are still there. High per sequence GC content for the majority of samples, and only ~20% unique reads for all samples? I'd be interested to see how these all change once I trim.

### Adapter trimming

It's time to trim! I decided to follow the methods from Zach's paper (Tobias et al. (2021)) since he also had stranded data and had to do de novo transcriptome assembly. Looking through his methods, he states that all of the quality control was done in a previous study, Tepolt et al. (2020). When I looked in the methods of that paper, I didn't see any specific information on trimming. I did, however, find this information in the supplement:

> The resulting raw sequences were cleaned and trimmed using Trim Galore! v0.4.0 (http://www.bioinformatics.babraham.ac.uk/projects/trim_galore/), a wrapper for Cutadapt (Martin, 2011). A quality cutoff of Phred ≥ 20 was used, and reads were only retained if they were ≥ 20 bp long after adapter removal and quality trimming.

Based on this, it seems like my trimming protocol is as follows:

1. Trim low quality reads (Phred ≥ 20)
2. Trim adapter sequences
3. Retain reads ≥ 20 bp long

I first needed to find the adapter sequences! I found [this link](https://www.neb.com/en-us/faqs/2021/01/15/what-sequences-need-to-be-trimmed-for-nebnext-libraries-that-are-sequenced-on-an-illumina-instrument?srsltid=AfmBOoqGuWAevn1loT1LjEgaLQSx7xG-7ONZ2ZjSmipUJjW9fuiGpUhh) and sent an email to NEB Technical Support to confirm these are infact the sequences. I found the same sequences in the manual for the kit as well. Since these are universal Illumina adapters, I could technically use the `--illumina` argument to remove the adapters:

> For adapter trimming, Trim Galore! uses the first 13 bp of Illumina standard adapters ('AGATCGGAAGAGC') by default (suitable for both ends of paired-end libraries), but accepts other adapter sequence, too

Since the [`trimgalore` user guide](https://github.com/FelixKrueger/TrimGalore/blob/master/Docs/Trim_Galore_User_Guide.md#step-2-adapter-trimming) only talks about the first 13 bp, I got paranoid and decicded I should specify the full sequences for trimming, as opposed to having it auto-detect the Illumina adapters. I proceeded to construct my trimming code in [this script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/06b-trimgalore.sh). At first, I had it structured as a multi-step trimming process. When I tried running the script, I realized that `trimgalore` was autodetecting my adapter sequences and proceeding with all steps at once! So, I modified the script to remove the multiple layers. I also found a [script from Sam Bogan](https://github.com/snbogan/CP_RNAseq/blob/main/Scripts/CP_cutadapt.txt) that looped through all file pairs in a directory while running `cutadapt` instead of listing the bajilions of files. I modified his code for my needs and got the following error message:

> Start TrimGalore
Trim 1: Remove low-quality reads
Option p is ambiguous (paired_end, path_to_cutadapt, phred33, phred64, polya)
Please respecify command line options

I then modified the script to reflect arguments used with `trimgalore`, and this was the result:

```
#Loop through input files in the directory
for input_file in ${DATA_DIR}/*_R1_001.fastq.gz; do
 # Extract the base filename
  filename=$(basename "$input_file")
 # Construct the corresponding R2 filename
  input_r2="${input_file/_R1/_R2}"
  # Run TrimGalore to trim adapters, remove low-quality sequences, and retain sequences of a specific length
    ${TRIMGALORE} \
    --output_dir ${OUTPUT_DIR} \
    --paired \
    -a "AGATCGGAAGAGCACACGTCTGAACTCCAGTCA" \
    -A "AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT" \
    --quality 20 \
    --length 20 \
    --fastqc_args \
    "--outdir ${OUTPUT_DIR} \
    --threads 28" \
    --path_to_cutadapt ${CUTADAPT} \
    $input_file \
    $input_r2
  # Print completion message
  echo "Trimming complete for $filename"
done
```

I checked on the error log after an hour to confirm that the script was indeed trimming multiple files! My wall time was 24 hours, so I was a little unsure if all the files would finish in the designated window. Alas, only 63 of the 174 files finished in 24 hours. I considered asking for a wall time extension, but then realized my trimming was running on a single core!! I added `--cores 8`` to the code to run multiplex the process. That gave me this warning message (which I am choosing to believe is fine because I didn't exceed eight cores):

> Trim 1: Remove low-quality reads
Using an excessive number of cores has a diminishing return! It is recommended not to exceed 8 cores per trimming process (you asked for 8 cores). Please consider re-specifying

The files finished trimming in ~19 hours when run on 8 cores.

I then started looking at the trimmed files. Several files had tons of overrepresented sequences that appeared to be TruSeq index adapters. I wonder if this happened because I manually specified adapters instead of using `--illumina`. In any case, I definitely needed to trim again. I decided to modify my trimming code to include `--stranded_illumina`. When I tried using `--stranded_illumina` I got an error message stating I couldn't use it since it wasn't a valid option? 

> Unknown option: stranded_illumina
Please respecify command line options

So I changed it back to `--illumina`. I figured this should be okay since I've used that option in the past with paired-end WGBS data. I can then compare this output with the previous trimming output to see which code performed better:

echo "Trim by auto-detecting illumina adapters"

```
#Loop through input files in the directory
for input_file in ${DATA_DIR}/*_R1_001.fastq.gz; do
 # Extract the base filename
  filename=$(basename "$input_file")
 # Construct the corresponding R2 filename
  input_r2="${input_file/_R1/_R2}"
  # Run TrimGalore to trim adapters, remove low-quality sequences, and retain sequences of a specific length
    ${TRIMGALORE} \
    --cores 8 \
    --output_dir ${OUTPUT_DIR} \
    --paired \
    --illumina \
    --quality 20 \
    --length 20 \
    --fastqc_args \
    "--outdir ${OUTPUT_DIR} \
    --threads 28" \
    --path_to_cutadapt ${CUTADAPT} \
    $input_file \
    $input_r2
  # Print completion message
  echo "Trimming complete for $filename"
done

#Move trimmed files to a new directory
mkdir ${OUTPUT_DIR}/trim-illumina-polyA
mv ${OUTPUT_DIR}/*trimmed.fq.gz ${OUTPUT_DIR}/trim-illumina-polyA/.
mv ${OUTPUT_DIR}/*trimmed_fastqc* ${OUTPUT_DIR}/trim-illumina-polyA/.
mv ${OUTPUT_DIR}/*fastq.gz_trimming_report.txt ${OUTPUT_DIR}/trim-illumina-polyA/.

echo "Perform MutiQC"

#MultiQC. Move completed MultiQC output to the correct trimming directory
${MULTIQC} \
${OUTPUT_DIR}/trim-illumina-polyA/*

mv ${OUTPUT_DIR}/multiqc_data ${OUTPUT_DIR}/trim-illumina-polyA/.

echo "Triming complete. Check MultiQC output before proceeding."
```

Ended up with some errors with `multiqc` after trimming:

> mv: cannot stat ‘/vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/*trimmed.fq.gz’: No such file or directory

Turns out my output files are named differently!

> >>>>> Now validing the length of the 2 paired-end infiles: 15-033_R1_001_trimmed.fq.gz and 15-033_R2_001_trimmed.fq.gz <<<<<
Writing validated paired-end Read 1 reads to 15-033_R1_001_val_1.fq.gz
Writing validated paired-end Read 2 reads to 15-033_R2_001_val_2.fq.gz

Looking at the [MultiQC report](https://github.com/yaaminiv/wc-green-crab/blob/main/output/06b-trimgalore/trim-illumina/multiqc_report.html), auto-detecting Illumina adapters did MUCH better!! There are no adapter sequences found in any of the trimmed files. I did, however, see a few overrepresented sequences in files. When I looked through the overrepresented sequences, `trimgalore` wasn't able to identify any hits. I decided to move forward with poly A tail trimming.

### Poly A tails

It  appears that I can [add `--poly-a` to remove poly-A tails from paired sequences when using `cutadapt`](https://cutadapt.readthedocs.io/en/stable/recipes.html). [However, `trimgalore` does not have this functionality automatically](https://github.com/FelixKrueger/TrimGalore/issues/180), but perhaps there is an [argument that could be used](https://github.com/FelixKrueger/TrimGalore/issues/97). With `cutadapt`, poly A trimming will be done after adapter trimming. With `trimgalore`, the user will need to manually complete a round of adapter and quality trimming prior to removing poly A tails.

Since I already completed a round of adapter and quality trimming with `trimgalore`, I decided to try the `trimgalore` poly A trimming argument. The `--polyA` argument was experimental in 2020 and supposed to be used with a specific ThermoFisher kit. The code stopped running due to an error, so I parsed through the error report:

> AUTO-DETECTING POLY-A TYPE
===========================
Attempting to auto-detect PolyA type from the first 1 million sequences of the first file (>> /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina/15-033_R1_001_val_
1.fq.gz <<)
Found perfect matches for the following mono-polymer sequences:
Poly-nucleotide type    Count   Sequence        Sequences analysed      Percentage
PolyT   6114    TTTTTTTTTT      1000000 0.61
PolyA   4549    AAAAAAAAAA      1000000 0.45
Using PolyT Polymer for trimming (count: 6114). Second best hit was PolyA (count: 4549)
Writing report to '/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/15-033_R1_001_val_1.fq.gz_trimming_report.txt'

Uh...........it seems like it's auto-detecting polyA and polyT tails in the first read file, and then choosing to go with polyT trimming? I need to do poly A trimming on the first reads and poly T trimming on the second reads, so this is definitely not going to fly. I'll need to specify "AAAAAAAAAA" as the adapter for the first read and "TTTTTTTTTT" as the adapter for the second read for these paired files. There was also an error when it came to finding the files:

> POLY-A TRIMMING MODE; EXPERIMENTAL!!

  >>> Now performing Poly-A trimming for the adapter sequence: 'TTTTTTTTTTTTTTTTTTTT' from file /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina/15-033_R1_001_val_1.fq.gz <<< 
gzip: /scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore//vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina/15-033_R1_001_val_1.fq.gz: No such file or directory

It was able to find the file to auto-detect the poly A type, but then when it came to trimming it was unable to find the file because of some weird replication in the file name. To troubleshoot this, I started by checking the `filename` commands:

```
#Loop through input files in the directory
for input_file in /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina/*_val_1.fq.gz; do
 # Extract the base filename
  filename=$(basename "$input_file")
 # Construct the corresponding R2 filename
  input_r2="${input_file/_val_1/_va1_2}"
  echo "$filename" "$input_file" "$input_r2"
done
```
  
> 5-197_R1_001_val_1.fq.gz 
/vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina/5-197_R1_001_val_1.fq.gz 
/vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina/5-197_R1_001_va1_2.fq.gz

Alright, it seems like the first issue is that my second read should end in `R2_001_val_2.fq.gz` not `_R1_001_va1_2.fq.gz`.

```
for input_file in ${OUTPUT_DIR}/trim-illumina/*_R1_001_val_1.fq.gz; do
 # Extract the base filename
  filename=$(basename "$input_file")
 # Construct the corresponding R2 filename
  input_r2="${input_file/_R1_001_val_1/_R2_001_val_2}"
  echo "$filename" "$input_file" "$input_r2"
done
```

> 5-197_R1_001_val_1.fq.gz 
/vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina/5-197_R1_001_val_1.fq.gz /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina/5-197_R2_001_val_2.fq.gz

That error is now fixed, but it wasn't the issue with the my code to begin with. I wondered if the issue was specific to the experimental poly A trimming argument, so I decided to run this chunk:

```
#Loop through input files in the directory
for input_file in ${OUTPUT_DIR}/trim-illumina/*_R1_001_val_1.fq.gz; do
 # Extract the base filename
  filename=$(basename "$input_file")
 # Construct the corresponding R2 filename
  input_r2="${input_file/_R1_001_val_1/_R2_001_val_2}"
  # Run TrimGalore to trim adapters, remove low-quality sequences, and retain sequences of a specific length
    ${TRIMGALORE} \
    --cores 8 \
    --output_dir ${OUTPUT_DIR} \
    --paired \
    -a "AAAAAAAAAA" \
    -a2 "TTTTTTTTTT" \
    --fastqc_args \
    "--outdir ${OUTPUT_DIR} \
    --threads 28" \
    --path_to_cutadapt ${CUTADAPT} \
    $input_file \
    $input_r2
  # Print completion message
  echo "Trimming complete for $filename"
done
```

Looks like the issue was specific to the poly A trimming argument! Looking back at the issue where I found the poly A argument, I think had I read that issue a little more closely I would have come to that conclusion sooner (but I guess I also got to fix a different naming issue, so all's well that ends well). I checked on the script to ensure that the poly T tail was trimmed for the paired file. Trimming the poly A tails improved some per base sequence content scores, but it didn’t make a huge dent into the overrepresented sequences. I think this is good enough for the first pass analysis I need to complete, so I’m going to move on!

### My side quest

When I first saw the `python` error from MultiQC, I figured oh, let me just run MultiQC on my own machine. WELL TURNS OUT I DON'T HAVE IT ANYMORE?? So here are all the things I did to resolve issue from many angles which took a billion years, so it's really just me getting all of my computers up to date.

- Opened Anaconda Navigator. It needed an update. The update took a million years.
- Update my love, `homebrew`, on my local machine
- Installed `homebrew` on my work computer

Since getting my laptop up to snuff was going to take a while, I also tried to figure out the `python` issue on Poseidon.

### Going forward

1. Preliminary metabolomics analysis
3. Preliminary RNA-Seq analysis
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
