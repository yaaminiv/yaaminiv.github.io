---
layout: post
comments: true
title: WGBS Analysis Part 33
tags: manchester topGO simplifyEnrichment
---

## Addressing reading committee edits

Now that I have some edits back from Steven, I decided to start addressing the easiest ones in the methods and results.

### Statistical testing

One of Steven's major comments was that it didn't make sense to test the distribution of DML against highly methylated CpGs. Looking back through my manuscript, I realized this test was a holdover from the *C. virginica* paper, in which I used MBD-BSseq and needed to compare my DML with what we believed our library preparation method enriched. In [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.Rmd), I revised the statistical test to look at all CpGs with 5x data vs. DML, since that would be the appropriate CpG background. I found that CpGs were differentially distributed in all categories between those two categories, with higher proportions of DML in genic regions than CpGs with 5x data! I also remade my figure to show this comparison.

![Screen Shot 2021-07-28 at 11 36 52 AM](https://user-images.githubusercontent.com/22335838/128756930-8d9f5c01-faa0-4f54-94f6-216406a10927.png)

**Figure 1**. Proportion of various CpG categories in genome features

### More numbers regarding genes and DML

Going through the edits, I realized I was missing information about how many genes contained DML. In [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.ipynb), I counted the number of unique gene IDs that overlapped with DML. The next thing I did was count the number of genes with multiple DML, and quantify how many DML each gene had. This is something I did for the *C. virginica* paper, but didn't attempt for this paper. To do this, I first extracted the gene IDs that overlap with DML in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.ipynb). Then, I imported the information into [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.Rmd) and counted the number of DML per gene. As expected, most genes had only one DML, and there were genes that contained multiple DML. However, there were several genes that contained > 5 DML, and even one that had 104! That felt suspicious to me, so I checked the gene length. It was a very long gene, so a little less concerning than I originally thought. I saved the DML/gene information in [this spreadsheet](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/10_DML-characterization/Number-of-DML-per-Gene.csv). I also wanted to know how many genic DML overlaped with C/T SNPs, so I obtained the numbers in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.ipynb) using `intersectBed`.

### Removing SNPs from enrichment analysis

Finally, I dealt with all the feedback on the SNP analysis. In [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/07-BS-SNPer.ipynb), I counted the number of unique SNPs identified. Then, I proceeded to remove the C/T SNPs from the set of data going into `topGO` enrichment. To do this, I imported my list of genic DML that overlapped with C/T SNPs into [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/12-Functional-Enrichment.Rmd). I used `left_join` to combine the original annotated dataframe with the SNP overlaps by a `chrStartEnd` column. Then, I filtered rows to keep any instances in which the chromosome column from the SNP dataframe said "NA", since that would mean there was no matching entry in the SNP dataframe:

```
allDMLGOterms <- left_join(x = allDMLGOterms, y = uniqueSNPAll, by = "chrStartEnd") #Add all columns from uniqueSNPAll to allDMLGOterms
allDMLGOtermsFiltered <- filter(allDMLGOterms, is.na(chr.y) == "TRUE")  %>%
  select(!c(chrStartEnd, chr.y, start.y, end.y, meth.diff.y)) #Keep all rows where chr.y has an "NA", since this indicates there was no matching entry in the DML-SNP overlap dataframe. Then drop chrStartEnd, chr.y, start.y, end.y, and meth.diff.y
```

I counted the number of unique genes and DML represented by the revised dataframe, then performed the enrichment analysis. Interestingly, I got very different results! Not only were the enriched biological process terms completely different from what I had previously, but there were also no enriched molecular function terms and enriched cellular component terms. I think differential enrichment results will be a sentence or so in my revised discussion.

<img width="885" alt="Screen Shot 2021-07-28 at 4 13 07 PM" src="https://user-images.githubusercontent.com/22335838/128931522-348380df-c868-4aea-b7ae-6b21dbf09854.png">

**Figure 2**. Revised `simplifyEnrichment` figure

When talking about the SNP analysis with Steven, he brought up an interesting point. Any DML overlaps with C/T SNPs mean that there was a T present, not C. However, `methylKit` requires a CpG site at a locus for all samples! I wasn't sure how to reconcile these two points, and since no one looked over my SNP analysis to begin with, I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1254). I'll certainly want to look over the SNP analysis before publication.

### Going forward

1. Look over SNP analysis
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
