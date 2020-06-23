---
layout: post
comments: true
title: MethCompare Part 21
tags: MethCompare glm perMANOVA PCoA
---

## Refining multivariate and modeling approaches

During our meeting Friday, I realized I interpreted the data analysis task incorrectly. Instead of combining CpG methylation status and genomic location information to use for multivariate and modeling approaches, I needed to consider the data separately!

### Remaking *M. capitata* tracks

Before I could revise my analysis, Hollie pointed out that the *M. capitata* gene track did not have the correct amount of genes [in this issue](https://github.com/hputnam/Meth_Compare/issues/75). In addition to using genes with the AUGUSTUS tag, I needed to pull genes that had the GeMoMa tag (from a different related species). In [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Generating-Genome-Feature-Tracks.ipynb), I matched multiple patterns with `grep` to revise the gene, CDS, and intron tracks.

```
#Match both AUGUSTUS and GeMoMa gene annotations
!grep -e "AUGUSTUS	gene" -e "GeMoMa	gene" Mcap.GFFannotation.gff > Mcap.GFFannotation.gene.gff
```

I used these revised tracks to create my three flank tracks and intergenic region track. Since the files have the same name, I didn't need to update my IGV sessions. Instead, I uploaded the revised tracks to `gannet` so they would populate IGV.

### Re-running scripts

Now that I had revised tracks, I needed to go back to my Jupyter notebook and characterize *M. capitata* feature overlaps! In [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x.ipynb) I characterize CG motif overlaps with the revised tracks. Then, I characterized overlaps between individual samples and feature tracks. I did the same thing in [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union.ipynb) for 5x union data. For the union data, I recreated CpG genomic location stacked barplots with [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd).

![Screen Shot 2020-06-22 at 9 52 49 AM](https://user-images.githubusercontent.com/22335838/85314289-234ef200-b46e-11ea-958d-671e84644514.png)

**Figure 1**. *CpG genomic location.

Looking at my output, it no longer looks like MBD-BS is no longer the best for capturing gene regions. Since I ran this code quickly and late at night, I need to double check that this stacked barplot is trustworthy.

### Revised statistical analyses

Now, the big stuff. I took the output for individual samples and recreated my summary tables in [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.Rmd). Based on feedback from Friday, I ran individual NMDS, ANOSIM, and model sets for the CpG methylation status and genomic location datasets. Similar to the methylation status models, I ran individual models for each genome feature since all the proportions add up to 1. I included replicate information as a random effect.

I ran my analyses and questions by Evan, and he suggested I switch over from NMDS and ANOSIM to PCoA and perMANOVA. Apparently, the creators of the `vegan` package recommend this. The perMANOVA has a more robust statistical output format as well. I kept my centered log-ratio transformation and euclidean distance matrix, but fed it into `cmdscale` to run a PCoA instead. I investigated the PCs, eigenvalues, and loadings. I then used `adonis` to run a global perMANOVA. To understand if a significant perMANOVA result was due to centroid or variance differences between groups, I ran a beta dispersion model. A non-significant result indicates that differences are due to centroids, not variance. After my global perMANOVA, I ran pairwise perMANOVA test to understand the significant global perMANOVA result. Similar to when I was running ANOSIM, I ended up with complete enumeration. However, it was easier to interpret the perMANOVA output and see how much variance the test explained. After running my regressions, I extracted p-values and corrected them using a false discovery rate.

Although most of my questions are addressed, I still have two more. When running the beta dispersion model for my *P. acuta* data, I found that group variances were significantly different from eachother! My guess is that sample 8 is an influential point or outlier that is contributing to variance differences between groups. I need to investigate this further. While I didn't need to make any changes to the models (so nice!), the models are set up in a way that compares WGBS to RRBS and MBD-BS and not compare the two enrichment methods. I'm not sure which post-hoc tests I need to use to compare these two methods. I knit the output into [this document](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.md), emailed Evan, and posted my follow-up questions [here](https://github.com/hputnam/Meth_Compare/issues/68). The end is (hopefully) near!

### Going forward

1. [Finalize analysis](https://github.com/hputnam/Meth_Compare/issues/68)
2. Check that union stacked barplots were generated correctly
3. Update methods
4. Update results
5. Update figures
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
  
