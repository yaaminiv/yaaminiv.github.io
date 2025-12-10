---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 83
tags: green-crab-wc 
---

## Posthoc tests for TTR and oxygen consumption

I started writing the results for TTR and oxygen consumption, but realized that I never actually did the posthoc tests with `emmeans`! So, I finally did those.

There was a significant impact of temperature, day, and their interaction on righting response. That was driven by [significantly slower righting response at 5ºC when compared to the other two temperatures](https://github.com/yaaminiv/wc-green-crab/blob/main/output/03-TTR-analysis/tukey-test-avgTTR-treatment.csv). There were [several pairs of days that had different TTR within temperatures](https://github.com/yaaminiv/wc-green-crab/blob/main/output/03-TTR-analysis/tukey-test-avgTTR-day.csv). I focused on the results from 5ºC and 25ºC only. The stats supported what I saw, which was that righting response slowed considerably after day 2 at 5ºC, and again after day 14. At 25ºC, righting response sped up after day 1, and again towards the end of the experiment. The rate of change within each temperature was also different, which measured the interaction.

Only temperature had a significant impact on oxygen consumption rate, which was much easier to interpret. The temperature differences, again, were [driven by slower oxygen consumption at 5ºC when compared to 15ºC and 25ºC, with no difference between 15ºC and 25ºC](https://github.com/yaaminiv/wc-green-crab/blob/main/output/04-respirometry-analysis/tukey-test-oxygenConsumptionRate-treatment.csv). It's nice to see the same story emerge between righting response and oxygen consumption!

For both TTR and oxygen consumption, I added the overall model results to the figures. I didn't bother adding any pairwise information differences because I think that would be way too overwhelming, but we'll see what others think.

### Going forward

1. Metabolomics results
2. Index, get advanced transcriptome statistics, and pseudoalign with `salmon`
4. Clean transcriptome with `EnTap` and `blastn`
4. Identify differentially expressed genes
5. Additional strand-specific analysis in the supergene region
2. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper

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
