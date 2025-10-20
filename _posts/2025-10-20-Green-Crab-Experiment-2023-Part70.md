---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 70
tags: green-crab-wc TTR mixed-effects-models
---

## Adding demographic information to respirometry analysis

I got my metabolomics and RNA-Seq data back a few months ago! Very exciting stuff, which means I need to make some progress on data analysis for a SICB presentation in January. To re-acquaint myself with the data, I'm going to start by applying the TTR and respirometry data analysis framework from my 2024 experiment paper to this dataset. All my analysis was done in [this script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/03-TTR-analysis.Rmd):

1. Mixed effects model with all demographic variables
2. Mixed effects model with significant demographic variables, temperature, day, and their interaction
3. Mixed effects models with significant demographic variables, significant treatment variables, and genotype information

The methods aren't new to me, so nothing noteworthy there. A few notes:

- Integument color was significant after multiple comparison correction when just looking at the demographic variables so I included it in the model with treatment variables
- Integument color was not significant in the model with treatment variables
- I ran an AIC comparison between the model with and without integument color, and AIC was slightly lower when integument color was not included
- Temperature, day, and their interaction were significant predictors of average TTR, but no other variables were
- This whole process sparked a question about whether or not I should use backwards deletion instead of this stepwise addition method. I spoke to Katie about this and then proceeded to look at Stack Overflow (links below). Turns out the general consensus is to *not* use stepwise addition or deletion approaches with mixed effects models since the p-value selection is not rigorous nor applicable to these kinds of models. I'll continue adding variables in batches and using LRT and AIC comparisons to determine which variables should be included in the model
  - https://stats.stackexchange.com/questions/58874/stepwise-introduction-of-predictors-to-mixed-effects-models
  - https://stats.stackexchange.com/questions/131123/mixed-models-and-backward-elimination
  
When I was [sorting through samples for metabolomics and RNA-Seq](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part59/), I was able to solve issues about some samples not having genotypes assigned to them in the script! Since I had the full genotype, I also was able to revise some figures and increase my sample size for TTR difference and Q10 analyses. Again, no noteworthy changes to report. At least now my methods are statistically sound!

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
