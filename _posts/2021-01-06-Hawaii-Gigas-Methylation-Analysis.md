---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis
tags: hawaii gigas-ploidy mox trimgalore
---

## Trimming Hawaii data

I'm starting data analysis for a new(-ish) project! [Dr. Maria Haws at the University of Hawai'i Hilo exposed diploid and triploid adult *C. gigas* to low or ambient pH in a factorial design for six weeks and dissected ctenidia tissue](https://github.com/RobertsLab/project-oyster-oa#changes-in-dna-methylation-in-diploid--triploid-crassostrea-gigas-exposed-to-different-ph-levels-id-haws). Sam extracted DNA from these tissues for WGBS at Zymo. Once he got the [sequencing data](https://docs.google.com/spreadsheets/d/1_XqIOPVHSBVGscnjzDSWUeRL7HUHXfaHxVzec-I-8Xk/edit#gid=0), [he ran FastQC](https://robertslab.github.io/sams-notebook/2020/12/06/FastQC-MultiQc-C.gigas-Ploidy-pH-WGBS-Raw-Sequence-Data-from-Haws-Lab-on-Mox.html). My goal is to identify differentially methylated loci associated with ploidy and/or ocean acidification in these samples.

To start, I wanted to trim data with `trimgalore` on `mox`. Even though `fastp` works faster, I wanted to use `trimgalore` because that's what we used in the [coral methylation comparison pipeline](https://github.com/hputnam/Meth_Compare/blob/master/code/00.01-DNA-sequence-processing.md). I started by downloading the sequence data from `nightingales` into the `gscratch/scrubbed/yaaminiv` directory on `mox`:

```
wget -r -l1 —no-parent -A “zr3644*gz” http://owl.fish.washington.edu/nightingales/C_gigas/ #Obtain sequencing data
wget http://owl.fish.washington.edu/nightingales/C_gigas/zr3644_MD5.txt #Obtain md5 information
md5sum -c zr3644_MD5.txt #Check md5sum
```

I copied the [`mox` script we used to trim coral samples](https://github.com/hputnam/Meth_Compare/blob/master/code/00.01-DNA-sequence-processing.md#quality-trimming) and pasted it into [a new shell script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/01-trimgalore.sh). For the paired WGBS data, I need to hard trim 10 bp from each end, so I made sure the script did that. I added all the paths for the paired data into the script, then transferred it to `mox` and ran it:

```
rsync —archive —progress —verbose yaamini@172.25.149.226:/Users/yaamini/Documents/project-oyster-oa/code/Haws/01-trimgalore.sh . #Transfer script to USER directory, not scrubbed directory
sbatch 01-trimgalore.sh
```

It ran...without actually trimming anything! Since there were no trimmed files, `fastqc` and `multiqc` didn't run either! When I checked the program paths, I noticed that path to `cutadapt` didn't lead to the program. I checked the shared `srlab/programs` folders, but couldn't find anything, so I decided to install `cutadapt` myself. When I brought this up at lab meeting, Laura mentioned that she had gone through the same process earlier. According to Sam, the way the `cutadapt` program works means that it can't be installed at a shared location — every user has to install it for themselves. I followed the clear steps Laura laid out in [this issue](https://github.com/RobertsLab/resources/issues/1053) to install `cutadapt` and add `fastqc` to my path. I then transferred the revised script to `mox` to run! The files are trimming, so now I just need to wait.

### Going forward

1. Look at FastQC and MultiQC output
2. Edit coral methylation scripts to fit data
3. Run `bismark` pipeline

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
