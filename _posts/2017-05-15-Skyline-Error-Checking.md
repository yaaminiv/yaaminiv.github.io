---
layout: post
comments: true
title: Skyline Error Checking
---

## Who wants to manually quality check peptides?!

Took 8.5 hours on a Sunday night, but I calculated an error rate for Skyline! I randomly selected 100 proteins in Skyline, and for each protein I checked one peptide. Ideally, these peptides had data availabe for every replicate. I evaluated whether or not Skyline chose the right peak, and if the peak boundaries were correctly deliniated.

For each peak, I assigned a "0" if Skyline picked the wrong peak and a "1" if it picked the peak correctly. For some peptides, there was no data available for a replicate. This would be a blank screen, so I gave it the value "N/A." I then calculated error rates per replicate and peptide. You can find my spreadsheet [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_Skyline_20170512/error-checking/2017-05-13-error-checking.txt).

Here are some examples of Skyline spectra.

1: Skyline picked the right peak

![unnamed](https://cloud.githubusercontent.com/assets/22335838/26095588/fe990ed2-39d2-11e7-884e-47ad4eb78e2f.png)

0: I assigned this value when Skyline picked the wrong peak. I also assigned this value when Skyline picked a peak where there was just noise.

Wrong peak:

![unnamed-2](https://cloud.githubusercontent.com/assets/22335838/26095636/26fcdaca-39d3-11e7-8421-4978949643e6.png)

Noise:

![unnamed-3](https://cloud.githubusercontent.com/assets/22335838/26095664/3d39dcc0-39d3-11e7-9509-812cea8e71b2.png)
![unnamed-4](https://cloud.githubusercontent.com/assets/22335838/26095663/3d344e4a-39d3-11e7-8462-575fb39ec5d4.png)

Changing peak boundaries:

![image-1](https://cloud.githubusercontent.com/assets/22335838/26095486/ab73308e-39d2-11e7-8528-011a414c7776.png)
![image-2](https://cloud.githubusercontent.com/assets/22335838/26095487/aba13ee8-39d2-11e7-93d0-42a1b0043b89.png)

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
