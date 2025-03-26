---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 22
tags: green-crab-cold TTR mixed-effects-models
---

## Analyzing WA crab TTR data

I'm analyzing the TTR data from WA crabs! I'm using the [same methods I used for my MA crabs](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part17/), which means I have a couple of things I will need to sort out:

- how to properly assess the significance of an interaction term
- convergence/rescaling errors for the mixed effects binomial model

I need to find someone to talk to about mixed effects models! But I figured I can at least get some preliminary data to think about.

Since the [WA code](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/03-TTR-analysis-WA.Rmd) is identical to the [MA code](https://github.com/yaaminiv/cold-green-crab/blob/main/code/MA/03-TTR-analysis-MA.Rmd), I'll just roll through the results and figures.

### Relevant links

I did some repo reorganization! Each subdirectory has additional subdirectories for MA or WA materials. I anticipate I'll have a third folder when I do population comparisons.

- MA
  - [data](https://github.com/yaaminiv/cold-green-crab/blob/main/data/MA/)
  - [code](https://github.com/yaaminiv/cold-green-crab/blob/main/code/MA/)
  - [output](https://github.com/yaaminiv/cold-green-crab/blob/main/output/MA/)
- WA
  - [data](https://github.com/yaaminiv/cold-green-crab/blob/main/data/WA/)
  - [code](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/)
  - [output](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/)

### Average TTR anlaysis

I used linear mixed effects models to assess the influence of temperature (1.5ºC or 5ºC), timepoint, and their interaction on TTR. I used crab ID as a random effect:

```
Linear mixed model fit by maximum likelihood  ['lmerMod']
Formula: log(TTRavg) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) +  
    integument.cont + carapace.width + weight + as.factor(missing.swimmer) +  
    (1 | crab.ID)
   Data: (modTTR %>% filter(., is.na(TTRavg) == FALSE))

     AIC      BIC   logLik deviance df.resid
   670.9    711.8   -324.5    648.9      293

Scaled residuals:
    Min      1Q  Median      3Q     Max
-2.5501 -0.6795 -0.1799  0.6739  3.2051

Random effects:
 Groups   Name        Variance Std.Dev.
 crab.ID  (Intercept) 0.02357  0.1535  
 Residual             0.47326  0.6879  
Number of obs: 304, groups:  crab.ID, 81

Fixed effects:
                            Estimate Std. Error t value
(Intercept)                  0.78967    0.94339   0.837
treatment5C                 -0.05283    0.12599  -0.419
timepoint                    0.73700    0.03610  20.414
as.factor(sex)M              0.07569    0.09696   0.781
integument.cont              0.01238    0.10736   0.115
carapace.width              -0.01821    0.02320  -0.785
weight                       0.01423    0.01194   1.192
as.factor(missing.swimmer)Y  0.06326    0.51017   0.124
treatment5C:timepoint       -0.42976    0.05027  -8.549

Correlation of Fixed Effects:
            (Intr) trtm5C timpnt as.()M intgm. crpc.w weight a.(.)Y
treatment5C -0.049                                                 
timepoint   -0.114  0.482                                          
as.fctr(s)M  0.197 -0.022  0.055                                   
intgmnt.cnt -0.172  0.096 -0.039  0.020                            
carapc.wdth -0.955 -0.043  0.037 -0.189  0.014                     
weight       0.643  0.053  0.011 -0.053 -0.105 -0.798              
as.fctr(.)Y  0.064 -0.007  0.044  0.002  0.009 -0.035 -0.067       
trtmnt5C:tm  0.036 -0.713 -0.697 -0.017 -0.084  0.012  0.029 -0.009
```

Similar to the MA crabs, crab ID explained very little of the overall variance! I then used ANOVA to compare models with or without the term of interest (temperature, timepoint, interaction). I created a table with my ANOVA output and saved the table [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/ttr-model-comparison-stat-output.csv). All three factors were significant!

I topped everything off with a lovely boxplot.

<img width="1138" alt="Screenshot 2024-08-20 at 5 37 19 PM" src="https://github.com/user-attachments/assets/6f67985c-79b1-4dea-9798-1d39bc2a6e88">

**Figure 1**. Average TTR at 1.5ºC and 5ºC for the duration of the experiment

Unlike the MA crabs, the WA crabs got slower and slower as the experiment went on! They didn't have the stabilization or recovery after 22 hours the way the MA crabs did. This population-level difference is really interesting to see, and I'm excited to dig into the genotype data (if I can get those extractions to work RIP) to see if that explains any of the differences.

### Threshold analysis

The next thing I did was analyze the flip vs. no flip data. Anecdotally, the MA populations had way more crabs that did not right themselves after 91 seconds than the WA population. I used a mixed effects binomial model, then followed up with ANOVA to assess the significance of temperature, timepoint, and their interaction. I saved the model output [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/ttr-binomial-model-comparison-output.csv). Similar to the MA crabs, the interaction term was not significant, but the other two terms are significant. I got the following error for each model comparison:

```
Warning in checkConv(attr(opt, "derivs"), opt$par, ctrl = control$checkConv,  :
  Model is nearly unidentifiable: large eigenvalue ratio
 - Rescale variables?
```

Since crab ID explains such little variance, I wonder how much I need to include individual information as a random effect? I guess I technically need to because assumptions, but if it explained such little variance in the average TTR model, is it really necessary? Again, I really gotta talk to someone about this.

Once again I finished off the analysis by making a stacked barplot visualizing the number of crabs that did or did not right within 91 seconds at each timepoint x trial.

<img width="1138" alt="Screenshot 2024-08-20 at 5 37 29 PM" src="https://github.com/user-attachments/assets/2fda1794-37cb-40ab-9f86-4898586e90c3">

**Figure 2**. Proportion of crabs that did not flip in the first, second, or third trials at each time point.

The visualization confirms the anecdotal vibes: fewer crabs were unable to right in the WA population. Once again, I find myself cursing Chelex and my inability to get genotype data prior to the experiment.

### Going forward

1. Troubleshoot genotyping
2. Clarify methods for average TTR analysis (reach out to Andy, Nic, or Megan?)
2. Individual-level TTR data analysis
3. Determine methods for comparing population responses
3. Troubleshoot lipid assay protocol
5. Conduct lipid assay for crabs of interest

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
