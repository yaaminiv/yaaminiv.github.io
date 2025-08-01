---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 68
tags: green-crab-wc RNA-library-preparation
---

## Pooling libraries

I'm ready to send my libraries out for sequencing! First, I need to determine my sample pools. Carolyn suggested creating pools of about 8 samples each that all have roughly the same concentration. This way, they can do QC and spike in more of the low volume samples if needed.

This means I need to create 11 pools. I sorted my library information by cDNA concentration and assigned pools that way. The information can be found [here](https://docs.google.com/spreadsheets/d/1B1tyeCI7F_T-l41144m6k_MEVhhU-XCHAEkr6PHoTpw/edit?gid=33522049#gid=33522049). I reviewed the list twice to ensure that each sample had a unique index, since I ordered the samples by combined indices to make it easier to pool the samples (much easier to figure out which pool a in a strip goes to, as opposed to a mad hunt for all the libraries that should go into one pool). A few notes:

- To actually pool my samples, Carolyn suggested I add ~10 µL of the library to each pool. I added 10 µL of library to each pool, except for 5-135. For some reason, I only had 10 µL of this sample! I added 5 µL of pool. I'm hoping that this is because it maybe got less TE so it's more concentrated, and not because I didn't transfer all the eluted library.
- Prior to adding the aliquot to a pool, I took the strip tube and flicked it, then spun it down. I forgot to flick mix for samples 15-161, 15-092, 5-009, and 25-202. By the time I realized I forgot to mix the samples, I decided to pipet mix 25-202, 5-004, 5-121, and 5-069. These samples were just pairs of two tubes so it was already a feat to keep everything straight.
- All pooling was done at room temperature since cDNA libraries should be relatively stable.

Once I pooled my samples, I put them back in the -20ºC. I sent them out on dry ice the next day so now we wait for data!

### Going forward

1. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper
6. Start methods and results of 2023 paper

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
