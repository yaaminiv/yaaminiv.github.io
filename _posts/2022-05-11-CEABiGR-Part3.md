---
layout: post
comments: true
title: CEABiGR
tags: virginica CEABiGR
---

## Methylation landscape analysis

We're updating foundational methods and results for CEABiGR! I'm working on the methylation analysis section, and decided to do my standard methylation landscape characterization for the data we have. I'm going to characterize the methylation landscape for male and female samples separately, since we're seeing sex-specific methylation and gene expression patterns.

### Revising genome feature tracks

But first...I revised the *C. virginica* genome feature tracks. I made the original genome feature tracks in 2018, but I didn't make it such that a feature was only included in one category. For example, there are overlaps between flanking regions and intergenic regions, and I think I only used Gnomon annotations. I created [this Jupyter notebook](https://github.com/sr320/ceabigr/blob/main/code/Generating-Genome-Feature-Tracks.ipynb) to update the way I created *C. virginica* genome feature tracks. I also pulled the RepeatMasker output from NCBI itself, instead of using the version created by Sam. I re-created the CG motif track as well so the creation of all feature tracks were in one notebook, and counted the overlap between CG motifs and all genome feature tracks.

**Table 1**. Number of genome features and overlaps with CG motifs

|       **Feature**      | **Number of Unique Features** | **Overlaps with CG Motifs** |
|:----------------------:|:-----------------------------:|:---------------------------:|
|      **CG Motifs**     |           14,458,736          |             N/A             |
|        **Genes**       |             38,838            |          7,778,105          |
|         **CDS**        |            645,368            |          1,728,303          |
|        **Exon**        |            731,916            |          2,334,303          |
|        **mRNA**        |             60,201            |          7,507,167          |
|       **lncRNA**       |             4,750             |           281,715           |
|       **Non-CDS**      |            337,305            |          12,138,514         |
|       **Intron**       |            311,341            |          5,497,597          |
|      **Exon UTR**      |            183,389            |           606,308           |
|   **Upstream Flanks**  |             34,817            |           694,265           |
|  **Downstream Flanks** |             35,224            |           616,684           |
| **Intergenic Regions** |             23,949            |          5,417,334          |
|         **TE**         |            344,267            |           611,471           |

### Methylation landscape

In [this Jupyter notebook](https://github.com/sr320/ceabigr/blob/main/code/General-Methylation-Landscape.ipynb), I created union bedGraphs for males and females separately. I kept my code for the all-sample union bedGraph since I can't remember if Katherine used it for any of her analyses. Once I had the union bedGraphs, I counted the number of highly, sparsely, and lowly methylated CpGs in each sample. I also counted the CpGs present in each genomic feature. I created [this R Markdown script](https://github.com/sr320/ceabigr/blob/main/code/General-Methylation-Landscape.Rmd) to create figures and perform chi-squared tests comparing the distribution of CpGs in the *C. virginica* genome with highly methylated CpGs. As expected, the distribution was significantly different. All output can be found in [this `gannet` folder](https://gannet.fish.washington.edu/seashell/ceabigr/output/methylation-landscape/), and the relevant count files, statistical output, and figures can be found [on Github](https://github.com/sr320/ceabigr/tree/main/output/methylation-landscape).

**Figures 1-3**. Genome feature overlaps for all 10x CpGs with data in at least one sample, highly methylated, moderately methylated, and lowly methylated CpGs for female and male union bedGraphs

![Screen Shot 2022-05-18 at 4 53 00 PM](https://user-images.githubusercontent.com/22335838/169154300-642953c1-0043-4e5e-9853-ae269f17d91c.png)

![Screen Shot 2022-05-18 at 4 53 06 PM](https://user-images.githubusercontent.com/22335838/169154303-a9878996-4b58-4f75-b4b7-52d0d21b98f7.png)

![Screen Shot 2022-05-18 at 4 53 56 PM](https://user-images.githubusercontent.com/22335838/169154305-dd75ff15-5701-43e2-a3b2-ff2ad3bf9a57.png)


### Going forward

1. Update foundation methods
2. Update foundation results
3. Revise with new expression data from Ariana
2. Tune sPLS parameters
3. Run sex-specific SPLS
4. Identify drivers

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
