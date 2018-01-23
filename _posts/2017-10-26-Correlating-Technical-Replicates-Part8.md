---
layout: post
comments: true
title: Correlating Technical Replicates Part 8
tags: DNR SRM
---

## Maybe I should have called this series "Troubleshooting"...

Anyways, I discussed my [previously found]() "improved" technical replication at lab meeting where Steven mentioned that closer together means nothing without looking at axes. Whoops, forgot about that part. Since it's more important to just look at the protein data, I made boxplots for each transition for all three levels of CV filtering (>20, >15, >10), removing technical replicates with large ordination distances (greater than 0.20). I made two sets of boxplots: using just sites, and using both sites and eelgrass condition.

**CV > 20**:

[R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-20-Boxplots/2017-10-26-Protein-Area-Boxplots-after-CV20-Filtering.R)

[Folder with boxplots](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-20-Boxplots)

**CV > 15**:

[R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-15/2017-10-26-CV-15-Boxplots/2017-10-26-Protein-Area-Boxplots-after-CV15-Filtering.R)

[Folder with boxplots](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-15/2017-10-26-CV-15-Boxplots)

**CV > 10**:

[R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-10/2017-10-26-CV-10-Boxplots/2017-10-26-Protein-Area-Boxplots-after-CV10-Filtering.R)

[Folder with boxplots](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-10/2017-10-26-CV-10-Boxplots)

My biggest concern now is the fact that we selected individual transition data to remove for each technical replicate, as opposed to removing a transition completely from the analysis. So some transitions have a much larger dataset than others. How can I account for these irregularities moving forward? I'm assuming my next step is to also examine the boxplots and quantify how similar/different my median/mean values are (i.e. t-tests or ANOVAs).

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
