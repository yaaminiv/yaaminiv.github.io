---
layout: post
comments: true
title: Remaining Analyses Part 16
tags: DNR SRM
---

## Tying up (more) loose ends

Brent and Emma suggested a few things to me many moons ago:

- See if there's a site-level bare vs. eelgrass effect
- Correct my p-values for multiple comparisons

A few weeks later, I finally did it.

### Bare vs. eelgrass effect

[R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-Individual-Site-Eelgrass-Differences/2018-02-15-Individual-Site-Eelgrass-Differences.R)

I took my data and broke it up by site. I then conducted an ANOVA at each site testing differences in protein abundance between bare and eelgrass habitats. I wrote out the ANOVA F-statistic, original p-value, and Benjamini-Hochberg corrected value in the following data tables:

[Case Inlet](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-Individual-Site-Eelgrass-Differences/2018-02-15-OneWayANOVA-TukeyHSD-by-Habitat-CaseInlet-pValues.csv)

[Fidalgo Bay](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-Individual-Site-Eelgrass-Differences/2018-02-15-OneWayANOVA-TukeyHSD-by-Habitat-FidalgoBay-pValues.csv)

[Port Gamble Bay](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-Individual-Site-Eelgrass-Differences/2018-02-15-OneWayANOVA-TukeyHSD-by-Habitat-PortGamble-pValues.csv)

[Skokomish River Delta](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-Individual-Site-Eelgrass-Differences/2018-02-15-OneWayANOVA-TukeyHSD-by-Habitat-SkokomishRiver-pValues.csv)

[Willapa Bay](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-Individual-Site-Eelgrass-Differences/2018-02-15-OneWayANOVA-TukeyHSD-by-Habitat-WillapaBay-pValues.csv)

I also created a bunch of boxplots, which can be found in [this folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-Individual-Site-Eelgrass-Differences). I did not find any site-level effects! I'm honestly glad that there are no site-level effects. That would make my story waaaaay more complicated.

### Correcting for multiple comparisons

[R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-06-Boxplots/2017-11-06-Protein-Area-Boxplots-after-Integration.R)

According to this [handy dandy stats website](http://www.biostathandbook.com/multiplecomparisons.html), doing a bunch of statistical tests can lead to some significant results just by chance. That's not good. With the Benjamini-Hochberg method, you can set a false discovery rate, then only count p-values that mee that FDR criteria. The FDR should be set ahead of time, before looking at the data. Since this is an exploratory study, I don't want to set a stringent FDR and miss something. But I also don't want to set my FDR so high that everything is significant. I settled on 10%.

In R, I used the function `p.adjust` to take the p-values I got from the ANOVA and modify them with the B-H method. Any p-values less than my FDR are considered significant. For these peptides, I can look at the Tukey HSD p-values, which are already adjusted.

Here's my [revised table](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-06-Boxplots/2017-11-06-OneWayANOVA-TukeyHSD-by-Site-pValues.csv). I have 13 peptides that are significant!

My next step is to incorporate all of the peptide abundance information into a succinct figure.

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
