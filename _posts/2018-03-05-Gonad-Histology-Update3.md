---
layout: post
comments: true
title: Gonad Histology Update 3
tags: manchester histology
---

## GLMs are neat

I forgot how useful all of the regression analysis techniques I learned last spring were until I had to use them for my own dataset! I met with Tim last week, who suggested I use a binomial GLM with a logit link to see if treatment affected maturation stage.

I created [this R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Manchester_Gonad_Histology/2018-02-27-Histology-Classification-Analyses.R) to analyze my [revised dataset](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/2018-02-27-Gigas-Histology-Classification.csv).

### Maturation stage

Any oyster with a maturation stage of 3 (ripe) or 4 (spawning/spent), I considered mature. I then created a new column that assigned 0 for immature and 1 for mature based on the maturation stages. I also renamed 

I then used stepwise addition to discern which covariates to put into my model. I started with three separate models to explain maturation: Mature ~ Treatment (ambient vs. low pH), Mature ~ modifiedSex (female vs. male vs. unripe), and Mature ~ Pre.or.Post.OA (pre-treatment vs. post-treatment sampling). The model that used sex to explain maturity was the most significant model, so that became my base. Using `add1`, I found that no other variable was significant and needed to be included in the model. Looking at the model summary, I saw that males were typically more mature than females.

### Sex ratios

For my post-treatment data, I restructured the data to make a contingency table with pH treatment as rows and sex classification (female vs. male vs. unripe) as columns. I then used a poisson GLM with a log link to analyze the contingency table with a chi-squared test for homogeneity. I failed to reject my null hypothesis.

### TL;DR

Treatment did not affect maturation. This makes sense with [my other results](https://yaaminiv.github.io/Reproductive-Output-Analyses/) that found egg production did not differ between low and ambient pH treatments. The only phenotype we have so far is lower hatch rates when a cross included a low pH female. I'm really interested to analyze my larval mortality data to see if there is a cohesive narrative.

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
