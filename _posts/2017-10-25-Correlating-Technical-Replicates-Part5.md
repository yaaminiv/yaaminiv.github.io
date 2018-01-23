---
layout: post
comments: true
title: Correlating Technical Replicates Part 5
tags: DNR SRM
---

## Things DO get better! (Kind of)

Steven took a [reformatted dataset](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-24-Normalized-Adjacent-Technical-Replicates) I [made with this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Dataframe-Manipulation.R) yesterday and calculated the coefficient of variance between each technical replicate for all my transitions. He then eliminated all transitions with a CV greater than 20 and [saved the document](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-24-Norm-modSR.csv). Today, I averaged my technical replicates and proceeded with my NMDS and ANOSIM analyses to see if anything had changed in [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-25-NMDS-ANOSIM-for-Cluster-Analysis-after-CV-Filtering.R).

I found that there was still no clear separation of proteins, but Skokomish River seemed to be a bit more separated from the general large cluster. My ANOSIM results were not significant, so I know statistically there is no separation occuring by site or eelgrass condition.

![NMDS-averaged](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-25-NMDS-Norm-Analysis-Averaged.jpeg)

**Figure 1**. NMDS for averaged technical replicates after CV filtering.

I then decided to look more closely at the dataset that Steven created and see how it affected the clustering of my technical replicates in [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-25-NMDS-for-Technical-Replication-after-CV-Filtering.R). This clustering is much better, but still not as good as it needs to be. While my samples are clustering together, they're supposed to be right on top of eachother.

![NMDS-tech-rep](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-25-NMDS-TechnicalReplication-Normalized-after-CV-Filtering.jpeg)

**Figure 2**. NMDS for technical replication.

I calculated the ordination distances between technical replicates as well.

![ordination-distances](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-25-NMDS-TechnicalReplication-Ordination-Distances-after-CV-Filtering.jpeg)

**Figure 3**. Ordination distances been technical replicates.

I decided to use a cutoff of 0.2 to remove the 10% of my samples with the highest ordination distances. After removing these samples from analysis, I averaged my technical replicates and remade my NMDS plot. Once again, there wasn't any clear separation. The Skokomish River points I thought might be separating out turned out to be some of the outliers that I pulled. Welp.

![NMDS-adj-averaged](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-25-NMDS-Norm-Analysis-Averaged-Adjusted.jpeg)

**Figure 4**. NMDS for averaged technical replicates after CV and ordination distance filtering.

This process demonstrated that using coefficients of variance can help improve my technical replication. I didn't look too closely at Steven's list, so I don't know if there were some proteins with only one transition after filtering that should be removed too. I wonder if using a stricter cutoff will improve things even more? I'm going to look at my regression cutoffs and reaverage my technical replicates to see if other methods change my outcomes. After giving everything a cursory look, I can figure out how to proceed.

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
