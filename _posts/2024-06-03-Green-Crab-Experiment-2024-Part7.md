---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 7
tags: green-crab-cold restriction-digest
---

## PCR optimization

[My PCR primers work](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part4/)! But the PCR protocol itself is not optimized to these primers since we saw laddering. Carolyn suggested I finagle with the thermocycler settings to try and eliminate the laddering

### 2024-05-31

Carolyn showed me how to set up a gradient on the thermocycler to test multiple different temperatures at once.

<img width="596" alt="Screenshot 2024-06-03 at 8 59 53 AM" src="https://github.com/yaaminiv/yaaminiv.github.io/assets/22335838/e9f85798-e68b-4409-9038-82677591906d">

**Figure 1**. Thermocycler gradient settings

I then ran a PCR using DNA from sample 172 (and a bit of 47 since I ran out of 172) on the gradient to see if there was a better temperature that would lead to less laddering.

<img width="596" alt="Screenshot 2024-06-03 at 8 54 21 AM" src="https://github.com/yaaminiv/yaaminiv.github.io/assets/22335838/f610e9e1-3e75-4159-a691-7d9789e65c42">

**Figure 2**. Gel image for gradient gel. 64ºC is the first lane and 54ºC is the last lane

We're still getting laddering. There is less laddering at 64ºC, but the band is also not as strong.

### 2024-06-03

It's possible that the laddering is occurring because the PCR protocol has a 90 second extension time. Maybe by having a slightly shorter extension time of 60 seconds instead, I can reduce the laddering! I used the same gradient as last time, but changed the extension time to 60 seconds.

![Screenshot 2024-06-03 at 5 04 25 PM](https://github.com/yaaminiv/yaaminiv.github.io/assets/22335838/e41066b0-2ea3-40ba-92a9-e1c583af698c)

**Figure 3**. Gel image for gradient gel with 60 second extension time. 64ºC is the first lane and 54ºC is the last lane

The shorted extension time does seem to reduce laddering a touch! When I showed the gel to Carolyn, she suggested I move forward with the 60ºC + 60 seconds on the thermocycler and test out the Alul restriction enzyme I'm borrowing from ToxLabs. Based on that, we can determine if we need to optimize the PCR more.

### Going forward

1. Set up cold room
3. Obtain MA crabs!
2. Tailor PCR protocol for new primers
3. Develop restriction band digest for genotyping
4. Develop heart rate protocol

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
