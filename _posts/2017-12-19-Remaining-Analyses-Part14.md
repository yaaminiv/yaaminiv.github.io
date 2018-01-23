---
layout: post
comments: true
title: Remaining Analyses Part 14
---

## I see the light

I think I'm almost done analyzing data! Which means I'm close to just sitting, entering my mental happy place, and writing away (yeah, I guess I'm one of those scientists that like to write) :relieved:

Here's what took me 4.5 days to feel motivated and 1.5 days to do:

### Downloaded tide data

I downloaded tide data from the links in [this lab notebook entry](https://yaaminiv.github.io/Remaining-Analyses-Part10/). I used [Union](http://tbone.biol.sc.edu/tide/tideshow.cgi?site=Union%2C+Washington&units=f) as my Skokomish River Delta Site, just as [Micah suggested](https://yaaminiv.github.io/Environmental-Data-Meeting-Part3/). The website allowed me to get tide data in 10 minute intervals, which perfectly aligns with the environmental data I have. I formatted and saved the tide data in [this .csv file](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/DNR/2017-12-13-Tidal-Data-by-Site.csv).

### Removed exposure times from data

Based [my meeting notes](https://yaaminiv.github.io/Environmental-Data-Meeting-Part3/), we decided to use a one-foot clipping. This means that I could not use any pH, salinity, or dissolved oxygen data when the tide was less than one foot. Such a conservative measure would ensure that the probe readings we use will always come from one that is submerged in water. In this [R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-13-Environmental-Data-Quality-Control.R), I replaced all probe readings from less than one-foot tides with "NA". I then replaced any values greater than 1.5IQR + Third Quartile or less than First Quartile - 1.5IQR (i.e. outliers) with "NA". Within R Studio, `as.Date` and the ability to find and replace within a highlighted selection were extremely useful. 

I followed Steven's suggestion to only use data from bare habitats, so I didn't manipulate any data from eelgrass outplants. The only exception is that I needed to use data from the eelgrass habitat at Port Gamble Bay for salinity data. For some reason, there is no salinity data from the bare outplant at that site. Not sure how this will effect downstream analyses.

I saved the quality controlled [pH](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-pH-Data-QC-with-Tide-Data.csv), [dissolved oxygen](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-DO-Data-QC-with-Tide-Data.csv), and [salinity](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-Salinity-Data-QC-with-Tide-Data.csv) in separate .csv files.

### Remade boxplots and timeseries figures

New data means new figures, right? I used the quality controlled data for pH, dissolved oxygen and salinity. I considered removing outliers for the temperature data, but since the general consensus was to not mess with that data, I didn't bother. If I need to, I have the code!

#### Temperature

[R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-QC-Environmental-Data-Temperature.R)

![temp-timeseries](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-Temperature-Fluctuations-and-Boxplot.jpeg)

![temp-boxplot](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-Temperature-Boxplot-Site-Only.jpeg)

**Figures 1-2**. Temperature time series and boxplot comparing temperature between sites.

Nothing different to see here! Temperature at Willapa Bay was higher on average when compared to Puget Sound sites.

#### pH

[R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-QC-Environmental-Data-pH.R)

![pH-timeseries](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-pH-QC-Fluctuations-and-Boxplot.jpeg)

![pH-boxplot](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-pH-QC-Boxplot-Site-Only.jpeg)

**Figures 3-4**. pH time series and boxplot comparing pH between sites.

Willapa Bay had the highest average pH, followed closely by Case Inlet. This is something I didn't see before! The time series figure still shows wonky pH patterns towards the end of the outplant, possibly due to probe burial.

#### Dissolved Oyxgen

[R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-QC-Environmental-Data-DO.R)

![DO-timeseries](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-Diurnal-DO-QC-Fluctuations.jpeg)

![DO-boxplot](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-DO-QC-Boxplot-Site-Only.jpeg)

**Figures 5-6**. Dissolved oxygen time series and boxplot comparing dissolved oxygen between sites.

Willapa Bay had the lowest average dissolved oxygen content. Additionally, it had the least variability.

#### Salinity

[R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-QC-Environmental-Data-Salinity.R)

![salinity-timeseries](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-Diurnal-Salinity-QC-Fluctuations-and-Boxplot.jpeg)

![salinity-boxplot](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-18-Salinity-QC-Boxplot-Site-Only.jpeg)

**Figures 7-8**. Salinity time series and boxplot comparing salinity between sites.

Woah, Willapa Bay very clearly had the lowest salinity levels! There also seems to be some sort of geographical gradient with salinity that could be helpful for Laura's work. The timeseries plot shows that we needed to remove a lot of the data from Fidalgo Bay due to the probe malfunctioning.

### Variable of interest tables

I've learned that Steven likes tables, so I made some. I calculated at 12 different variables of interest at each site for each environmental variable in [this R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-19-Environmental-Data-Variables-of-Interest.R). These are the variables I looked at:

- Maximum
- Minimum
- Range
- Mean
- Variance
- Standard Deviation
- Percentage of data ± 2 SD
- First Quartile
- Median
- Third Quartile
- IQR
- Percentage of data > 1.5IQR * Third Quartile and < 1.5IQR * First Quartile

I wrote out a different .csv file for each environmental variable:

- [Temperature](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-19-Temperature-Data-Variables-of-Interest.csv)
- [pH](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-19-pH-Data-Variables-of-Interest.csv)
- [Dissolved Oxygen](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-19-DO-Data-Variables-of-Interest.csv)
- [Salinity](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2017-12-19-Salinity-Data-Variables-of-Interest.csv)

Now I have something to share at tomorrow's lab meeting! I'm also going to analyze the [growth data from Micah](https://yaaminiv.github.io/Environmental-Data-from-Micah-Part2/) with the hopes of sharing those results.

Back to R!

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
