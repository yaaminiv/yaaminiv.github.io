---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 23
tags: hawaii gigas-ploidy methylKit
---

## Revising figures and numbers in text

Welp...back at this paper again! We have a special issue deadline of April 18, so this time I actually have to finish things up. I started by picking up where I left off in [this poorly written lab notebook post](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part22/).

### Hunting down numbers and contingency tests

The first thing I wanted to do was update the manuscript with numbers from my `methylKit` analyses. I referred to [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/07-Genomic-Location-of-DML.ipynb) to get the total number of DML found for pH and ploidy contrasts, and the number of DML that overlapped with various genome features. In this process, I realized I needed to remake some figures. I remade the figure showing the number of 5x CpGs across the genome in various methylation categories in [this R Markdown script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/06-General-Methylation-Landscape.Rmd).

<img width="1139" alt="Image" src="https://github.com/user-attachments/assets/702d6baa-e43b-4c5d-aceb-1ea651a8116a" />

**Figure 1**. Distribution of all 5x CpGs, highly methylated CpGs, moderately methylated CpGs, and lowly methylated CpGs across various genome features.

I then made a similar figure showing the distribution of ploidy-DML and pH-DML across various genome features in [this R Markdown script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/07-Genomic-Location-of-DML.Rmd).

<img width="1139" alt="Image" src="https://github.com/user-attachments/assets/423f3d08-9144-4c99-b1ff-de5ceb97861a" />

**Figure 2**. Distribution of ploidy-DML and pH-DML in various genome features

In the same R Markdown script, I used a contingency test to understand if the distribution of ploidy-DML or pH-DML were significantly different from all 5x CpGs in the *C. gigas* genome. Interestingly, only the [distribution of pH-DML in intergenic regions was significantly different](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_07-DML-characterization/CpG-location-statResults-pH.txt). The [distribution of ploidy-DML was not significantly different for any genomic feature](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_07-DML-characterization/CpG-location-statResults-ploidy.txt).

### Heatmaps

The last thing I wanted to do was remake heatmaps for my DML. I opened [this R Markdown script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit.Rmd) to make heatmaps using `pheatmap`.

<img width="1162" alt="Image" src="https://github.com/user-attachments/assets/4833c745-061e-4d7e-85e8-73198358308d" />

<img width="1162" alt="Image" src="https://github.com/user-attachments/assets/651e8d5c-4dec-40d4-a053-cd0a216196f4" />

**Figures 3-4**. Heatmap of ploidy-DML and pH-DML

So.......these don't inspire any confidence in the DML identification. I think there are two things at play here:

- Using a minimum of eight samples/locus
- Holding pH as a covariate when examining ploidy-DML, and vice versa

I created [this issue](https://github.com/RobertsLab/project-oyster-oa/issues/42) and asked Steven to examine the DML in IGV. If we don't have any confidence, then I think the next step is to redo `methylKit` without the covariate to see if that improves our confidence in the DML.

### Going forward

1. Review `methylKit` results
2. Examine distribution across chromosomes
5. Add Rajan's comments to the Google Doc
1. Update methods
7. Update results
5. Revise discussion
7. Revise introduction
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
