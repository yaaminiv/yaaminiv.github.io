---
layout: post
comments: true
title: Reproductive Output Analyses
tags: manchester
---

## i.e. My grand return to RStudio

It's been a while since I did any stats-related! I tackled the preliminary analysis of the [egg production](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/2017-07-30-Pacific-Oyster-Larvae/2018-02-14-Egg-Production-Data.csv) and [hatch rate](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/2017-07-30-Pacific-Oyster-Larvae/2018-02-14-Hatch-Rate-Data.csv) data from Manchester. Since my [original data](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/2017-07-30-Pacific-Oyster-Larvae/2017-07-29-Spawning-Calculations.xlsx) was in an .xlsx file with several different tabs, I converted the relevant tabs into separate .csv files. I then created a new [R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Manchester_ReproductiveOutput_20180214/2018-02-14-Reproductive-Output.R) and started coding some ANOVAs.

### Egg Production

![eggproduction](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/Manchester_ReproductiveOutput_20180214/2018-02-14-Egg-Production-by-Treatment.jpeg)

There were significant differences in egg production between treatments (F = 25.87017, p = 0.00112206). Females exposed to [heat shock](https://yaaminiv.github.io/Manchester-Heat-Shock-Experiment/) produced more eggs than females exposed to either low or ambient pH treatments (HS-A = 0.0022743; L-HS = 0.0016528).

### Hatch Rate

![hatchrate](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/Manchester_ReproductiveOutput_20180214/2018-02-14-Hatch-Rate-by-Treatment.jpeg)

![femrate](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/Manchester_ReproductiveOutput_20180214/2018-02-14-Hatch-Rate-by-Female-Treatment.jpeg)

![malerate](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/Manchester_ReproductiveOutput_20180214/2018-02-14-Hatch-Rate-by-Male-Treatment.jpeg)

There was only a significant difference between treatments when taking female treatment into account (F = 5.606537, p = 0.01039109). This was due to a difference between low and ambient pH hatch rates, with low pH hatch rates being less than ambient pH hatch rates (L-A = 0.0111872). This is what I suspected when I first looked at this data!

### TL;DR

Females exposure to low pH treatments could explain lower larval hatch rates #MaternalEffect Since there was no difference in the number of eggs produced between females exposed to low and ambient pH treatments, perhaps low pH eggs are lower quality. There could also be an epigenetic effect at play (cue MBD-seq preparation...or at least cue bringing this finding to Steven's attention and hoping there's money in the budget).

Once I sort out my histology data, I think I'll have a story to tell for NSA.

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
