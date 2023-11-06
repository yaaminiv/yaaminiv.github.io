---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 26
tags: green-crab-wc time-to-right respirometry genotype
---

## Incorporating genotype into performance assays

Now that I [have most respirometry crab genotypes](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part25/), I can incorporate genotype data into my analyses!

### Time-to-right

I went back to [this R Markdown file](https://github.com/yaaminiv/wc-green-crab/blob/main/code/03-TTR-analysis.Rmd) to add genotype information. I didn't add genotype as a factor in the ANOVA because I only have data for 25 crabs. I did, however, create a new version of the individual difference in TTR plot with shapes defined by genotype:

<img width="1137" alt="Screenshot 2023-11-06 at 3 45 57 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/a019d5bf-4054-41a9-9a41-081822c741dc">

**Figure 1**. Difference between average TTR and individual TTR, with shapes indicating crab genotype

Nothing jumped out at me with genotypes in the 5ºC treatment, but at least I now have this information ready for a back-up slide in my PICES talk.

### Respirometry

Here's where it matters more, the respirometry analysis. In [this R Markdown file](https://github.com/yaaminiv/wc-green-crab/blob/main/code/04-respirometry-analysis.Rmd), I added demographic variables and genotype to my slope information for individual crabs. This was easier said than done. To assign these variables to individual crabs, I matched probe number in the respirometry dataset with probe number recorded in the TTR spreadsheet. The issue? Not every respirometry crab had probe number recorded in the Google Sheet! At first, I thought this just meant that some of the probe numbers recorded in the paper data sheets were not transferred over. And this was true for a few days. However, I was missing probe numbers for T1 on 7/5, and for T3 and T6 on 7/26. For 7/5, I assigned probe numbers numerically. For 7/26 I also assigned probe numbers numerically, but felt confident that this was actually the case because on 7/25 all probe numbers were assigned numerically. I'll just need to keep this in mind in case the individual patterns show crabs doing "weird" things on those days compared to other days.

I also noticed that for T8, 15-160 was the respirometry crab until days 28 and 42, when 15-163 was used instead. I vaguely remember this is because 15-160 died, and indeed, when I went to [my lab notebook post](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part13/) I saw that 15-160 died on 7/22/23. I updated the "dead date" column in my metadata spreadsheet. This also means that I need to get a genotype for 15-163.

I analyzed log-transformed blank-corrected slopes using an ANOVA with demographic variables, genotype, treatment, day, and treatment:day as explantory variables. Data was filtered such that any slopes with a blank ratio ≥ 0.10 were removed. I used `step` to identify the most parsimonious model:

```{r}
lmAllSlopeResultsSig <- aov(log(-slope_nmol_hr_corrected) ~ treatment + as.numeric(day) +
    missing.legs + final.genotype + treatment:as.numeric(day), data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Run the most significant model identified by step
summary(lmAllSlopeResultsSig) #Significant impact of treatment and treatment*day, and marginally significant impact of genotype
```

>                           Df Sum Sq Mean Sq F value Pr(>F)    
treatment                   2  51.36  25.682  99.231 <2e-16 ***
as.numeric(day)             1   0.68   0.679   2.623 0.1084    
missing.legs                1   0.20   0.197   0.760 0.3853    
final.genotype              2   1.08   0.540   2.085 0.1296    
treatment:as.numeric(day)   2   1.77   0.885   3.421 0.0365 *  
Residuals                 102  26.40   0.259                   
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
8 observations deleted due to missingness

Again, I noticed a significant influence of treatment:day without a significant day effect. I looked at the AIC for this model, this model - treatment:day, and this model - treatment:day - day, this model - treatment:day - missing legs, this mdoel - treatment:day - day - legs:

```
lmAllSlopeResultsSigMinusInt <- aov(log(-slope_nmol_hr_corrected) ~ treatment + as.numeric(day) +
    missing.legs + final.genotype, data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Same model, without interaction
lmAllSlopeResultsSigMinusDay <- aov(log(-slope_nmol_hr_corrected) ~ treatment +
    missing.legs + final.genotype, data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Same model, without interaction or day
lmAllSlopeResultsSigMinusLegs <- aov(log(-slope_nmol_hr_corrected) ~ treatment + day + final.genotype, data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Same model, without interaction or legs
lmAllSlopeResultsSigMinusLegsDay <- aov(log(-slope_nmol_hr_corrected) ~ treatment + final.genotype, data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Same model, without interaction, legs, day
AIC(lmAllSlopeResults, lmAllSlopeResultsSig, lmAllSlopeResultsSigMinusInt, lmAllSlopeResultsSigMinusLegs, lmAllSlopeResultsSigMinusDay, lmAllSlopeResultsSigMinusLegsDay) #Calculate AIC. Most parsimonious model (lowest AIC) is the model selected by step
```

>                                df AIC
lmAllSlopeResults	               18	185.0708		
lmAllSlopeResultsSig	           10	175.5846		
lmAllSlopeResultsSigMinusInt	    8	178.7917		
lmAllSlopeResultsSigMinusLegs	   11	178.7626		
lmAllSlopeResultsSigMinusDay	    7	179.2537		
lmAllSlopeResultsSigMinusLegsDay	6	178.7647

Interestingly, the most parsimonious model includes missing legs, day, and the interaction! So that's the one we'll go with. That means there's a significant influence of treatment and treatment:day, a marginal influence of day, and no influence of genotype or missing legs. Again, I used a Tukey HSD to look at pairwise differences in oxygen consumption by treatment:

```
TukeyHSD(lmAllSlopeResultsSig, "treatment", conf.level = 0.95) #Use TukeyHSD for categorical variables. Significant difference between all treatments
```

> Tukey multiple comparisons of means
    95% family-wise confidence level
Fit: aov(formula = log(-slope_nmol_hr_corrected) ~ treatment + as.numeric(day) + missing.legs + final.genotype + treatment:as.numeric(day), data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.1)))
$treatment
              diff         lwr        upr     p adj
25C-15C  0.3130756  0.02934812  0.5968030 0.0268387
5C-15C  -1.2440364 -1.54050808 -0.9475647 0.0000000
5C-25C  -1.5571120 -1.82903458 -1.2851894 0.0000000

Oxygen consumption was lower at 5ºC and higher at 25ºC. I plotted these data two ways. First, by treatment:

```{r}
all_MO2_slope_results_reorder %>%
  filter(., blank_ratio < 0.10) %>%
  ggplot(., aes(x = treatment, y = -slope_nmol_hr_corrected, color = treatment)) +
  geom_boxplot() + geom_point(aes(shape = treatment), size = 2) +
  xlab("") + ylab("Oxygen Consumption (nmol/hr)") +
  stat_summary(fun = median,
               geom = "line",
               aes(group = 1),
               col = "black", lty = 2) +
  facet_wrap(. ~ day, ncol = 3, labeller = labeller (day = facet_labels)) +
  scale_x_discrete(breaks = c("05C", "15C", "25C"),
                   labels = c("", "", "")) +
  scale_y_continuous(breaks = seq(0, 25000, 5000)) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  theme_classic(base_size = 15) + theme(axis.ticks.x = element_blank(),
                                        axis.title.x = element_blank(),
                                        strip.text.x = element_text(size = 15),
                                        strip.background = element_rect(colour = "white"))
ggsave("figures/multipanel-oxygen-consumption-corrected.pdf", height = 8.5, width = 11)
#Create a boxplot for oxygen consumption (µmol/hr). Use -slope_nmol_hr so the larger the slope, the faster the oxygen consumption over the hour. Add a line connecting each boxplot midpoint. Facet wrap and use the labeller to relabel facets. Add consistent color and shape scales.
```

<img width="1137" alt="Screenshot 2023-11-06 at 4 32 35 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/3bed6722-631b-438a-b23f-e219ddfd56a2">

**Figure 2**. Oxygen consumption by treatment throughout the experiment

Then, using genotype to set shapes:

```{r}
all_MO2_slope_results_reorder %>%
  filter(., blank_ratio < 0.10) %>%
  ggplot(., aes(x = treatment, y = -slope_nmol_hr_corrected, color = treatment)) +
  geom_boxplot() + geom_point(aes(shape = final.genotype), size = 2) +
  xlab("") + ylab("Oxygen Consumption (nmol/hr)") +
  stat_summary(fun = median,
               geom = "line",
               aes(group = 1),
               col = "black", lty = 2) +
  facet_wrap(. ~ day, ncol = 3, labeller = labeller (day = facet_labels)) +
  scale_x_discrete(breaks = c("05C", "15C", "25C"),
                   labels = c("", "", "")) +
  scale_y_continuous(breaks = seq(0, 25000, 5000)) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  scale_shape_manual(values = c(3, 4, 1, 2),
                     name = "Genotype",
                     breaks = c("CC", "CT", "TT"),
                     labels = c("CC", "CT", "TT")) +
  theme_classic(base_size = 15) + theme(axis.ticks.x = element_blank(),
                                        axis.title.x = element_blank(),
                                        strip.text.x = element_text(size = 15),
                                        strip.background = element_rect(colour = "white"))
ggsave("figures/multipanel-oxygen-consumption-corrected-genotype.pdf", height = 8.5, width = 11)
#Create a boxplot for oxygen consumption (µmol/hr). Use -slope_nmol_hr so the larger the slope, the faster the oxygen consumption over the hour. Add a line connecting each boxplot midpoint. Facet wrap and use the labeller to relabel facets. Add consistent color and shape scales.
```

<img width="1137" alt="Screenshot 2023-11-06 at 4 33 15 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/0047d550-c092-478a-b88a-4ecb228a4f26">

**Figure 3**. Oxygen consumption by treatment and genotype throughout the experiment

Just like with [the 2022 experiment](https://yaaminiv.github.io/Green-Crab-Experiment-Part19/), oxygen consumption is slower and less variable at 5ºC, and higher and more variable at 25ºC. However, after the one-month mark, it seems like variability in oxygen consumption at 25ºC decreases. Something to investigate after PICES.

Now I guess I gotta make a talk.

### Going forward

1. Prepare talk for PICES
2. Q10 analysis for 2023 experiment
3. Finish extracting respirometry samples (160, 163, and 35)
4. Continue with Chelex extractions, PCRs, and gels for TTR crabs
5. Update methods and results of 2022 paper
6. Examine HOBO data from 2023 experiment
7. Demographic data analysis for 2023 paper
8. Redo genotype incorporation with C or T as a factor instead of full genotype for respirometry analysis
8. Update methods and results of 2023 paper
9. Revisit Julia's genotypes

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
