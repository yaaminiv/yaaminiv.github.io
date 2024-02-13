---
layout: post
comments: true
title: CEABiGR Part 11
tags: virginica CEABiGR gene-expression alt-splicing
---

## Predominant isoform shift model

We're wrapping up the CEABiGR paper! We're aiming for submission to a special issue by the end of the month, which means all analyses are (almost) finalized. In that spirit, I decided to remake my predominant isoform model. I've [already evaluated how low pH influences predominant isoform shift](https://yaaminiv.github.io/CEABiGR-Part8/) by examining if the shift happens more or less than what would be expected by chance. The next step is to understand how methylation influences this shift.

I returned to [this R Markdown script](https://github.com/sr320/ceabigr/blob/main/code/42-predominant-isoform.Rmd) to construct a model. I wanted to include measures of gene methylation, gene length (longer genes have a higher likelihood of alternative start/stop sites) and gene expression (higher gene expression has a higher chance of detecting a signal). I reformatted my existing dataframe to have these variables of interest, then calculated a change in gene body methylation or expression (exposed - control). Since predominant isoform already considers treatment in its definition, I didn't explicitly need treatment in my model. But, that also means I need indirect ways to account for treatment with methylation and expression:

```
predIsoFemWide <- femMethExpCV %>%
  select(., gene_name, treatment, change, avg_meth, avg_exp, length) %>%
  pivot_wider(names_from = treatment, values_from = c(avg_meth, avg_exp)) %>%
  replace(is.na(.), 0) %>%
  mutate(., change_meth = avg_meth_exposed - avg_meth_control) %>%
  mutate(., change_exp = avg_exp_exposed - avg_exp_control) #Take data and select gene name, change, treatment, average methylation, average expression, and length columns. Pivot wider, appending treatment information to names of the columns. Replace all NA with 0 (no expression or methylation detected). Calculate the change in methylation and expression.
```

After modifying the data, I ran a binomial model:

```
predIsoFemModel <- glm(I(change == 1) ~ change_meth +
                         log10(length) + log2(change_exp + 1),
                       family = binomial(),
                       data = predIsoFemWide) #Create a binomial model, where 1 = the predominant isoform shifted when comparing low and control pH conditions. Use change in methylation, gene length, and change in gene expression as explanatory variables
```

> Call:
glm(formula = I(change == 1) ~ change_meth + log10(length) +
    log2(change_exp + 1), family = binomial(), data = predIsoFemWide)
Coefficients:
                      Estimate Std. Error z value Pr(>|z|)    
(Intercept)          -8.488489   0.236370 -35.912   <2e-16 ***
change_meth          -0.004782   0.008119  -0.589    0.556    
log10(length)         1.449024   0.057612  25.152   <2e-16 ***
log2(change_exp + 1) -0.210410   0.018908 -11.128   <2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Change in DNA methylation does not significantly explain any difference in whether or not the predominant isoform shifted. As expected, gene length and gene expression are significant explanatory variables. I used `step` to remove the non-significant methylation term, then saved the output for the most parsimonious model (gene length and expression only) [here](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/fem-predo-isoform-model-output.csv).

I repeated the modeling exercise with the male data:

```
predIsoMaleModel <- glm(I(change == 1) ~ change_meth +
                         log10(length) + log2(change_exp + 1),
                       family = binomial(),
                       data = predIsoMaleWide) #Create a binomial model, where 1 = the predominant isoform shifted when comparing low and control pH conditions. Use change in methylation, gene length, and change in gene expression as explanatory variables
summary(predIsoMaleModel) #Only length and gene expression significant
```

> Call:
glm(formula = I(change == 1) ~ change_meth + log10(length) +
    log2(change_exp + 1), family = binomial(), data = predIsoMaleWide)
Coefficients:
                      Estimate Std. Error z value Pr(>|z|)    
(Intercept)          -3.105821   0.319937  -9.708  < 2e-16 ***
change_meth           0.014862   0.006697   2.219   0.0265 *  
log10(length)         0.349172   0.076841   4.544 5.52e-06 ***
log2(change_exp + 1)  0.050911   0.023499   2.167   0.0303 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Interestingly, change in methylation is significant for predominant isoform shifts in male samples. This is especially considering how there was no treatment influence on transcriptional noise (CV of gene expression). While predominant isoform shifts and transcriptional noise are not the same thing, I'd assume that changes in predominant isoform would be encapsulated by transcriptional noise. However, if there's a shift in predominant isoform but the overall gene expression level hasn't changed, perhaps that's why you can get a significant change in predominant isoform without a concurrent effect of treatment on CV of gene expression. I'll need to think on this interpretation a bit more.

### Going forward

1. Update methods and results
2. Compare with Aranda and Liew models in discussion section
2. Outline discussion
3. Write discussion
4. Write abstract
5. Outline introduction
6. Write introduction
7. Send to co-authors for approval
8. Submit to journal special issue

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
