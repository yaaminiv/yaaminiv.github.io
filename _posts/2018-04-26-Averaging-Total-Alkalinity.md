---
layout: post
comments: true
title: Averaging Total Alkalinity
tags: manchester
---

## I have water chemistry data!

Sam ran samples from the beginning, middle, and end of my Manchester adult pH exposure on the titrator. He then calculated total alkalinity for these samples. I saved the information in [this .csv file](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/Water-Chemistry-Data/2018-04-26-Total-Alkalinity-per-Tank.csv). More information is available in [his notebook post](http://onsnetwork.org/kubu4/2018/04/24/total-alkalinity-calculations-yaaminis-ocean-chemistry-samples/).

For [my paper](https://docs.google.com/document/d/1r7N2d_ax8xp5slTgn43E3bmih3lBB8-eQmfg0ICs6Fo/edit#), Steven suggested I make a table with average total alkalinity values for each sampling period for both control and experimental treatments. I used [this R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/Water-Chemistry-Data/2018-04-26-Total-Alkalinity-Average-Calculations.R) to average total alkalinity values and calculate standard errors, and exported the data in [this .csv file](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/Water-Chemistry-Data/2018-04-26-Average-Total-Alkalinity.csv). It's something I could have just done in Excel, but this way I have functional for loops in case I need to run more water samples and perform these calculations on a larger dataset.

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
