---
layout: post
comments: true
title: Mixed Effects Models
tags: manchester mixed-effect-models
---

## lmers and glmers

I got reviewer comments back on my Manchester paper! One of the reviewers suggested I used linear mixed effect models instead of a binomial GLM to evaluate gonad maturity before and after treatment. They also suggested I use a linear mixed effect model instead of a one-way ANOVA to look at differences in D-hinge count too. By using such a model, I can account for random effects, like experimental tank or sire.

To create a linear mixed model in R, I needed the `lme4` package. Within the package, I can use `lmer` for a linear mixed model and `glmer` for a generalized linear mixed model. The syntax is the same as `lm` or `glm`, but I can add random effects with the following term:

> y ~ x + (1 | RANDOM-VARIABLE)

### Gonad maturation

After talking to Tim and reviewing [this guide](https://ase.tufts.edu/gsc/gradresources/guidetomixedmodelsinr/mixed%20model%20guide.html), I tackled a generalized linear mixed effect model for gonad maturation differences before and after treatment. In my [data sheet](https://github.com/RobertsLab/paper-gigas-early-gametogenic-exposure/blob/master/data/2017-Adult-Gigas-Tissue-Sampling/2018-02-27-Gigas-Histology-Classification.csv), I added a column for tank. Pre-treatment oysters were all in the same tank, "Pre," and post-treatment oysters were coded based on their experimental tank. This is when I ran into a problem. Remember how my tissues got mixed up during processing so I had a bunch of unknown tissues from the ambient treatment? I added all of those to the same "unknown tank." While they didn't mix up any treatments, I actually have no way of tracing back the experimental tank for all but two of my ambient treatment tissues. If I cannot assign a tank to these tissues, a `glmer` that uses tank as a random effect may not be appropriate. I went through with the analysis in [this R script](https://github.com/RobertsLab/paper-gigas-early-gametogenic-exposure/blob/master/analyses/Gonad-Histology/2018-02-27-Histology-Classification-Analyses.R), I emailed Tim with my concerns and will update the code as necessary.

### D-hinge counts

I did something similar for my D-hinge counts. Instead of a `glmer`, I used a `lmer`. First, I specified the sire in a column in [this spreadsheet](https://github.com/RobertsLab/paper-gigas-early-gametogenic-exposure/blob/master/data/2017-07-30-Pacific-Oyster-Larvae/2018-02-14-Hatch-Rate-Data.csv). I used a `lmer` in [this R script]() and found that the variance for the random effect overlapped zero. This means that the random effect probably doesn't have much weight, and I can 1) pool by male treatment and 2) use a normal linear model or ANOVA. When looking at the `lmer`, I had an chi-squared p-value of 0.02445, meaning there were significant differences between all four treatments. I would need to unpack this more to figure out which pairwise differences were significant. I also used a `lmer` when pooling male treatments, and found that once again, the variance for the random effect (sire) overlapped zero. My ANOVA p-value was 0.00271.

Now that I understand how to use `lmer` and `glmer`, I need to understand how to interpret my results. I think a little more reading will help me with this.

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
