---
layout: post
comments: true
title: WGBS Analysis Part 31
tags: manchester gigas-broodstock methylKit bedtools topGO
---

## Revising `methylKit` parameters

Before I defended, I wanted to revise my `methylKit` code to test all low and ambient pH samples together, instead of separating female and indeterminate samples for analysis. I can justify including the indeterminate samples with the female samples since they may mature into female oysters, and I'm anyways using maturation stage as a covariate to account for any stage-specific differences.

### 4 vs. 4 test

I returned to the R Studio Server to run `methylKit`, then transferred the code to [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.Rmd). Using a q-value of 0.01, I identified 12,826 DML with a 25% difference, 1,599 DML with a 50% difference, and 59 DML with a 75% difference. When I opened this [IGV session](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/10_DML-characterization/dml.xml) to spot-check the DML, the 50% difference looked very reasonable.

![Screen Shot 2021-06-16 at 11 07 13 AM](https://user-images.githubusercontent.com/22335838/124641326-6f838f00-de43-11eb-8b42-d3e28ea1f488.png)

![Screen Shot 2021-06-16 at 11 07 44 AM](https://user-images.githubusercontent.com/22335838/124641333-714d5280-de43-11eb-9757-4a817da49708.png)

![Screen Shot 2021-06-16 at 11 08 22 AM](https://user-images.githubusercontent.com/22335838/124641336-71e5e900-de43-11eb-8208-20d7df60535e.png)

**Figures 1-3**. IGV screenshots for DML with a 50% difference between treatments.

Even though 1,599 is way more DML that I've identified previously, I decided to stick with these parameters! I then updated the figures that used the `methylKit` data. First, I wanted to include maturation stage information in the PCA to demonstrate that samples did not cluster together by maturation stage.

![Screen Shot 2021-06-16 at 2 40 22 PM](https://user-images.githubusercontent.com/22335838/124641454-93df6b80-de43-11eb-85f8-09a637ea8ba4.png)

**Figure 4**. [Global methylation PCA with stage-specific information](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/06-methylKit/figures/all-sample-PCA.pdf)

Next, I wanted to see if DML that overlapped with SNPs clustered together in my heatmap. [When I added an asterisk to each row that was a SNP-DML overlap, I didn't see any clustering](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/06-methylKit/figures/all-heatmap-SNPdesignation.pdf). I decided to produce one heatmap for all DML identified, instead of splitting it by SNP-DML overlaps and unique DML.

![Screen Shot 2021-07-06 at 10 23 19 AM](https://user-images.githubusercontent.com/22335838/124642541-cfc70080-de44-11eb-8338-4509b9e0eed0.png)

**Figure 5**. [Heatmap of DML](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/06-methylKit/figures/all-heatmap.pdf)

Finally, I plotted the distribution of DML in various chromosomes against the number of genes in that chromosome. The number of DML seemed to track the number of genes.

![Screen Shot 2021-07-06 at 10 23 38 AM](https://user-images.githubusercontent.com/22335838/124642697-fbe28180-de44-11eb-95fc-578773d10d7f.png)

**Figure 6**. [Chromosomal distribution of DML](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/06-methylKit/figures/DML-chr-distribution.pdf)

### Genomic location of revised DML

I took my revised DML list and used it in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.ipynb) to characterize the genomic location of DML. The majority of DML were found in genes, with more in introns than CDS or exon UTR. Unique C/T SNPs overlapped with ~20% of DML. I then updated my statistical tests and figures in [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.Rmd). The proportions of DML in CDS, introns, downstream flanks, lncRNA, and intergenic regions were significantly different than that of highly methylated CpGs in those same regions.

![Screen Shot 2021-07-06 at 10 34 32 AM](https://user-images.githubusercontent.com/22335838/124643845-6ba53c00-de46-11eb-9f13-a25436bc0bc4.png)

**Figure 7**. [Genomic location of highly methylated CpGs and DML](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/10_DML-characterization/figures/DML-feature-overlaps.pdf)

### Revisiting `topGO` enrichment

Now that I had a revised DML list, I wanted to try enrichment again. I annotated my DML list in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/11-GOterm-Annotation.ipynb), I opened [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/12-Functional-Enrichment.Rmd). Prior to enrichment, I created [this table](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/12-functional-enrichment/pH-BP-GOterms.csv) with each GOterm associated with genes containing DML, and its frequency in my dataset. I used this information to create a REVIGO figure to visualize the biological processes associated with all DML for my defense slides.

![Screen Shot 2021-07-06 at 10 45 49 AM](https://user-images.githubusercontent.com/22335838/124644642-5bda2780-de47-11eb-9d8f-dd679833ee3a.png)

**Figure 8**. All biological process GOterms associated with DML in genes

I then proceeded with a Fisher's Exact test for enrichment with `topGO`, and found enriched [biological process](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/12-functional-enrichment/all-BP-FisherTestResults.csv) and [molecular function](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/12-functional-enrichment/all-MF-FisherTestResults.csv) GOterms with this dataset! After I defend, I want to find a way to visualize this result.

### Going forward

1. Visualize enrichment results
2. Report `mc.cores` issue to [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html)
2. Perform randomization test
2. Update `mox` handbook with R information
3. Determine if larval DNA/RNA should be extracted

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
