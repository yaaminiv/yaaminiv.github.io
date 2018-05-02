---
layout: post
comments: true
title: In Situ Carbonate Chemistry Parameters
tags: manchester
---

## CCC: Carbonate Chemistry Calculations

After [converting my pH values from mV to actual pH units](https://yaaminiv.github.io/pH-Unit-Conversion-and-Averaging/), I needed to calculate the in situ pH. pH values can change based on the temperature and salinity at the time of measurement, so those variables need to be taken into account.

To calculate in situ pH (amongst other carbonate chemistry parameters), I needed total alkalinity, salinity, and temperature measurements. Therefore, I could only complete these calculations for the days I had [TA measurements](https://yaaminiv.github.io/Averaging-Total-Alkalinity/). I compiled all of the necessary inputs into [this .csv file](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/Water-Chemistry-Data/2018-04-30-In-Situ-Variable-Calculations/2018-04-30-Seacarb-Inputs.csv). Hollie shared the code I needed to calculate carbonate chemistry parameters in `seacarb`, which I modified in [this R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/Water-Chemistry-Data/2018-04-30-In-Situ-Variable-Calculations/2018-04-30-In-Situ-pH-Calculations.R). After running `seacarb`, I got my in situ carbonate chemistry parameters in [this .csv file](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/Water-Chemistry-Data/2018-04-30-In-Situ-Variable-Calculations/2018-04-30-In-Situ-Carbonate-Chemistry-Parameters.csv)! Because the salinity of Tris buffer was extremely similar to that of my experimental tanks, my pH measurements did not change at all.

Although my JSR manuscript has already been submitted (YAY), Steven recommended that I do some work to bolster the water chemistry section, since that will be scrutinized the most. I now have scripts ready for all carbonate chemistry calculations, so that will be an easy fix. I just need to figure out if we need to run more TA samples.

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
