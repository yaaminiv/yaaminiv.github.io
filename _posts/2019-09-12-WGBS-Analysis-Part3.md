---
layout: post
comments: true
title: WGBS Analysis Part 3
tags: manchester gigas-broodstock WGBS mox bismark methylKit
---

## Rerunning `bismark` and starting with `methylKit`

### My `bismark` headache

[After Mox maintenance ended](https://yaaminiv.github.io/WGBS-Analysis-Part2/), I ran [this script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/scripts/2019-09-03-Bismark.sh) and hoped for the best. Somehow, only my ambient file was processed! I tried running just the alignment for my low pH sample with [this script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/scripts/2019-09-11-Bismark-YRVL-Align.sh) and got this error:

```
Use of uninitialized value $path_to_bowtie in concatenation (.) or string at /gscratch/srlab/programs/Bismark-0.21.0/bismark line 6893.
Failed to execute Bowtie 2 porperly (return code of 'bowtie2 --version' was 32512). Please install Bowtie 2 or HISAT2 first and make sure it is in the PATH, or specify the path to the Bowtie 2 with --path_to_bowtie2 /path/to/bowtie2, or --path_to_hisat2 /path/to/hisat2
```

Stuck after changing `path_to_bowtie` to `path_to_bowtie2` and not getting the script to run, I reopened [this issue](https://github.com/RobertsLab/resources/issues/742)/. Steven had me add the following line of code to the top of my script:

```
source /gscratch/srlab/programs/scripts/paths.sh
```

Apparently it does something with the `bash` paths set on Mox such that the paths in my script are recognized. I was able to align, deduplicate, sort, and index the low pH file!...but I couldn't get `bismark2summary` to work:

```
It looks like there is a mix of samples, e.g. RRBS as well as WGBS, in this folder. Please consider running bismark2summary only on samples of the same data type,
or specify the input files manually (--help for more information). Extiting...
```

For some reason, `bismark` thinks that my files were made with different sequencing methods. I created [this script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/scripts/2019-09-12-Bismark-bismark2summary.sh) and manually set the files I wanted the command to use. Even then, I still got the error! I posted [this issue](https://github.com/RobertsLab/resources/issues/744) because I was concerned about the file formats. Steven suggested I reformat my scripts and process the files together.

I created a `2019-09-11-Gigas-Bismark` directory and started running [this script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/scripts/2019-09-12-Bismark.sh) to hopefully process both files together. In the meantime, I transferred all files I've generated to start testing out `methylKit`. I used this code to transfer the files:

```
rsync --archive --progress --verbose /gscratch/scrubbed/yaamini/analyses/Gigas-WGBS/2019-09-03-Gigas-Bismark/* yaamini@172.25.149.226:/Volumes/web/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-03-Bismark
```

### Trying `methylKit`

Even though my final bam files may not be identical to the ones I transferred from Mox, I wanted to use them to test out the [`methylKit` pipeline I generated for *C. virginica*](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd). I created a new R project and [this R Markdown script](), changed all the file names, and started processing files. I decided to keep the same settings I used for *C. virgincia*: 

- 5x coverage; generated using `processBismarkAln` with `mincov = 2` to quickly process my files, then using `filterByCoverage` to set minimum and maximum coverage thresholds (5 and 100)
- `destrand = TRUE` to separate forward and reverse strands

I started `processBismarkAln` and it's chugging along. Here's hoping I can solidify the pipeline and get data so I can make a presentation!

**Update 2019-09-12 10:45 p.m.: I have over 15,000 DML! Relevant files below:**

All DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-5x-Locations.bed); [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-50-Cov5.csv)
Hypermethylated DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-5x-Locations-Hypermethylated.bed); [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-50-Cov5-Hypermethylated.csv)
Hypomethylated DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-5x-Locations-Hypomethylated.bed); [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-50-Cov5-Hypomethylated.csv)

I will check these in IGV before proceeding with analyses.

### Going forward

2. Get revised `bismark` output from Mox and run through analysis pipeline
3. Verify DML in IGV
3. Obtain *C. gigas* feature tracks
4. Characterize locations of DML
5. Conduct a gene enrichment and/or identify genes with DML and discern what it means

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
