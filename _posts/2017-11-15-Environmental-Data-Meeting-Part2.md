---
layout: post
comments: true
title: Environmental Data Meeting Part 2
tags: DNR meeting-notes
---

## Some more information from Micah and Alex

Micah send us [environmental data](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/DNR/2017-11-14-Environmental-Data-from-Micah.csv) with the following information:

> These are roughly 200,000 observations at 10-minute intervals spanning eelgrass and bare at our five sites from Jun 1 to July 20ish, 2016.

> Key:
> WB/SK/PG/CI/FB = Willapa Bay, Skokomish, Port Gamble Bay, Case Inlet, and Fidalgo Bay
> E/B = Eelgrass, Bare
> pH, pHT, do, doT = pH (total scale, calculated for in-situ temperature), temperature recorded by pH sensor, do (in mg/L), temperature recorded by do sensor
> (XX) = sensor label, just ignore this
> Some quick notes: We have no pH data for PGE, because both sensors failed. Also no data for SKB and SKE before end of June. Sensors were first deployed at that site on 6/22/16. There are no data of any kind before that date. Both dissolved oxygen sensors at FB gave some very high readings, and the DO sensor at FBE appears to have failed some time in July (yielding negative and sky-high values). I left these in to provide the data in its rawest form. Many of these sensors were exposed on extreme low tides, and we may want to clip out those data.

And Alex sent some [*C. gigas* physiological information](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/DNR/2017-11-14-Alex-Prelim-Data.csv) as well:

> There are some gaps that are easy to address: i need to reweigh some vials to get tissue weight, we need to match proteome vials with isotope vials, etc.. and some not so easy to address: we crushed 10 of 15 shells per habitat in each site - in an embarassing lack of foresight we did not check that these were the same oysters used in biomarker analyses... these can be crushed but will take a while.

Before our meeting, Laura [visualized the environmental data](https://github.com/laurahspencer/LabNotebook/blob/master/_posts/2017-11-14-Look-at-DNR-Environmental-Data.md). Willapa Bay had hotter temperatures than the other Puget Sound sites, which could play into [my resutls](https://yaaminiv.github.io/Remaining-Analyses-Part3/).

Here are some notes from our meeting:

- Pulling out air time/times the sensors were exposed to the air
  - Keep air time in for temperatures, since the oysters would be exposed to the same temperatures
  - Need to cross reference tidal data for pH and dissolved oxygen measurements
    - We can pick out tidal heights and times for those thresholds, then use it to clip out data where the pH and dissolved oxygen temperatures were exposed
- Consolidating environmental data for bare and eelgrass habitats
  - In environmental data section, can show different and significant results
  - Can see if variation in bare and eelgrass measurements led to variation in protein abundances for each site, especially with pH
    - Can easily visualize bare and eelgrass datapoints in boxplots using `jitter`
  - Will need to consolidate data somehow, but an option was never given...
- Uses dissolved oxygen sensor for temperature measurements
- Could do boxplots on a weekly basis
  - Capture week effect of pH, DO and temperature variation
- Daily trends
  - Micah suggested picking a window of time for max and min pH
    - 3 a.m. to 5 a.m. for pH min, 4 p.m. to 6 p.m. (or 6 p.m. to 8 p.m.) for pH max
    - Windows good for caculating means, understanding significance
  - Average daily trend plots in R
  - Quantify temporal scale of variability
    - Acute exposure times
      - Plot frequency of extremes
      - Plot total time in "extreme" condition based on thresholds
- Annectodal site information
  - Case Inlet and Fidalgo Bay never fully exposed at low tide
  - Port Gamble Bay, Skokomish River Delta, and Willapa Bay fully exposed at low tide
- Information we will get soon
  - Micah
    - Date and time of sampling
    - Chlorophyll and conductivity information
    - Methods and R code for clipping out air exposure information in pH and DO data
  - Alex
    - Sent information on *C. gigas* samples I used for proteomics, will provide all correlated information
    - Will try and track down family information for outplanted siblings
  - Emma
    - Should have information on the samples I'm rerunning tonight, will walk through samples in Skyline for me while I'm gone
    - Rerunning technical duplicates will be $2000, triplicates will be $2800

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
