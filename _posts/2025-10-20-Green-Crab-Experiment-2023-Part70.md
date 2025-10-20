---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 70
tags: green-crab-wc TTR mixed-effects-models
---

## Adding demographic information to respirometry analysis

Just popping in here to say I updated my respirometry analysis to properly account for the impact of various demographic variables! Modifications done in [this script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/04-respirometry-analysis.Rmd).

Notes:

- Statistical output found [here](https://github.com/yaaminiv/wc-green-crab/blob/main/output/04-respirometry-analysis/model-comparison-stat-output.csv)
- No demographic variables were significant
- I......forgot to do a multiple test correction when I was originally looking at the impact of experimental variables on oxygen consumption. Turns out when you do the necessary corrections, only temperature is a significant predictor of oxygen consumption! Not day, and not the interaction between temperature and day. This is different than the 2022 experiment where temperature and day were both significant. I think the main difference here is that the 25ºC condition wasn't eliciting as stressful of a response as the 30ºC condition did. There's a slight increase in oxygen consumption between days 7 and 14, and a slight difference in variation in consumption at day 14, but other than that things are pretty consistent.
- No impact of genotype variables on oxygen consumption. This makes me sad because you'd think with HIF-1a mutations we'd see something going on! But perhaps there's more to be studied at the molecular level.

Guess it's time to open up the new molecular datasets!

### Going forward

1. Revise respirometry analysis
2. Preliminary metabolomics analysis
3. Preliminary RNA-Seq analysis
2. Examine HOBO data from 2023 experiment
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
