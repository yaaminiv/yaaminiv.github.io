---
layout: post
comments: true
title: CEABiGR Part 10
tags: virginica CEABiGR gene-expression alt-splicing
---

## Polynomial terms in transcriptional noise analysis

When I presented my [transcriptional noise models](https://yaaminiv.github.io/CEABiGR-Part9/) to Katie, she noticed some curvature in the FPKM vs. transcriptional noise plot for the male oysters. Based on this, she suggested adding a quadratic term to the male model, and doing the same with the female model to be consistent. I tested the following model for each sex in [this script](https://github.com/sr320/ceabigr/blob/main/code/42-predominant-isoform.Rmd):

log10(CV expression) ~ methylation + log2(expression)<sup>2</sup> + log2(expression) + log10(CV methylation) + gene length + treatment + methylation*treatment + log2(expression)<sup>2</sup>*treatment + log2(expression)*treatment

### Female model

I ran the model with the quadratic term and the interaction between the quadratic term and treatment, then compared this model with the ones I ran previously using AIC scores.

```
transNoiseFem <- lm(data = femMethExpCV,
                    log10(CoV_FPKM) ~ avg_meth + log10(CoV_Meth) + I((log2(avg_exp + 1)^2)) + log2(avg_exp + 1) + log10(length) + as.factor(treatment) + as.factor(treatment)*avg_meth + as.factor(treatment)*I((log2(avg_exp + 1)^2)) + as.factor(treatment)* log2(avg_exp + 1)) #Model with quadratic relationship between expression and transcriptional noise
```

```
transNoiseFem2 <- lm(data = femMethExpCV,
                     log10(CoV_FPKM) ~ avg_meth + log10(CoV_Meth) + log2(avg_exp + 1) + log10(length) + as.factor(treatment) + as.factor(treatment)*avg_meth + as.factor(treatment)*log2(avg_exp + 1)) #Try model without polynomial relationship between expression and transcriptional noise
```

```
transNoiseFem3 <- lm(data = femMethExpCV,
                     log10(CoV_FPKM) ~ avg_meth + log10(CoV_Meth) + log2(avg_exp + 1) + as.factor(treatment) + as.factor(treatment)*log2(avg_exp + 1)) #Remove insignificant terms from transNoiseFem2
```

```
AIC(transNoiseFem, transNoiseFem2, transNoiseFem3) #Compare AIC between models.
```

```
               df       AIC
transNoiseFem  11 -26682.88
transNoiseFem2  9 -22303.35
transNoiseFem3  7 -22305.01
```

The first model with the quadratic relationship between expression and transcriptional noise had the lowest AIC, so I used that as the final model.

```
Call:
lm(formula = log10(CoV_FPKM) ~ avg_meth + log10(CoV_Meth) + I((log2(avg_exp +
    1)^2)) + log2(avg_exp + 1) + log10(length) + as.factor(treatment) +
    as.factor(treatment) * avg_meth + as.factor(treatment) *
    I((log2(avg_exp + 1)^2)) + as.factor(treatment) * log2(avg_exp +
    1), data = femMethExpCV)

Residuals:
     Min       1Q   Median       3Q      Max
-1.07986 -0.13092 -0.00255  0.12807  0.91221

Coefficients:
                                                       Estimate Std. Error t value Pr(>|t|)    
(Intercept)                                           1.415e-01  7.180e-03  19.712  < 2e-16 ***
avg_meth                                             -3.847e-03  6.830e-05 -56.325  < 2e-16 ***
log10(CoV_Meth)                                       1.035e-01  2.921e-03  35.439  < 2e-16 ***
I((log2(avg_exp + 1)^2))                              1.058e-02  2.952e-04  35.849  < 2e-16 ***
log2(avg_exp + 1)                                    -1.042e-01  2.035e-03 -51.189  < 2e-16 ***
log10(length)                                         7.139e-03  1.879e-03   3.799 0.000146 ***
as.factor(treatment)exposed                          -8.623e-03  2.716e-03  -3.174 0.001502 **
avg_meth:as.factor(treatment)exposed                  6.376e-04  8.585e-05   7.427 1.13e-13 ***
I((log2(avg_exp + 1)^2)):as.factor(treatment)exposed  6.181e-03  4.152e-04  14.888  < 2e-16 ***
log2(avg_exp + 1):as.factor(treatment)exposed        -7.494e-02  2.866e-03 -26.147  < 2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.1966 on 64224 degrees of freedom
Multiple R-squared:  0.5973,	Adjusted R-squared:  0.5973
F-statistic: 1.059e+04 on 9 and 64224 DF,  p-value: < 2.2e-16
```

Interestingly, adding the quadratic terms to the model meant that all input explanatory variables were now significant! The model output can be found [here](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/transcriptional-noise/fem-model-output.csv). I also [calculated partial R<sup>2</sup> values](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/transcriptional-noise/fem-model-partialR.csv). Gene methylation still has the highest partial R<sup>2</sup> (0.47), followed by the model residuals (0.40).

### Male model

I went through the same process for the male model:

```
transNoiseMale <- lm(data = maleMethExpCV,
                     log10(CoV_FPKM) ~ avg_meth + log10(CoV_Meth) + I((log2(avg_exp + 1)^2)) + log2(avg_exp + 1) + log10(length) + as.factor(treatment) + as.factor(treatment)*avg_meth + as.factor(treatment)*I((log2(avg_exp + 1)^2)) + as.factor(treatment)* log2(avg_exp + 1)) #Model with polynomial relationship between expression and transcriptional noise
```

```
transNoiseMale2 <- lm(data = maleMethExpCV,
                      log10(CoV_FPKM) ~ avg_meth + log10(CoV_Meth) + log2(avg_exp + 1) + log10(length) + as.factor(treatment) + as.factor(treatment)*avg_meth + as.factor(treatment)*log2(avg_exp + 1)) #Model without polynomial relationship between expression and transcriptional noise
```

```
transNoiseMale3 <- lm(data = maleMethExpCV,
                      log10(CoV_FPKM) ~ avg_meth + log10(CoV_Meth) + log2(avg_exp + 1) + as.factor(treatment) + as.factor(treatment)*avg_meth) #Remove insignificant terms from transNoiseMale2 but keep treatment
```

```
transNoiseMale4 <- lm(data = maleMethExpCV,
                      log10(CoV_FPKM) ~ avg_meth + log10(CoV_Meth) + log2(avg_exp + 1) + as.factor(treatment)*avg_meth) #Remove treatment from transNoiseMale3
```

```
AIC(transNoiseMale, transNoiseMale2, transNoiseMale3, transNoiseMale4)
```

```
               df         AIC
transNoiseMale  11 -4290.92845
transNoiseMale2  9    90.65897
transNoiseMale3  7    87.75041
transNoiseMale4  7    87.75041
```

Once again, the model with the quadratic terms was had the lowest AIC score, and all explanatory variables were significant.

```
Call:
lm(formula = log10(CoV_FPKM) ~ avg_meth + log10(CoV_Meth) + I((log2(avg_exp +
    1)^2)) + log2(avg_exp + 1) + log10(length) + as.factor(treatment) +
    as.factor(treatment) * avg_meth + as.factor(treatment) *
    I((log2(avg_exp + 1)^2)) + as.factor(treatment) * log2(avg_exp +
    1), data = maleMethExpCV)

Residuals:
     Min       1Q   Median       3Q      Max
-1.77765 -0.10835  0.01416  0.12913  0.82052

Coefficients:
                                                       Estimate Std. Error t value Pr(>|t|)    
(Intercept)                                           4.275e-01  2.224e-02  19.227  < 2e-16 ***
avg_meth                                             -1.397e-03  9.387e-05 -14.878  < 2e-16 ***
log10(CoV_Meth)                                       1.447e-01  5.269e-03  27.457  < 2e-16 ***
I((log2(avg_exp + 1)^2))                              6.750e-02  1.441e-03  46.837  < 2e-16 ***
log2(avg_exp + 1)                                    -4.098e-01  8.561e-03 -47.865  < 2e-16 ***
log10(length)                                        -2.699e-02  4.233e-03  -6.375 1.87e-10 ***
as.factor(treatment)exposed                           1.466e-01  1.878e-02   7.806 6.19e-15 ***
avg_meth:as.factor(treatment)exposed                  7.033e-04  9.956e-05   7.064 1.67e-12 ***
I((log2(avg_exp + 1)^2)):as.factor(treatment)exposed  1.547e-02  2.135e-03   7.248 4.40e-13 ***
log2(avg_exp + 1):as.factor(treatment)exposed        -9.521e-02  1.270e-02  -7.499 6.73e-14 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.2171 on 19835 degrees of freedom
Multiple R-squared:  0.3763,	Adjusted R-squared:  0.3761
F-statistic:  1330 on 9 and 19835 DF,  p-value: < 2.2e-16
```

I was surprised that treatment was a significant part of the model, since visually it doesn't look like there's a difference in transcriptional noise between treatments. The rest of the model output can be found [here](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/transcriptional-noise/male-model-output.csv). The residuals had the highest partial R<sup>2</sup> (0.62), and gene methylation had the highest partial R<sup>2</sup> of any input variable (0.16). The partial R<sup>2</sup> table can be found [here](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/transcriptional-noise/male-model-partialR.csv).

### Lingering questions

What does having a quadratic relationship between gene expression and transcriptional noise even mean? For female and male oysters, very low and very high gene expression is associated with increased transcriptional noise. However, when gene expression is somewhere in the middle, transcriptional noise is lower. Maybe when gene expression is lower, there's less incentive to regulate transcriptional noise because expression is low to begin with. But as expression level increases, it may be beneficial to regulate which transcripts are in play. When gene expression is high, perhaps transcriptional noise reduces because the cell requires specialized transcript to fulfill various housekeeping roles. I need to think about this explanation more.

### Going forward

1. Share revised model output with Katie
2. Update methods and results
3. Run separate models for housekeeping vs. environmental response genes
1. Compare with Aranda and Liew models
3. Investigate other layers of predominant isoform analysis
3. Remove EpiDiverse/SNP C->T SNPs and rerun analyses
4. Preliminary mQTL analysis
2. Tune sPLS parameters
3. Re-run sex-specific SPLS
4. Dig into functional annotations for genes in networks

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
