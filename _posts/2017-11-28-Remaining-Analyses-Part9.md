---
layout: post
comments: true
title: Remaining Analyses Part 9
---

## My final NMDS

The first way I thought I could tie together environmental data with protein expression was an NMDS plot with eigenvectors based on environmental variables. I realized that wasn't possible only after I made my NMDS by region in [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-ANOSIM-for-Cluster-Analysis.R).

![NMDS](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-28-NMDS-Analysis-Averaged-by-Region.jpeg)

**Figure 1**. NMDS for protein expression by region (Puget Sound vs. Willapa Bay).

I actually ended up with a significant ANOSIM result when splitting up the regions! I got R = 0.2368 instead of an expected R of -0.002, with a p-value of 0.31.

Since the eigenvector thing isn't going to work out, here's my game plan to create figures with protein expression, environmental variable and biomarker data:

- Clip out low tide/exposure recordings from salinity, pH, and dissolved oxygen data
- Visualize diurnal fluctuations
  - Salinity
  - pH
  - DO
- Boxplots
  - Salinity
  - pH
  - DO
- Calculate maximum, minimum, mean, and median for variables that show differences in Willapa Bay. So far, these are [temperature, delN, and tissue mass](https://yaaminiv.github.io/Remaining-Analyses-Part8/)
- Remake boxplots for significantly different peptides with secondary y-axes for variables found to be significant in the previous step
- Add lines denoting calculated maximum, minimum, etc. on top of boxplots for protein expression

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
