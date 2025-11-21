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


I'd just need to install `salmon` separately.

- install trinity
- install BOWTIE2
- install salmon
- install jellyfish

### Draft code

While the terminal was doing its thing I figured I would read the `trinity` manual to determine what parameters to use. Ideally, I was looking for a way to run `trinity` on a subset of my data to determine the optimum parameters, or figure out if `trinity` produced multiple potential transcriptomes that I could evaluate later. I also noticed that `trinity` [could be run in chunks to compensate for run time restrictions](https://github.com/trinityrnaseq/trinityrnaseq/wiki/Running-Trinity#running-trinity-in-multiple-sequential-stages).

The first thing that jumped out to me is that I could provide a file list to `trinity`, instead of pasting file names separated by commas. I also need to specify what each file in a pair is (forward and reverse). Cool, but I didn't see anything about running the program on a subset of data or trying multiple parameters.


### Going forward

1. Transcriptome assembly with `trinity`
4. Clean transcriptome with `EnTap` and `blastn`
5. Quantify transcript counts with `salmon`
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
