---
layout: post
comments: true
title: Remaining Analyses Part 22
tags: DNR SRM
---

## Revised salinity data!

The good news: I have actual, real salinity data now

The bad news: Salinity no longer explains my protein abundance data

Micah sent me [new salinity data](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/DNR/2018-05-30-Fixed-Salinity-from-Micah.csv). The [original data](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/DNR/2017-11-25-Calculated-Salinity-Output-from-Micah.csv) was salinity that was incorrectly converted from conductivity. Here are Micah's notes:

> Data availability:
Data for Port Gamble Bare were unavailable due to instrument failure
Instruments at Skokomish Bare and Skokomish Eelgrass were deployed on 06/22/16, so there are no data before that date at this site

> Data treatment:
Conductivity observations less than 0 were removed (these occur when the instrument is dry)
Salinity was calculated from conductivity using the swSCTp command in R package 'oce', with temperature at 25°C and pressure at 10dbar

> Qualitative:
Looking at these data, I think something went wrong at Case Inlet Eelgrass, Fidalgo Bay Bare, and Willapa Bay Bare. Declining values or sporadic low values suggest that the instruments were buried or clogged at some point.
Skokomish Bare and Skokomish Eelgrass had really different salinities!

I then ran the revised salinity data through [this script to quality control it using tidal data](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-13-Environmental-Data-Quality-Control.R), then created graphs with [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-QC-Environmental-Data-Salinity.R). I still need to run it through [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-19-Environmental-Data-Variables-of-Interest.R) to extract important environmental variables. However, I saw that Willapa Bay no longer has the lowest salinity!

![newsal](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2018-05-31-Salinity-QC-Boxplot-Site-Only.jpeg)

![diurnal](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2018-05-31-Diurnal-Salinity-QC-Fluctuations-and-Boxplot.jpeg)

**Figures 1-2. Boxplot and diurnal fluctuations of salinity during the outplant.**

Alex and Micah were suspicious about the low salinity values, so at least I have correct data now. The sad part is that there's no longer a salinity explanation for my data! I'm going to rework my discussion to focus a bit more on temperature. Then, I'm going to do a preliminary analysis of the NANOOS chlorophyll data. Woot.

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
