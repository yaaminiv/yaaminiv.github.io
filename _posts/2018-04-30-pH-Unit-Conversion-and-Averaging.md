---
layout: post
comments: true
title: pH Unit Conversion and Averaging
tags: manchester
---

## Having pH in mV makes no sense

...which is why I had to convert some pH measurements today! My goal was to take my [water chemistry data](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/Water-Chemistry-Data/2018-04-30-pH-Calculations/2018-04-30-Manchester-Water-Chemistry-Data.xlsx) and use an [R script from Hollie](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/Water-Chemistry-Data/2018-04-30-pH-Calculations/2018-04-30-pH-Data-mV-Conversion.R) to convert mV readings to pH units. Looking at the script, I realized the first thing I needed to do was separate my water chemistry data into individual grab sample and calibration curve spreadsheets by date. I could have done this in an R script, but I didn't want to spend time figuring it out. I just copied and pasted stuff into different .csv files. You can find the raw data for [grab samples](https://github.com/RobertsLab/project-oyster-oa/tree/master/data/Manchester/Water-Chemistry-Data/2018-04-30-pH-Calculations/2018-04-30-Grab-Samples) and [calibration curve measurements](https://github.com/RobertsLab/project-oyster-oa/tree/master/data/Manchester/Water-Chemistry-Data/2018-04-30-pH-Calculations/2018-04-30-Calibration-Measurements) in the linked folders.

Then, I ran the script! It gave me a separate data file for each sampling day (named "Data YEAR-MONTH-DAY .csv"). Those data files can be found in [this folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/data/Manchester/Water-Chemistry-Data/2018-04-30-pH-Calculations). I then compiled all of the data from the individual files into [this one .csv file](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/Water-Chemistry-Data/2018-04-30-pH-Calculations/2018-04-30-pH-Discrete-Samples-by-Tank.csv) that only has data from oyster experimental tanks. Finally, I ran that data through [this R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/Water-Chemistry-Data/2018-04-30-pH-Calculations/2018-04-30-pH-Average-Calculations.R) to average pH by experimental condition! I wrote out the data [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/Water-Chemistry-Data/2018-04-30-pH-Calculations/2018-04-30-Average-pH.csv).

Now to update the methods section to reflect this process. So close to submitting this paper!

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
