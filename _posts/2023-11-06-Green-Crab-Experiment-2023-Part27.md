---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 26
tags: green-crab-wc respirometry genotype
---

## Modifying respirometry analysis

Now that PICES is over, I want to revisit a few things regarding the respirometry analysis.

### Genotype

Carolyn suggested I look at genotype using binary variables for whether or not the crab has at least one C or T allele. This assumes that having at least one allele would be enough to impact physiological response. I returned to [this R Markdown script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/04-respirometry-analysis.Rmd) to re-run the ANOVA. I decided to run an ANOVA with the demographic variables, binary genotype variables, temperature, day and the interaction between the two. After using `step` to select the most parsimonious model with binary genotype data, I could compare the best-fit models from the full genotype model and binary genotype model. This will give me one final model........to rule them all.

First, I ran the ANOVA and stepwise backwards deletion:

```
lmBinarySlopeResults <- aov(log(-slope_nmol_hr_corrected) ~ treatment*as.numeric(day) + sex + weight + size + integument.color + missing.legs + hasC + hasT, data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Examine the influence of treatment, day, and various demographic factors on respiration rate. Remove outliers based on blank ratios prior to analysis.
summary(lmBinarySlopeResults) #Significant impact of treatment, treatment:day, and hasT on respiration
```

>                           Df Sum Sq Mean Sq F value Pr(>F)    
treatment                  2  51.36  25.682  96.975 <2e-16 ***
as.numeric(day)            1   0.68   0.679   2.563  0.113    
sex                        1   0.18   0.179   0.677  0.413    
weight                     1   0.01   0.007   0.025  0.876    
size                       1   0.05   0.047   0.178  0.674    
integument.color           5   0.55   0.110   0.416  0.836    
missing.legs               1   0.16   0.161   0.609  0.437    
hasC                       1   0.12   0.122   0.460  0.499    
hasT                       1   1.07   1.073   4.052  0.047 *  
treatment:as.numeric(day)  2   2.41   1.206   4.552  0.013 *  
Residuals                 94  24.89   0.265                   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
8 observations deleted due to missingness

```
lmBinarySlopeResultsStep <- step(lmBinarySlopeResults, direction = "backward") #Use backwards-deletion approach to identify the most significant model (lowest AIC)
summary(lmBinarySlopeResultsStep) #The most significant model is one that includes treatment, day, the interaction, and hasT
```

```
lmBinarySlopeResultsSig <- aov(log(-slope_nmol_hr_corrected) ~ treatment + as.numeric(day) +
                                 hasT + treatment:as.numeric(day), data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Run the most significant model identified by step
summary(lmBinarySlopeResultsSig) #Significant impact of treatment and treatment*day, and marginally significant impact of genotype
```

>                            Df Sum Sq Mean Sq F value Pr(>F)    
treatment                   2  51.36  25.682  98.422 <2e-16 ***
as.numeric(day)             1   0.68   0.679   2.601 0.1098    
hasT                        1   0.72   0.721   2.763 0.0995 .  
treatment:as.numeric(day)   2   1.59   0.794   3.041 0.0520 .  
Residuals                 104  27.14   0.261                   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
8 observations deleted due to missingness

Once again, treatment:day was included as a significant variable when day was not significant. So I ran the `step` model - treatment:day and the model - treatment:day - day and calculated AIC:

```
lmBinarySlopeResultsSigMinusInt <- aov(log(-slope_nmol_hr_corrected) ~ treatment + as.numeric(day) +
                                         hasT, data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Significant model - interaction
lmBinarySlopeResultsSigMinusDay <- aov(log(-slope_nmol_hr_corrected) ~ treatment +
                                         hasT, data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Significant model - interaction - day
AIC(lmBinarySlopeResults, lmBinarySlopeResultsSig, lmBinarySlopeResultsSigMinusInt, lmBinarySlopeResultsSigMinusDay) #Calculate AIC. Most parsimonious model is the one identified by step
```

>                              df AIC
lmBinarySlopeResults	         18	185.0708		
lmBinarySlopeResultsSig	        8	174.6480		
lmBinarySlopeResultsSigMinusInt	6	176.9574		
lmBinarySlopeResultsSigMinusDay	5	177.6132

The best-fit model is the one picked by `step`! There was a significant impact of treatment and treatment*day, and marginally significant impact of day the presence of the T allele. I then conducted post-hoc tests for treatment, day, and presence of the T allele.

```
TukeyHSD(lmBinarySlopeResultsSig, "treatment", conf.level = 0.95) #Use TukeyHSD for categorical variables. Significant difference between all treatments
```

> Tukey multiple comparisons of means
    95% family-wise confidence level
Fit: aov(formula = log(-slope_nmol_hr_corrected) ~ treatment + as.numeric(day) + hasT + treatment:as.numeric(day), data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.1)))
$treatment
              diff        lwr        upr     p adj
25C-15C  0.3130756  0.0282661  0.5978851 0.0275407
5C-15C  -1.2440364 -1.5416387 -0.9464341 0.0000000
5C-25C  -1.5571120 -1.8300716 -1.2841524 0.0000000

Oxygen consumption was significantly lower in the 5ºC treatment, and significantly higher in the 25ºC treatment.

```
pairwise.t.test(x = log(-(all_MO2_slope_results_demTTRdata_genotype$slope_nmol_hr_corrected)), all_MO2_slope_results_demTTRdata$day, p.adjust.method = "bonferroni") #Use pairwise t-test for quantitative variables. Marginal significance between days 49 and all other days.
```

>	Pairwise comparisons using t tests with pooled SD
data:  log(-(all_MO2_slope_results_demTTRdata_genotype$slope_nmol_hr_corrected)) and all_MO2_slope_results_demTTRdata$day
   07      14      21      28      42     
14 1.00000 -       -       -       -      
21 1.00000 1.00000 -       -       -      
28 1.00000 1.00000 1.00000 -       -      
42 1.00000 1.00000 1.00000 1.00000 -      
49 0.00627 0.00154 0.00031 0.00107 5.6e-05

There is a significant difference between day 49 and the other days of the experiment. This is probably because day 49 only had 5ºC measurements, so the marginal significance of day is not a real phenomenon.

```
TukeyHSD(lmBinarySlopeResultsSig, "hasT", conf.level = 0.95) #Use TukeyHSD for categorical variables. Significant difference between all treatments
```

> Tukey multiple comparisons of means
    95% family-wise confidence level
Fit: aov(formula = log(-slope_nmol_hr_corrected) ~ treatment + as.numeric(day) + hasT + treatment:as.numeric(day), data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.1)))
$hasT
         diff         lwr       upr     p adj
Y-N 0.1899553 -0.05553223 0.4354427 0.1279555

Oxygen consumption was higher when the crab had a T allele. I need to review if the T allele is the putative cold or warm tolerance allele so I can interpret this better.

### Variability in oxygen consumption

One thing I did for the 2022 dataset was [quantify SD and CV](https://yaaminiv.github.io/Green-Crab-Experiment-Part19/) for each treatment x day combination.

<img width="1137" alt="Screenshot 2023-11-06 at 9 32 32 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/be12980e-287d-4458-b2d2-321b7a44adc8">

<img width="1137" alt="Screenshot 2023-11-06 at 9 32 41 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/2c9ab0b4-3465-4a6f-b5d3-ecab1422bcb4">

**Figures 1-2**. SD and CV for each treatment x day combination.

Variability for the 15ºC and 25ºC treatments are much higher, with the 25ºC having the highest variability at day 14. CV for all three treatments is closest on days 14 and 28, and that the 15ºC has a much higher CV than the 25ºC treatment at day 21. Also, CV for the 5ºC is highest on day 49, which is the first day there's an outlier in the 5ºC treatment. These patterns seem to be more influenced by potential outliers.

### Q10 analysis

<img width="1137" alt="Screenshot 2023-11-06 at 9 32 52 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/54938992-07f7-487e-8610-2a1fba39d1dd">

**Figure 3**. Pairwise Q10 for at each day oxygen consumption was measured.

Just like with the 2022 data, Q10 was lower for the 15ºC-25ºC comparison and higher for the 5ºC-15ºC comparison. This suggests that respiration is more temperature-dependent at 5ºC, and could involve more biochemical remodeling.

### Going forward

1. Finish extracting respirometry samples (160, 163, and 35)
2. Continue with Chelex extractions, PCRs, and gels for TTR crabs. Update methods and results of 2022 paper
3. Examine HOBO data from 2023 experiment
4. Demographic data analysis for 2023 paper
6. Update methods and results of 2023 paper
7. Revisit Julia's genotypes

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
