---
layout: post
comments: true
title: Correlating Technical Replicates
---

## Are some transitions worse than others?

Short answer: 
Yes.

Long answer:
Emma suggested that I regress my second batch of technical replicates against my first batch to see if there were certain transitions that are messier than others. I used [this R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/2017-10-10-Correlating-Transitions-in-Technical-Replicates.R) to plot the regressions. The final plots can be found in [this folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations).

*Random gold star moment 1: My for loop for making all my plots worked the first time I wrote it #WIN *
*Random gold star moment 2: I used a relative path to set my working directory #LEARNING *

I added the adjusted R-squared value to the top left corner of each plot. There are definitely potential outliers and leverage points in each plot, and some transitions have higher R-squared than others. I also see that some samples are continually those potential outliers and leverage points. Generally, the three transitions associated with each peptide have the same R-squared values.

![bad-R](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/CHOYP_ACAA2.1.1%7Cm.30666%20ELGLNNDITNMNGGAIALGHPLAASGTR%20y12%20.jpeg)

**Figure 1**. Transition with the lowest adjusted R-squared value.

![good-R](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-10-Transition-Replicate-Correlations/CHOYP_G6PD.2.2%7Cm.46923%20SDELYEAWR%20y5%20.jpeg)

**Figure 2**. Transition with one of the higher adjusted R-squared values.

The next step is to consult Emma and Steven to create a selection criteria. Here are two ideas:

- Establish an R-squared cutoff. Any transitions with adjusted R-squared values lower than the cutoff should be eliminated.
- Identify outliers and leverage points in each plot. Remove these points and re-plot to see if the R-squared value increases.

My guess is that we'll use some combination of methods to determine which transitions to keep. While I work on this, I'm also [reviewing my SRM protocol for reproducibility](https://yaaminiv.github.io/Reproducible-SRM-Analysis/) and making an NMDS plot with just the PRTC peptides to see if that provides us with any additional information.

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

