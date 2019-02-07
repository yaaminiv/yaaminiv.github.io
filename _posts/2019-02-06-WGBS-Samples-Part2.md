---
layout: post
comments: true
title: WGBS Samples Part 2
tags: manchester labwork gigas-broodstock WGBS
---

## Continuing isopropanol precipitations

While thinking about my [WGBS sample preparation](https://yaaminiv.github.io/WGBS-Samples) last night, I realized that I don't have enough of my low pH sample! I had a sample concentration of 20.8 ng/µL, but only 14 µL of sample after the Qubit. This means that sample volume was less than 500 ng and unusable for sequencing. After talking to Shelly, I did the following:

### Isolate new samples

I needed at least 200 ng of sample to reach the 500 ng minimum. I took 6.3 µL of 9-T2 and 5.5 µL of 10-T3, added them to a new tube, then vortexed the new pooled sample. I put the pooled sample back in the fridge until I needed it.

### Reprecipitate supernatant

I took the isopropanol supernatant I saved and added 5.31 µL more sodium acetate and 15 µL of isopropanol. I then spun the tube for 30 minutes at 12000 rpm and 4ºC. When I went to pipet the supernatant off, I couldn't see a white pellet. I took off the supernatant and disposed of it, leaving a little bit of liquid at the bottom. For an ethanol wash, I  added 90 µL of  75% ethanol and spun the tube for 10 minutes at 12000 rpm and 4ºC. Once again, I couldn't see a clear pellet. I took off as much ethanol as I could and warmed the tube for 2 minutes on a heat block set to 37ºC. I took a little bit of remaining liquid and added it to the samples to the 200 µL of pooled samples I made prior.

With the ethanol wash supernatant, I added 2.5 µL of sodium acetate and 10 µL of 75% ethanol. I then put the tube in the -80ºC for an hour. After the incubation, I spun the tube for 30 minutes at 12000 rpm and 4ºC. Just like the ethanol supernatant, I couldn't see a clear pellet. I did an ethanol wash with 90 µL of 75% ethanol then spun the tube for 10 minutes at 12000 rpm and 4ºC. After removing the supernatant, I set the tube on the 37ºC heat block for 2 minutes. I used the 200 µL of pooled samples to wash out any DNA stuck to the side of the ethanol wash supernatant tube. I had to run to class, so I left the tube to incubate at room temperature for a few minutes.

### Precipitate supernatant and sample combination

Shelly was nice enough to measure my sample concentration! Here's what she did:

> Before I combined the two tubes, one looked particularly cloudy, which is odd. So I combined the samples and precipitated again (40uL sample + 28uL isopropanol + 4 uL 3M NaOAc). Vortexed well. Spun for 30 min @4C. Pipetted off supernatant. There was a white pellet, which probably shouldn't be so white. I think that indicates a lot of salt. So I did 2x 1mL washes with 75% ethanol each spinning for 30 min. The pellet still seemed pretty white. Then let dry at room temp open cap for 15 min. Then resuspended in 11uL EB (Qiagen, from Sam's bench), and it was much less cloudy. Then let sit at room temp for 30 min before reading concentration.

The final concentration was 64 ng/µL in 10 µL. I have more than enough sample to send for sequencing!

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
