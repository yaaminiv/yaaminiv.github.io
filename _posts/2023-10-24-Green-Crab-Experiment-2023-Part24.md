---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 24
tags: green-crab-wc survival-analysis time-to-right respirometry
---

## Preliminary analyses for 2023 experiment

I need to put my PICES talk together! Which means I actually need to analyze the performance assay data from this summer's experiment. Thankfully, I'll be using a lot of the code I already wrote for [the 2022 experiment](https://github.com/yaaminiv/green-crab-metabolomics).

### Survival analysis

First thing I needed to do was look at the [survivorship data](https://github.com/yaaminiv/wc-green-crab/blob/main/data/survivorship.csv)! In [this R Markdown file](https://github.com/yaaminiv/wc-green-crab/blob/main/code/02-demographic-data.Rmd), I conducted a Kaplan-Meier survival analysis. I kept some of my code from last summer's demographic data analysis in case I wanted to return to it later, but I didn't think the PICES talk needed to include any of that information.

Anyways, survivorship analysis. I first with a Cox regression model to look at overall differences in survival due to temperature.

```
coxModel <- mortalityDataLong %>%
  coxph(Surv(time, status, type = "right") ~ treatment, data = .) #Use a Cox regression model to investigate survival differences by treatment.
coxModel #No significant difference between treatments
```

>                 coef exp(coef) se(coef)      z     p
> treatment30C  0.7787    2.1787   0.7072  1.101 0.271
> treatment5C  -0.4520    0.6363   0.9129 -0.495 0.621
> Likelihood ratio test=2.87  on 2 df, p=0.2385
> n= 137, number of events= 11

As we suspected when we reduced the temperature to 25ºC from 30ºC, there was no significant difference in survivorship between the three temperature treatments!

```
summary(coxModel) #Confidence interval information for each treatment as compared to the base condition (15)
```

>              exp(coef) exp(-coef) lower .95 upper .95
> treatment30C    2.1787      0.459    0.5448     8.713
> treatment5C     0.6363      1.571    0.1063     3.809

Then, I got the hazards ratio (HR) with respect to the 15ºC control from the `exp(coef)` column in the model summary. A HR > 1 means an increase in the hazard when compared to the control. There is an 2.18-fold increase in death as the experiment progresses for the 25ºC treatment, while only a 0.63 increase in death for the 5ºC treatment. However, these are not significantly different than what is expected for the 15ºC treatment.

```
mortalityDataLong %>%
  survfit2(Surv(time, status, type = "right") ~ treatment, data = .) %>%
  ggsurvfit(linewidth = 1, linetype_aes = TRUE) +
  scale_color_manual(values = c(plotColors[2], plotColors[1], plotColors[3]),
                     labels = c("15", "25", "5"),
                     name = "Temperature (ºC)") +
  scale_linetype_manual(values = c(1, 6, 4),
                        labels = c("15", "25", "5"),
                        name = "Temperature (ºC)") +
  scale_x_continuous(breaks = c(seq(0, 45, 5), 49)) +
  scale_y_continuous(limits = c(0, 1), breaks = seq(0, 1, 0.2)) +
  labs(x = "Days", y = "Overall Survival Probality") +
  annotate(geom = "text", x = -1, y = 0.2,
           label = expression(paste(bolditalic("Overall: "), Chi^2, " = 3.00, p = 0.20")),
           hjust = 0) +
  annotate(geom = "text", x = -1, y = 0.15,
           label = expression(paste(bolditalic("15ºC vs. 25ºC: "), "HR = 2.18, p = 0.27")),
           hjust = 0) +
  annotate(geom = "text", x = -1, y = 0.1,
           label = expression(paste(bolditalic("15ºC vs. 25ºC: "), "HR = 0.63, p = 0.62")),
           hjust = 0) +
  theme_classic(base_size = 15) #Plot KM curves based on the Surv object analyzing survival time by treatment, with "right" indicating the kind of censoring (samples still alive at the end of the experiment were "right" censored). Customize line width, line type, colors, legend, axes, and labels. Add annotations for statistics. Use default R plotting theme with a base font size of 15.
ggsave("figures/KM-curves.pdf", height = 8.5, width = 11)
```

To visually confirm that there was no difference in survivorship between the treatments, I plotted Kaplan-Meier survivorship curves.

<img width="1138" alt="Screenshot 2023-11-06 at 2 16 40 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/8f58ef50-4071-4726-900b-3c4938ff2f7a">

**Figure 1**. Kaplan Meier survivorship curves for crabs at 5ºC, 15ºC, and 25ºC

It's wild to see that the crabs were able to sustain basal functions for 6-7 weeks at more "extreme" temperatures without any change in survival.

### TTR analysis

Next, [TTR data](https://github.com/yaaminiv/wc-green-crab/blob/main/data/time-to-right.csv)! I used [this R Markdown file](https://github.com/yaaminiv/wc-green-crab/blob/main/code/03-TTR-analysis.Rmd) for the analysis. Similar to the 2022 experiment, I analyzed the data in two ways: 1) differences in average TTR for each treatment x day combination, and 2) a thresholding analysis. For the first approach, I used an ANOVA to look at differences in log-transformed average TTR with various explanatory variables:

```
TTRmodel <- aov(log(TTRavg) ~ treatment + day + treatment*day + as.factor(sex) + as.factor(integument.color) + size + weight + as.factor(missing.swimmer), data = modTTR) #ANOVA, with log(TTRavg) as the response variable, and treatment, day, the interaction between treatment and day, sex, integument color, size, weight, and whether or not a crab is missing legs as explanatory variables.
```

Then, I used `step` to implement a backwards deletion approach to find the most parsimonious model:

```
TTRmodelStep <- step(TTRmodel, direction = "backward") #Use step-wise backwards selection to identify the best fit model
```

And ran the most parsimonious model:

```
TTRmodelSig <- aov(log(TTRavg) ~ treatment + day + as.factor(integument.color) + size + weight + treatment:day, data = modTTR) #Run the most significant model identified by step
summary(TTRmodelSig) #Significant impact of treatment, treatment x day, weight, and integument color. Marginally significant influence of size. No impact of day alone
```

>                              Df Sum Sq Mean Sq F value   Pr(>F)    
treatment                     2 280.18  140.09 585.099  < 2e-16 ***
day                           1   0.44    0.44   1.821 0.177613    
as.factor(integument.color)   5   5.08    1.02   4.245 0.000816 ***
size                          1   0.68    0.68   2.850 0.091746 .  
weight                        1   1.25    1.25   5.213 0.022679 *  
treatment:day                 2  32.70   16.35  68.292  < 2e-16 ***
Residuals                   783 187.48    0.24                     
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
8 observations deleted due to missingness

A few things popped out from these results. One, a very significant treatment effect! Two, that there's a significant effect of treatment:day, but not of the day main effect. I ran the model from `step`, the model - treatment:day, and the model - treatment:day - day to see if there was a more parsimonious model:

```
TTRmodelSigMinusDayInt <- aov(log(TTRavg) ~ treatment + day + as.factor(integument.color) + size + weight, data = modTTR) #Run the most significant model identified by step - treatment:day
TTRmodelSigMinusDay <- aov(log(TTRavg) ~ treatment + day + as.factor(integument.color) + size + weight, data = modTTR) #Run the most significant model identified by step - treatment:day - day
TTRmodelSigMinusDayIntSize <- aov(log(TTRavg) ~ treatment + day + as.factor(integument.color) + weight, data = modTTR) #Run the most significant model identified by step - treatment:day - day - size
AIC(TTRmodelSig, TTRmodelSigMinusDayInt, TTRmodelSigMinusDay, TTRmodelSigMinusDayIntSize)
```

>                           df  AIC
TTRmodel	                  17	1140.073
TTRmodelSig	                14	1135.977		
TTRmodelSigMinusDayInt	    12	1259.964		
TTRmodelSigMinusDay	        12	1259.964
TTRmodelSigMinusDayIntSize	11	1261.588

Model fit improves when treatment:day is removed. Since AIC is the same for models without treatment:day but with or without day, I'll keep day in the final model. Interestingly, removing size (which was not significant) increases the AIC of the model. But also........I have no idea why the AIC from `AIC` and `step` are different.

```
summary(TTRmodelSigMinusDayInt) #Most parsimonious model. Significant influence of treatment, integment color, and weight, but no effect of day or size.
```

>                             Df Sum Sq Mean Sq F value  Pr(>F)    
treatment                     2 280.18  140.09 499.468 < 2e-16 ***
day                           1   0.44    0.44   1.554 0.21288    
as.factor(integument.color)   5   5.08    1.02   3.624 0.00301 **
size                          1   0.68    0.68   2.433 0.11919    
weight                        1   1.25    1.25   4.450 0.03521 *  
Residuals                   785 220.18    0.28                    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
8 observations deleted due to missingness

I saved the output from the final model [here](https://github.com/yaaminiv/wc-green-crab/blob/main/output/03-TTR-analysis/TTR-ANOVA-output.csv).

Once I had my final model, I checked my assumptions. The residuals are distributed normally, but they are borderline heteroskedastic (not distributed evenly across x-axis), probably because of their categorical nature. After checking assumptions, I used a Tukey HSD post-hoc test to look at pairwise difference between treatments. [The output](https://github.com/yaaminiv/wc-green-crab/blob/main/output/03-TTR-analysis/TTR-ANOVA-treatment-Tukey.csv) shows a significant difference between all treatments, with the 25ºC TTR being slightly faster than the control and the 5ºC TTR being slower than the other two treatments.

For my threshold analysis, I calculated the upper bound of outliers from an overall boxplot of the data (~6 seconds). I then used a binomial GLM and stepwise deletion approach to identify the most parsimonious model. The final model is as follows:

```
TTRthreshModel2 <- glm(formula = I(TTRthresh == 1) ~ treatment + day + as.factor(integument.color) +
    weight + treatment:day, family = binomial(), data = modTTR) #Run model selected by step with only significant terms
summary(TTRthreshModel2) #Only weight significant
```

> Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred
Call:
glm(formula = I(TTRthresh == 1) ~ treatment + day + as.factor(integument.color) +
    weight + treatment:day, family = binomial(), data = modTTR)
Coefficients:
                               Estimate Std. Error z value Pr(>|z|)  
(Intercept)                    16.38917  592.95355   0.028    0.978  
treatment25C                    0.63163    1.08375   0.583    0.560  
treatment5C                    -1.49470    0.91836  -1.628    0.104  
day                             1.01552    0.99085   1.025    0.305  
as.factor(integument.color)G  -14.31567  592.95283  -0.024    0.981  
as.factor(integument.color)O  -15.57255  592.95422  -0.026    0.979  
as.factor(integument.color)Y  -15.01206  592.95274  -0.025    0.980  
as.factor(integument.color)YG -13.63841  592.95280  -0.023    0.982  
as.factor(integument.color)YO -15.05720  592.95289  -0.025    0.980  
weight                          0.03523    0.01757   2.004    0.045 *
treatment25C:day               -0.93290    0.99239  -0.940    0.347  
treatment5C:day                -1.02331    0.99088  -1.033    0.302  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
(Dispersion parameter for binomial family taken to be 1)
    Null deviance: 536.60  on 795  degrees of freedom
Residual deviance: 371.35  on 784  degrees of freedom
  (8 observations deleted due to missingness)
AIC: 395.35
Number of Fisher Scoring iterations: 14

The only significant variable in explaining whether or not a crab righted within the threshold was weight. Weight was also a significant variable in the overall ANOVA. Generally, I believe the smaller crabs have faster TTR. I'll talk to Carolyn to see if I need to look at a TTR that's normalized by weight and then repeat the analysis.

<img width="1138" alt="Screenshot 2023-11-06 at 2 49 42 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/12dcbeb1-c17b-410f-b4df-6aa6baa68cc3">

**Figure 2**. Average TTR for each treatment and day

It's interesting that the 5ºC TTR was similar to the other crabs at days 1 and 2, but by day 7 TTR was much higher. Then, TTR for the 5ºC crabs slowly becomes faster over time, indicative of different short- and long-term acclimation responses. This data is also really consistent with [what I saw last year](https://yaaminiv.github.io/Green-Crab-Experiment-Part16/)!

Because I tracked the same individuals over the course of the experiment, I wanted to see if there were any individual-level responses to TTR. Specifically, there's a lot of variability in the 5ºC treatment. Can that variability be attributed to just one or two crabs, or does each crab "become the outlier" every once in a while? To do this, I subset TTR for my 27 respirometry crabs. While there were additional crabs I looked at throughout the experiment, I thought this subset would be small enough to manage for initial analyses. I then plot TTR for these crabs:

```
modTTR %>%
  filter(., respirometry == "Y") %>%
  dplyr::select(., c(crab.ID, day, tank, treatment, TTRavg)) %>%
  mutate(., treatment = case_when(treatment == "5C" ~ "05C",
                                  treatment == "15C" ~ "15C",
                                  treatment == "25C" ~ "25C")) %>%
  ggplot(., mapping = aes(x = day, y = TTRavg, group = crab.ID, color = treatment, shape = treatment)) +
  geom_point(size = 1, alpha = 0.75) + geom_line(linetype = 3) +
  facet_wrap(vars(treatment, tank), ncol = 3) +
  scale_x_continuous(name = "Day",
                     breaks = c(seq(0, 42, 14), 49),
                     limits = c(0, 49)) +
  scale_y_continuous(name = "Individual Time-to-Right (s)",
                     breaks = c(0, seq(1, 5, 2), seq(5, 15, 5))) +
  # scale_y_continuous(name = "Average Time-to-Right (s)") +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  theme_classic(base_size = 15) + theme(strip.text.x = element_blank(),
                                        strip.background = element_blank()) #Plot TTR for each individual crab that was tracked over the entire experiment. Facet by tank and treatment so it's easier to visualize. Assign colors to each treatment.
ggsave("figures/time-to-right-avg-individuals.pdf", height = 8.5, width = 11)
```

This wasn't the most useful exercise, so then I calculated the difference between the average TTR for that treatment x day and the individual crab TTR and plotted that:

```{r}
modTTR %>%
  filter(., respirometry == "Y") %>%
  dplyr::select(., c(crab.ID, day, tank, treatment, TTRavg, TTRavgFull)) %>%
  mutate(., treatment = case_when(treatment == "5C" ~ "05C",
                                  treatment == "15C" ~ "15C",
                                  treatment == "25C" ~ "25C")) %>%
  ggplot(., mapping = aes(x = day, y = TTRavg - TTRavgFull, group = crab.ID, color = treatment, shape = treatment)) +
  geom_point(size = 1, alpha = 0.75) + geom_line(linetype = 3) +
  facet_wrap(vars(treatment, tank), ncol = 3) +
  scale_x_continuous(name = "Day",
                     breaks = c(seq(0, 42, 14), 49),
                     limits = c(0, 49)) +
  scale_y_continuous(name = "Difference in Time-to-Right (s)",
                     breaks = seq(-5, 10, 5)) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  theme_classic(base_size = 15) + theme(strip.text.x = element_blank(),
                                        strip.background = element_blank()) #Plot difference in individual TTR from average TTR for each individual crab that was tracked over the entire experiment. Facet by tank and treatment so it's easier to visualize. Assign colors to each treatment.
ggsave("figures/time-to-right-avg-individuals-diff.pdf", height = 8.5, width = 11)
```

<img width="1138" alt="Screenshot 2023-11-06 at 3 10 26 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/e1b4518e-6c62-4276-9a98-c174107bf3af">

**Figure 3**. Difference between average and individual TTR

Okay, this I liked! It's easy for me to interpret for two reasons: 1) I can (for the most part) track individual crabs in each panel and see that for 5ºC, all the crabs were really variable and 2) there's BARELY any difference between individual crabs for the 15ºC and 25ºC treatments (those lines are happily hovering around 0). I spent an inordinate amount of time trying to assign different shapes for each crab within a panel, but gave up.

Carolyn suggested I plot percent change from average TTR for individual crabs so everything can be on the same scale:

```{r}
modTTR %>%
  filter(., respirometry == "Y") %>%
  dplyr::select(., c(crab.ID, day, tank, treatment, TTRavg, TTRavgFull)) %>%
  mutate(., treatment = case_when(treatment == "5C" ~ "05C",
                                  treatment == "15C" ~ "15C",
                                  treatment == "25C" ~ "25C")) %>%
  ggplot(., mapping = aes(x = day, y = ((TTRavg - TTRavgFull)/TTRavgFull)*100, group = crab.ID, color = treatment, shape = treatment)) +
  geom_point(size = 1, alpha = 0.75) + geom_line(linetype = 3) +
  facet_wrap(vars(treatment, tank), ncol = 3, scale = "free_y") +
  scale_x_continuous(name = "Day",
                     breaks = c(seq(0, 42, 14), 49),
                     limits = c(0, 49)) +
  scale_y_continuous(name = "Percent Change in Time-to-Right") +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  theme_classic(base_size = 15) + theme(strip.text.x = element_blank(),
                                        strip.background = element_blank()) #Plot difference in individual TTR from average TTR for each individual crab that was tracked over the entire experiment. Facet by tank and treatment so it's easier to visualize. Assign colors to each treatment.
ggsave("figures/time-to-right-avg-individuals-percChange.pdf", height = 8.5, width = 11)
```

While you can still see that for the 15ºC and 25ºC treatments the crabs move together whereas the crabs do their own thing in 5ºC, I thought that the percent change plot was really noisy.

### Respirometry analysis (the start)

Finally, the bane of my existence, respirometry analysis. To be fair, this really only sucks because cleaning the data sucks. I imported, cleaned, and analyzed [respirometry data](https://github.com/yaaminiv/wc-green-crab/tree/main/data/respiration) in [this R Markdown script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/04-respirometry-analysis.Rmd). Compared to last year's experiment, this year's data took me longer to clean because there were several instances of gaps in the data where a probe was disconnected or not fully connected. Most of the time, we realized a probe had a faulty connection in the beginning and were able to reset the probe and restart data collection. Sometimes, we were farther into data collection. In those instances, I cut the data until the probe was reattached. I contemplated just cutting out the gap in the data, but I have less confidence in data collected prior to a probe coming loose than after we've reattached it. Essentially, data was cleaned until around the first evident downward slope.

Also, I did a silly thing this summer and forgot to take salinity measurements my first two respirometry time points. To get salinity measurements for those time points so I could calculate dissolved oxygen, I averaged salinity across the experiment for each treatment and then used those average values:

```{r}
salinityValues <- data.frame(day = c(7, 14, 21, 28, 42, 49),
                             tank1 = c(NA, NA, 36, 40, 36, 39),
                             tank2 = c(NA, NA, 35, 35, 35, NA),
                             tank3 = c(NA, NA, 32, 34, 33, NA),
                             tank4 = c(NA, NA, 36, 39, 36, 39),
                             tank5 = c(NA, NA, 35, 34, 35, NA),
                             tank6 = c(NA, NA, 32, 36, 33, NA),
                             tank7 = c(NA, NA, 37, 38, 36, 39),
                             tank8 = c(NA, NA, 35, 34, 35, NA),
                             tank9 = c(NA, NA, 32, 34, 32, NA)) %>%
  column_to_rownames(., var = "day") %>%
  t(.) %>%
  as.data.frame() %>%
  mutate(., "7" = rowMeans(., na.rm = TRUE)) %>%
  mutate(., "14" = rowMeans(., na.rm = TRUE)) #Create new dataframe with all salinity values recorded based on lab notebook posts. Use NA when no salinity values were recorded. Convert day column to rownames, then transpose. Use rowMeans to calculate average salinity values to use for the first two days of measurements for each tank.
```

Around here I did a HARD PIVOT to the genotype data so I could incorporate it into my analyses!

### Going forward

1. Incorporate genotype into TTR and respirometry analyses
2. Prepare talk for PICES
3. Q10 analysis for 2023 experiment
4. Finish extracting respirometry samples
5. Continue with Chelex extractions, PCRs, and gels for TTR crabs
6. Update methods and results of 2022 paper
7. Examine HOBO data from 2023 experiment
8. Demographic data analysis for 2023 paper
9. Update methods and results of 2023 paper

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
