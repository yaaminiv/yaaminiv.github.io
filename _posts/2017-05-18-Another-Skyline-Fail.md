---
layout: post
comments: true
title: Another Skyline Fail
tags: DNR DIA skyline
---

## The DIA pipeline has been quite a #strugglebus 

![ride-this-virtual-struggle-bus-and-watch-your-lif-1-11214-1380570808-0_big](https://cloud.githubusercontent.com/assets/22335838/26259728/29fdce7e-3c7f-11e7-9377-96b5bf518068.jpg)

It's unfortunate that the strugglebus website no longer works :/ Let's go through our current struggle:

- I showed Emma my [error checking results](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_Skyline_20170512/error-checking/2017-05-13-error-checking.txt)
- She said my error rate was pretty high (and I agreed)
- She looked at my blib and Laura's blib with people from her lab (a.k.a. the Skyline creators)
- They found asterisks at the end of some of the blib sequences that could affect Skyline's ability to peak peaks
- Another problem may be that brecan reports the same peptide multiple times, and Skyline doesn't like that

Overall, this means that my current Skyline output isn't valid anymore. I can't work on MSstats or target identification using Skyline data, but I can still look through the literature for proteins of interest. Emma and her team are working on fixing Skyline for our files, so I just have to wait! After everything's fixed, I will need to check error rates again for the same peptides.

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
