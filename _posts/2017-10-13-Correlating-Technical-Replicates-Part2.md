---
layout: post
comments: true
title: Correlating Technical Replicates Part 2
tags: DNR SRM
---

## Step 1: R-squared Cutoffs

In [this R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-13-NMDS-for-Technical-Replication-with-Cutoffs.R), I used three different R-squared cutoffs to weed out transitions and reexamine my technical replication. I tried the combinations of normalized and nonnormalized data with different cutoffs. I made NMDS plots for each option and calculated the distances between my technical replicates. I eliminated any and all transitions that had adjusted R-squared values below the cutoff.

**Cutoff = 0.6, nonnormalized data**:

![0.6-nonnormalized-NMDS](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-13-NMDS-TechnicalReplication-NonNormalized-Cutoff1.jpeg)

![0.6-nonnormalized-distances](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-13-NMDS-TechnicalReplication-Ordination-Distances-NonNormalized-Cutoff1.jpeg)

**Cutoff = 0.6, normalized data**:

![0.6-normalized-NMDS](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-13-NMDS-TechnicalReplication-Normalized-Cutoff1.jpeg)

![0.6-normalized-distances](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-13-NMDS-TechnicalReplication-Ordination-Distances-Normalized-Cutoff1.jpeg)

**Cutoff = 0.7, nonnormalized data**:

![0.7-nonnormalized-NMDS](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-13-NMDS-TechnicalReplication-NonNormalized-Cutoff2.jpeg)

![0.7-nonnormalized-distances](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-13-NMDS-TechnicalReplication-Ordination-Distances-NonNormalized-Cutoff2.jpeg)

**Cutoff = 0.7, normalized data**:

![0.7-normalized-NMDS](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-13-NMDS-TechnicalReplication-Normalized-Cutoff2.jpeg)

![0.7-normalized-distances](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-13-NMDS-TechnicalReplication-Ordination-Distances-Normalized-Cutoff2.jpeg)

**Cutoff = 0.8, normalized data**:

![0.8-normalized-NMDS](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-13-NMDS-TechnicalReplication-Normalized-Cutoff3.jpeg)

![0.8-normalized-distances](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-13-NMDS-TechnicalReplication-Ordination-Distances-Normalized-Cutoff3.jpeg)

I found that normalizing my data made my NMDS plots look better, so I didn't use an 0.8 cutoff with nonnormalized data. Overall, my NMDS plots looked better, but they're still not fantastic. With a 0.6 cutoff, I had 88 transitions, 45 with a 0.7 cuttoff, and 17 with a 0.8 cutoff. I personally think the 0.7 cutoff is a happy medium between losing too much data on proteins and a better NMDS plot. I still need to try the x = y slope method outlined in this [issue](https://github.com/RobertsLab/project-oyster-oa/issues/18). For now I'll update Emma and Steven and see what they think.

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
