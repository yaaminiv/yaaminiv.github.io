---
layout: post
comments: true
title: MEPS Revisions Part 1
tags: DNR SRM MEPS-Revisions
---

## Revisiting NMDS for protein abundance data

According to Github it's been exactly a year since I last touched my SRM data :0 But now I'm back and remaking NMDS plots! For the MEPS revision, reviewers have asked me to complete an analysis of the environmental data, and include the environmental data when looking at differential protein abundance. To do this, I'm going to conduct a constrained ordination analysis, in which I examine my protein abundance data, while constraining it for environmental variables in each site and habitat. Julian suggested I revisit my protein abundance data before I proceed with the constrained analysis, so here I am.

In this [R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-ANOSIM-for-Cluster-Analysis.R) (side note: can you believe I ever used plain R and not R Markdown?! I cannot wait to convert this script into an R Markdown file) I took my protein abundance data and first transformed it using a Hellinger's transformation. The idea here is to control for any rare peptides or high instances of 0s by downweighting them in the analysis. Then, I used a euclidean distance matrix on the transformed data. Finally, I ordinated the data with an NMDS. I performed either a one- or two-way ANOSIM to assess the significance of groupings

### Site and Habitat

For this ANOSIM, the R statistic was 0.088, meaning that within group and between group similarities were equal (p = 0.073). There is no evidence to refute the null hypothesis that site and habitat groupings exist.

### Habitat only

![screen shot 2018-11-14 at 4 30 56 pm](https://user-images.githubusercontent.com/22335838/48522126-4940f900-e82c-11e8-80bb-dbf83e1faff1.png)

**Figure 1**. Ordination of protein abundance data looking solely at habitat type. Confidence ellipses are used to demonstrate overlaps or segregation of data.

Once again, within- and between-group similarities between bare and eelgrass habitats were similar (R = 0.044, p = 0.122).

### Site only

![screen shot 2018-11-14 at 4 30 47 pm](https://user-images.githubusercontent.com/22335838/48522258-de43f200-e82c-11e8-9128-6fd786423aef.png)

![screen shot 2018-11-14 at 4 31 32 pm](https://user-images.githubusercontent.com/22335838/48522259-de43f200-e82c-11e8-979a-b9da76135371.png)

**Figures 2-3**. Ordination of protein abundance data looking solely at site designation. Oyster sample IDs for each site are either bounded by a polygon (top), or confidence ellipses are used to demonstrate overlapping nature of data (bottom). Both the polygon and confidence ellipse for Willapa Bay are slightly segregated from the other four sites.

Within- and between-group similarities between all five sites were similar (R = 0.064, p = 0.065). However, when I repeated the ANOSIM with region designation (Puget Sound vs. Willapa Bay), there was mild evidence for regional differences (R = 0.226, p = 0.053).

### Loadings

I correlated the NMDS scores with the original data matrix to obtain loadings, then plotted only those that were significant at the 0.001 level. Turns out that's a lot of peptides. 

![screen shot 2018-11-14 at 4 30 39 pm](https://user-images.githubusercontent.com/22335838/48522403-6f1acd80-e82d-11e8-9cee-194efb05dd17.png)

**Figure 4**. Peptide loadings significant at the 0.001 level.

I'll need to revise the significance criteria to see if I can plot less loadings up there.

### Going forward

1. Complete ordination analysis of environmental data
2. Meet with Julian to discuss unconstrained ordination results
3. Plan for constrained ordination approach

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
