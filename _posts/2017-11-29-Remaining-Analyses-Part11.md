---
layout: post
comments: true
title: Remaining Analyses Part 11
tags: DNR SRM
---

## Connecting the dots

Overwhelmed with the amount of data I had to work with, I did what any #ResponsibleGradStudent would do: talk to my adviser. Steven and I decided that the two best things to do would be to visualize diurnal fluctuations and regress peptide abundances against biomarkers. For the environmental variables, I'm not going to clip out any low tide exposure unless we think there's something interesting to pursue and refine.

### Environmental variables

#### Temperature

This the variable that I for sure think could explain some differences in peptide abundance between sites.

[R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-15-Environmental-Data-Analyses-Temperature.R)

![diurnal](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-15-Diurnal-Temperature-Fluctuations.jpeg)

**Figure 1**. Diurnal fluctuation of temperature at each site.

![boxplot](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-15-Temperature-Boxplot-Site-Only.jpeg)

**Figure 2**. Differences in temperatures at each site.

#### Salinity

I was [unable to clip out low exposure data](https://yaaminiv.github.io/Remaining-Analyses-Part10/), but like I mentioned above, I'm not going to clip it out unless something looks interesting. Aside from higher salinity at Fidalgo Bay (which could be an artifact of low tide exposure), I don't see anything worth pursuing.

[R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Environmental-Data-Analyses-Salinity.R)

![diurnal](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Diurnal-Salinity-Fluctuations-and-Boxplot.jpeg)

**Figure 3**. Diurnal fluctuation of salinity at each site.

![boxplot](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Salinity-Boxplot-Site-Only.jpeg)

**Figure 4**. Differences in salinity at each site.

#### pH

Willapa Bay had higher pH than Port Gamble Bay and Skokomish River Delta, but not the other two sites. Unlike the other environmental variables, all of my sites were not significantly different from eachother. The significant differences are between Fidalgo Bay and Port Gamble Bay (0.0004023), Skokomish River Delta (0.0028254), and Willapa Bay (p = 0.0002764)

[R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Environmental-Data-Analyses-pH.R)

![diurnal](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Diurnal-pH-Fluctuations-and-Boxplot.jpeg)

**Figure 5**. Diurnal fluctuation of pH at each site.

![boxplot](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-pH-Boxplot-Site-Only.jpeg)

**Figure 6**. Differences in pH at each site.

#### Dissolved Oxygen

Willapa Bay had slightly lower dissolved oxygen than the other sites. However, this could be skewed by the fact that there are some extreme exposure values!

[R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Environmental-Data-Analyses-DO.R)

![diurnal](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Diurnal-DO-Fluctuations.jpeg)

**Figure 7**. Diurnal fluctuation of temperature at each site.

![boxplot](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-DO-Boxplot-Site-Only.jpeg)

**Figure 8**. Differences in temperatures at each site.

### Biomarkers

To see if any of Alex's biomarkers explained peptide abundance variation, I regressed each biomarker against each peptide. My many, many scatterplots can be [found here](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Biomarker-Scatterplots).

I skimmed through all of the R-squared values the regressions and didn't see many that had values over 0.5. Perhaps there's a different method I should try.

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
