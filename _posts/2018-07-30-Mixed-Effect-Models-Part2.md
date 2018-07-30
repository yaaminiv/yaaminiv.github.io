---
layout: post
comments: true
title: Mixed Effects Models
tags: manchester mixed-effect-models
---

## Assumptions and p-values

[Last week](https://yaaminiv.github.io/Mixed-Effect-Models/), I tackled reviwer comments and used mixed effect models for my analyses. I was right to be concerned about using a glmer with tank as a random effect without any way to assign tanks to some tissues. I reverted back to my binomial GLM. Although I obtained a p-value for my lmer analysis, I wasn't sure if I was going about it correctly. After some Googling, I found a [tutorial](http://www.bodowinter.com/tutorial/bw_LME_tutorial2.pdf) from Bodo Winter at UC Merced for linear mixed effect models! It has very clear instructions for constructing mixed effects models with `lme4` and for using likelihood ratio tests for p-values.

### Likelihood Ratio Tests

In [this R script](https://github.com/RobertsLab/paper-gigas-early-gametogenic-exposure/blob/master/analyses/Larval-Abundance/2018-02-14-Reproductive-Output.R), I modified my code to use likelihood ratio tests instead of an ANOVA workaround. To do this, I needed to build a null model without the fixed effect of interest (either Parental.Treatment or Female.Treatment), and a full model with the effect of interest. I then used an ANOVA to compare both models and get a p-value. A significant p-value means that the effect is part of the most parsimonious model.

When I examined all treatments, I found that parental treatment affected D-hinge counts (Chi-squared = 9.1878, df = 3, p = 0.0269). D-hinge counts were -0.15795 ± 0.10468 lower for low pH female-ambient male and -0.15795 ± 0.10509 lower for low pH female-low pH male. While I have a significant p-value for this model, there is still evidence of a maternal effect. I investigated this further with my second lme using Female.Treatment instead of Parental.Treatment. Female treatment affected D-hinge counts (Chi-squared = 8.1781, df = 1, p = 0.00424). D-hinge counts were -0.21090 ± 0.07033 lower for families with low pH females.

### Assumptions

I needed to check the following assumptions for these models:

1. Normality of residuals
2. Linearity (because *linear* mixed effect models)
3. Homoskedasticity

To check residual normality, I used `qqnorm` and `qqline`. Visual inspection did not reveal any obvious violations of the normality assumption, since the data fell on a straight line. I plotted residuals vs. fitted values to check for linearity and homoskedasticity. There was no heteroskedasticity in my residual plots, but I did see striped patterns.

![untitled](https://user-images.githubusercontent.com/22335838/43427117-1337369e-940d-11e8-8d28-79c76335534d.png)

**Figure 1**. Residual plot for Female.Treatment model.

I'm not sure what this means, and neither Google nor my QSCI 483 notes were of any help. [This guide](https://ase.tufts.edu/gsc/gradresources/guidetomixedmodelsinr/mixed%20model%20guide.html) suggests that such a pattern in my residual plot is acceptable, but [this Bodo Winter tutorial](http://www.bodowinter.com/tutorial/bw_LME_tutorial1.pdf) does not. I might bring it up at lab meeting or ask Tim or Julian for some help. My gut feeling is that there's some caveat to residual analysis with mixed effects models that I'm missing.

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

<script id="dsq-count-scr" src="//the-responsible-grad-student.disqus.com/count.js" async></script>=
