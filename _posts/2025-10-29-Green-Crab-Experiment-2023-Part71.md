---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 71
tags: green-crab-wc TTR RNA-Seq fastqc
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

### TrimGalore!

It's time to trim! I decided to follow the methods from Zach's paper (Tobias et al. (2021)) since he also had stranded data and had to do de novo transcriptome assembly. Looking through his methods, he states that all of the quality control was done in a previous study, Tepolt et al. (2020). When I looked in the methods of that paper, I didn't see any specific information on trimming. I did, however, find this information in the supplement:

> The resulting raw sequences were cleaned and trimmed using Trim Galore! v0.4.0 (http://www.bioinformatics.babraham.ac.uk/projects/trim_galore/), a wrapper for Cutadapt (Martin, 2011). A quality cutoff of Phred ≥ 20 was used, and reads were only retained if they were ≥ 20 bp long after adapter removal and quality trimming.

Based on this, it seems like my trimming protocol is as follows:

1. Trim adapter sequences
2. Trim low quality reads (Phred ≥ 20)
3. Retain reads ≥ 20 bp long

NEBNext UltraExpress RNA kit sequenced on NovaSeq X Plus 25B lane.
Included 1% PhiX spike-in
Got demulitplexed raw data delivered
Do I need to remove the polyA tails?

find adaptor sequence from FAQ

https://www.neb.com/en-us/faqs/2021/01/15/what-sequences-need-to-be-trimmed-for-nebnext-libraries-that-are-sequenced-on-an-illumina-instrument?srsltid=AfmBOoqGuWAevn1loT1LjEgaLQSx7xG-7ONZ2ZjSmipUJjW9fuiGpUhh

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
