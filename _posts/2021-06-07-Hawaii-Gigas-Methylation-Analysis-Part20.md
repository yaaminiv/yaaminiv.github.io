---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 20
tags: hawaii gigas-ploidy
---

## Figures and statistical analyses

It's your favorite time and mine: figures and stats! These are the last things I need to finalize my existing methods and results sections. Afterwards, I'll work  on an enrichment analysis and dive into the discussion.

### [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html)

I opened [this R Markdown script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit.Rmd) to make figures from the `methylKit` data. First, I imported coverage information I made in [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit.Rmd). Then, I calculated the number of loci with higher or lower coverage in triploids:

```{r}
sum(as.numeric(as.character(ploidyCoverage$diploid)) < as.numeric(as.character(ploidyCoverage$triploid)), na.rm = TRUE) #5395386 loci with higher coverage in triploids
sum(as.numeric(as.character(ploidyCoverage$diploid)) >= as.numeric(as.character(ploidyCoverage$triploid)), na.rm = TRUE) #6956658 loci with lower or equal coverage in triploids
```

I then created histograms with coverage distributions for [diploids](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/figures/diploid-coverage-distribution.pdf) and [triploids](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/figures/triploid-coverage-distribution.pdf). The plots themselves aren't extremely informative because most of the loci are within the first histogram bin. In the future, I need to create a gapped axis. I did something similar and calculated the number of loci with higher or lower methylation in triploids:

```{r}
sum(as.numeric(as.character(ploidyMethylation$diploid)) < as.numeric(as.character(ploidyMethylation$triploid)), na.rm = TRUE) #2735188 loci with higher methylation in triploids
sum(as.numeric(as.character(ploidyMethylation$diploid)) >= as.numeric(as.character(ploidyMethylation$triploid)), na.rm = TRUE) #6605916 loci with lower or equal coverage in triploids
```

I created frequency distributions for [diploids](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/figures/diploid-frequency-distribution.pdf) and [triploids](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/figures/triploid-frequency-distribution.pdf), made a PCA of global methylation information, then created a multipanel plot:

![Screen Shot 2021-06-08 at 1 45 24 PM](https://user-images.githubusercontent.com/22335838/125129655-a44a4d00-e0b4-11eb-931c-2293f71feed2.png)

**Figure 1**. Average percent methylation and coverage for diploids and triploids, and PCA of global methylation profiles

### `DSS`

Next, I opened this [R Markdown script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-DSS.Rmd#L175) to make heatmaps for each DML category. I merged the DML location information with the union 1x bedgraph to get sample percent methylation for each DML, then used that in my heatmaps. When I looked at the figures, I didn't see any clear differential methylation signal between treatments or ploidy. Something to look into...

I also created a figure with the distribution of DML in chromosomes, normalized by the number of CpGs in each chromosome. I also included a line tracking the number of genes in each chromosome. I saved all of the individual plots in [this folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_04-DSS/figures). I combined this figure with the three heatmaps in an InDesign document to create a multipanel plot:

<img width="1087" alt="Screen Shot 2021-06-09 at 12 00 48 AM" src="https://user-images.githubusercontent.com/22335838/125129659-a6aca700-e0b4-11eb-93f5-27ae5891e549.png">

**Figure 2**. Heatmaps for different DML categories, and chromosomal distribution of DML

### Methylation landscape

I delved into understanding the methylation landscape in [this R Markdown document](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/06-General-Methylation-Landscape.Rmd). I performed a chi-squared test to understand the difference in CpG distribution in the genome between all CpGs in the genome and highly methylated CpGs, and found that the proportion of CpGs in exon UTR was significantly different between the genome and highly methylated CpGs. I created a stacked barplot to visualize the distributions of all CpGs in the genome, highly methylated CpGs, moderately methylated CpGs, and lowly methylated CpGs.

![Screen Shot 2021-06-08 at 4 03 42 PM](https://user-images.githubusercontent.com/22335838/125129658-a6141080-e0b4-11eb-8fe2-5f44bde8e3df.png)

**Figure 3**. Proportion of CpGs in various genome features

### Genomic location of DML

I did something similar with the location of DML in the genome in [this R Markdown script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/07-Genomic-Location-of-DML.Rmd)! I performed chi-squared tests for each DML category against highly methylated CpGs and saved the output ([ploidy](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_07-DML-characterization/CpG-location-statResults-ploidy.txt), [pH](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_07-DML-characterization/CpG-location-statResults-pH.txt), and [ploidy:pH](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_07-DML-characterization/CpG-location-statResults-ploidypH.txt)). I created a stacked barplot with information for each DML category and highly methylated CpGs as well.

<img width="921" alt="Screen Shot 2021-06-09 at 12 46 05 AM" src="https://user-images.githubusercontent.com/22335838/125129660-a7453d80-e0b4-11eb-867b-0c0f44c1d6d5.png">

**Figure 4**. Proportion of DML in various genome features

Next steps: finishing analyses and updating the manuscript.

### Going forward

1. Perform enrichment
6. Update methods
7. Update results
5. Outline discussion
6. Write discussion
7. Write introduction
2. Conduct randomization test with `DSS`
2. Try [EpiDiverse/snp](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)


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
