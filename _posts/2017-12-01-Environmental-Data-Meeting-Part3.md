---
layout: post
comments: true
title: Environmental Data Meeting Part 3
tags: DNR meeting-notes
---

## Yesterday's meeting notes

In the third installment of our "what does all of this actually mean" meetings, Micah, Alex, Emma, Brent, Laura, Steven and I discussed the progress we've made integrating all of our data into one cohesive story.

**Notes**:

- Dissolved oxygen measurements
  - FB most eelgrass dominated, higher pH, could have daily super saturation (DO > 12)
    - Need to do literature survey to verify measurements are "real"
  - Padilla Bay: DO ~ 19.3 max for sensors that never come out of the water
- Should clip DO, pH and salinity data
  - Conservative one hour/one foot clipping
  - Use Union for SK tidal data
  - Just use bare for all sites
  - Correcting values to the right mean salinity from sensors can be difficult, lead to discrepancies
  - End drops in salinity and pH could be burials
- Can examine brief window of environmental data one or two days before sampling
- Number of low tides could be interesting
  - ex. Lots of drops in salinity at WB --> could number of low tides affect protein expression?
- Eelgrass extent as an explanatory variable
  - Global eelgrass effect could override any bare sites?
- Biomarker data
  - Ignore fatty acid data for now since there's a low sample size
  - Final height is a proxy for growth
  - FB grew the most, CI grew the least
  - Tissue mass highest in FB, then PG. WB, SK, and CI were similar
  
**Next steps**:

- Figure out [biomarker comparison table code](https://yaaminiv.github.io/Remaining-Analyses-Part12/)
- Scrub data
- Make environmental variable table
  - Average
  - Median
  - Maximum
  - Minimum
  - Standard deviation/variance
  - Number of observations above/below SD/2 SDs
  - Number of exposures/low tides
  - Days exposed
  - Total time exposed
  
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

