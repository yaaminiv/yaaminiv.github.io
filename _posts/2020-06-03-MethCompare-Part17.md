---
layout: post
comments: true
title: MethCompare Part 17
tags: MethCompare bedtools
---

## Fixing characterization code and remaking barplots

Welp I'm still working on this pipeline! 

### Checking duplicates in `bedtools`

When looking at the CDS track, Hollie noticed that CDS from alternative transcripts are included as individual entries, even if they overlap. Mac suggested that I check my `bedtools` code and confirm that I'm only recording one overlap for each CpG. I looked at [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union.ipynb) to check my CpG genomic location code. I was using `bedtools -wb`, which printed all overlapping regions with the CpG. I only needed to record the CpG location if it overlapped with my feature track once, so I changed it to `bedtools -u` and reran my script. I saved the text files with counts for each species to use downstream.

### Remaking barplots

I used the text files with counts for the number of overlaps between CpGs and genome features to remake my stacked barplots in [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd). Looking at the barplots, they didn't really change that much, but at least it reflects data without any duplicate overlaps.

<img width="981" alt="Screen Shot 2020-06-03 at 2 12 49 PM" src="https://user-images.githubusercontent.com/22335838/83689717-6111d680-a5a4-11ea-87fb-7fcb3409a912.png">

**Figure 1**. Genomic location of CpGs detected by each method.

When Shelly was looking over my code [in this issue](https://github.com/hputnam/Meth_Compare/issues/68) she noticed that MBD-BS in *M. capitata* has the highest percentage of methylated CpGs according to [this table](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union-CpG-Type.txt), but my figure shows WGBS as having more methylated CpGs. While reviewing [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd), I realized that when creating the table, MBD-BS data was in the first row, followed by RRBS and WGBS. Instead, I assumed that WGBS would be in the first row. I really should have been tipped off by the fact that the first row had lower CpG counts since MBD-BS coverage was poorer. I added row names to reduce confusion, then reorganized my rows so that the first was WGBS data, second was RRBS, and third was MBD-BS. I then remade the methylation status barplots so they accurately reflected the data.

<img width="988" alt="Screen Shot 2020-06-03 at 1 55 25 PM" src="https://user-images.githubusercontent.com/22335838/83688430-3de62780-a5a2-11ea-9d84-89a3af806ae9.png">

**Figure 2**. Methylation status of CpGs detected by each method.

I added both figures to [this issue](https://github.com/hputnam/Meth_Compare/issues/72) to keep track of the changes I made and the motivation for doing so. 

### Thinking more about statistics

After doing some research and reviewing multivariate statistics notes, I think our best course of action is to use either an ANOSIM or perMANOVA. We have a multivariate data set-up: one independent variable (sequencing method) and multiple dependent variables (methylation status or genomic location). Using individual sample information, I can assess within-group and between-group differences. I could use a global ANOSIM  or perMANOVA to first see if there are differences between groups. Then I can use pairwise multivariate tests to look at method differences and finally, use post-hoc tests (simper or disper) to understand significant pairwise differences. I posted my thoughts in [this issue](https://github.com/hputnam/Meth_Compare/issues/68) for feedback.

### Updating scripts README

In our last meeting we decided to [clean up the scripts README](https://github.com/hputnam/Meth_Compare/issues/70). I went through and ordered the scripts I created in the order they should be run. I also included brief descriptions about each script. I removed my Jupyter notebook that characterized 10x data, since we're using 5x data instead of 10x! I thought about removing my script for creating figures with indiviudal sample data, but I thought maybe those figures might be useful for the supplement, or I could use the script to explore individual-level multivariate analyses.

### Going forward

1. [Finalize statistical methods for comparison and conduct them](https://github.com/hputnam/Meth_Compare/issues/68)
1. [Locate TE tracks](https://github.com/hputnam/Meth_Compare/issues/56)
2. [Characterize intersections between data and TE, and create summary tables]
4. Look into program for mCpG identification

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
