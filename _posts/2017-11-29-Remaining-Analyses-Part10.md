---
layout: post
comments: true
title: Remaining Analyses Part 10
---

## Following through on my plan

...kind of.

I started to look up tidal data so I can clip out any low tide and exposure values, as [Micah suggested](https://yaaminiv.github.io/Remaining-Analyses-Part9/). However, I could not find any tidal information for the Skokomish River Delta site on that database. The website had me pick cities that were closest to the outplant sites. Here's what I decided on.

[Vaughn, Case Inlet](http://tbone.biol.sc.edu/tide/tideshow.cgi?site=Vaughn%2C+Case+Inlet%2C+Puget+Sound%2C+Washington&units=f)

[Port Gamble, Port Gamble Bay](http://tbone.biol.sc.edu/tide/tideshow.cgi?site=Port+Gamble%2C+Washington&units=f)

[Anacortes, Fidalgo Bay](http://tbone.biol.sc.edu/tide/tideshow.cgi?site=Anacortes%2C+Guemes+Channel%2C+Washington&units=f)

[Nahcotta, Willapa Bay](http://tbone.biol.sc.edu/tide/tideshow.cgi?site=Nahcotta%2C+Willapa+Bay%2C+Washington&units=f)

I'm also unclear as to what exposure we need to clip out. Are we taking out any data from all low tides, or just from complete instrument exposure? How do we know what the difference is? It seems like I need the depth data from Micah to proceed.

Right now I think it makes the most sense to just visualize the salinity data, without clipping out any funky values (it seems like Micah already did some of that anyways...?) and then see if it will be a candidate explanatory variable for my protein expression. I've also been rethinking my original idea for a regression analysis. I think the power of such an analysis would be compromised since I only have protein expression from the very end of the experiment, as opposed to multiple time points. Condensing such high resolution environmental data into one number (mean or median) to build a model off of seems unwise. I think I'm going to shoot an email off to Julian to see if he has any ideas on how to tackle this.

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
