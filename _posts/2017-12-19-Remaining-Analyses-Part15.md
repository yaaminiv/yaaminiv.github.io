---
layout: post
comments: true
title: Remaining Analyses Part 15
tags: DNR SRM
---

## The [growth data](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/DNR/2017-12-05-OysterGrowth.csv) wasn't very interesting

Well, I guess it's interesting. But just not for my proteomics story. In this [R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-19-Growth-Data-Analyses/2017-12-19-Growth-Data-Analyses.R), I looked at differences in percent growth by site.

![growth-boxplot](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-19-Growth-Data-Analyses/2017-12-19-Growth-Differences-Between-Sites.jpeg)

**Figure 1**. Differences in percent growth by site.

Willapa Bay had the least variation in percent growth for just my samples. The ANOVA was significant, but a Tukey test revealed that the only significant pairwise differences were FB-CI (p = 0.0313908) and SK-FB (p = 0.0102010).

To see if the growth data could explain any differences in peptide abundance, I regressed peptide abundance against percent growth. I generated 37 scatterplots, which can be found in this [folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-19-Growth-Data-Analyses/2017-12-19-Peptide-Growth-Scatterplots). None of the R-squared values were any different from zero, so I didn't put them into a new table.

My conclusion is that the growth data isn't going to be a part of my paper's story. One less thing to worry about!

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
