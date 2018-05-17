---
layout: post
comments: true
title: Remaining Analyses Part 22
tags: DNR SRM
---

## Revising environmental data

Before I begin, a quick shoutout to myself for making the comments on my R scripts detailed enough that I can return to a script I created in December and rerun it!

Micah and Alex both pointed out that the Pacific oysters were only outplanted on June 19, 2016. This means all of my environmental data calculations used data points before the experiment even started! To fix this, I made changes to [this R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-19-Environmental-Data-Variables-of-Interest.R). I used two commands to subset the data I needed:

> temperatureData$Date <- as.Date(temperatureData$Date, format = "%m/%d/%y")

This command makes R recognize all of my dates as actual dates instead of character strings.

> temperatureData <- temperatureData[temperatureData$Date >= "2016-06-19", ]

This command allows me to subset rows with dates on or after 2016-06-19, the start of the outplant experiment.

I repeated those commands for my temperature, pH, dissolved oxygen, and salinity data. With the subset, I calculated variables of interest:

- [Temperature variables of interest](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-19-Temperature-Data-Variables-of-Interest.csv)
- [pH](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-19-pH-Data-Variables-of-Interest.csv)
- [DO](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-19-DO-Data-Variables-of-Interest.csv)
- [Salinity](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-19-Salinity-Data-Variables-of-Interest.csv)

In separate R scripts, I used the same commands to subset data and remake my boxplots and line graphs that show fluctuation over time. You can find the scripts below:

- [Temperature R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-QC-Environmental-Data-Temperature.R)
- [pH](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-QC-Environmental-Data-pH.R)
- [DO](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-QC-Environmental-Data-DO.R)
- [Salinity](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-QC-Environmental-Data-Salinity.R)

A quick note on salinity:

I had [this .csv file]() with calculated salinity output from Micah. In my [paper draft](https://docs.google.com/document/d/1giP16iXWPE7oDSNI7fyLV3p_1jqsXuuxlH7cJQAwhLM/edit?pli=1#), Micah pointed out that those are conductivity values, and not salinity. However, conductivity values would be much smaller! I emailed Micah to confirm what the values were, and he sent me this response:

> I did indeed send you 'calculated salinity' values - I took the conductivity output in mS/cm and converted it to salinity in PSU at the in situ temperature using the swSCTp function in the R package oce. BUT, after making a call to the manufacturer, I've found out that the conductivity output is standardized by the sensor software to 'what it would be at 25°C' rather than outputting the raw value at the in situ temperature. So, roughly speaking, salinity values should be ~7 units lower than they are in this spreadsheet.

TL;DR They are salinity values but the wrong ones. Either way, Willapa Bay still has the lowest salinity values. My code will be able to handle revised data once I get it, so I'm all set on that front.

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
