---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 57
tags: green-crab-wc genotype TTR respirometry mixed-effects-model
---

## Linear mixed effects models for TTR

I'm giving a couple of talks and I wanted to update the stats for the 2023 project! I used the [same linear mixed effects model framework](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part17/) with the 2023 data in [this R Markdown script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/03-TTR-analysis.Rmd). The output for the models can be found here:

- [Average TTR without genotype](https://github.com/yaaminiv/wc-green-crab/blob/main/output/03-TTR-analysis/ttr-model-comparison-stat-output.csv): Treatment, day, and their interaction are significant predictors of average TTR
- [Average TTR with genotype](https://github.com/yaaminiv/wc-green-crab/blob/main/output/03-TTR-analysis/ttr-genotype-model-comparison-output.csv): Genotype or allele presence does not significantly impact average TTR
- [Difference in TTR](https://github.com/yaaminiv/wc-green-crab/blob/main/output/03-TTR-analysis/ttr-diff-base-comparison-stat-output.csv): Only temperature is a significant predictor in difference in TTR
- [Difference in TTR with genotype](https://github.com/yaaminiv/wc-green-crab/blob/main/output/03-TTR-analysis/ttr-diff-genotype-comparison-output.csv): Genotype or allele presence does not significantly impact difference in TTR

I did something similar with genotype for the respirometry data in [this R Markdown script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/04-respirometry-analysis.Rmd):

- [Oxygen consumption without genotype](https://github.com/yaaminiv/wc-green-crab/blob/main/output/04-respirometry-analysis/respirometry-base-model-comparison-stat-output.csv): Temperature, day, and their interaction are significant predictors of oxygen consumption
- [Oxygen consumption with genotype](https://github.com/yaaminiv/wc-green-crab/blob/main/output/04-respirometry-analysis/respirometry-genotype-model-comparison-stat-output.csv): Full genotype isn't a significant predictor, but presence of the T allele is a marginally significant predictor

I think it's interesting that T allele presence is a marginally significant predictor of oxygen consumption! Based on my notes, the T allele is the cold-adapted allele. We saw a consistent acclimation response at cold temperatures with respect to respiration. While there is some variation in CT/TT responses at 25ºC, there aren't nearly enough data points at each day to pull out meaningful variation. The marginal significance could underlie the need for a larger sample size.

### Going forward

1. Clarify which crabs were actually sampled when and fix NAs in TTR data
5. Identify samples for gene expression and metabolomics
4. Examine HOBO data from 2023 experiment
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
