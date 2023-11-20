---
layout: post
comments: true
title: Green Crab Experiment Part 30
tags: green-crab metabolomics
---

## Making a plan for metabolomics analysis

### Analytical workflow

Before I jumped into analysis, I surveyed some recent metabolomics papers to see if I could come up with a consensus on analytical method:

[Huffmyer et al. (2023)](https://doi.org/10.1101/2023.03.20.533475):

- PLS-DA in `mixOmics` package
- WCNA with WGCNA correlated to identify with coral developmental time points
- analyzed for enrichment of compound class and KEGG pathway with MetaboAnalyst web interface

[Williams et al. (2021)](https:/doi.org/10.1126/sciadv.abd4210):

- DGCA for correlation of metabolite pairs between ambient and stressed Mcap
- Significant correlations —> used to create co-occurrence networks

[Noisette et al. (2021)](https://doi.org/10.3390/metabo11090584):

- PCA then PLS-DA
- VIP identified from PLS-DA
- ANOVA for differences in metabolites between groups
- functional enrichment and pathway topology analysis (takes into account role of metabolite, position, and direction of interaction in a network)
- MetaboAnalyst used for all analyses

[Trigg et al. (2019)](https://doi.org/10.1038/s41598-019-46947-6):

- All analyses in R except for PLS-DA and random forest analyses in MetaboAnalyst
- Remove compounds detected in less than half of crabs prior to downstream analyses
- univariate analyses: two-way ANOVA for each compound (OA + DO), Benjamini-Hochberg FDR correction. sig compounds (model ANOVA < 0.01 and effect < 0.05 w/o FDR correction) used for pathway analyses
- multivariate: normalized by mean-centering and scaling, PLS-DA with default settings
- Metamapp: map metabolite relationships

Looks like a PLS-DA and some sort of MetaboAnalyst enrichment are common place, so I'll start there! Huffmyer et al. (2023) has the most recent scripts available on Github, so I'll base a lot of my work off of that.

### Going forward

1. Analyze metabolomics data

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
