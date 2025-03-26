---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 17
tags: green-crab-cold TTR mixed-effects-models
---

## Analyzing TTR data for MA crabs

Let's analyze some data! After attending SEB, I got a good comment on my TTR analysis. My TTR data from 2022, 2023, and (now) 2024 all violate the standard assumption that observations are independent. This is because I'm conducting repeated measurements on crabs! I need to account for the non-independence of my data in my analysis. After searching the internet and talking to various people, I've identified different approaches:

1. [Linear mixed effects model with individual as a random effect](https://jontalle.web.engr.illinois.edu/MISC/lme4/bw_LME_tutorial.pdf)
2. [Repeated measures ANOVA](https://www.datanovia.com/en/lessons/repeated-measures-anova-in-r/)
3. Time series analysis but accounting for individual variation

Although, [this paper](https://pubmed.ncbi.nlm.nih.gov/14973047/) seems to suggest that a repeated measures ANOVA may not be appropriate for data. Since I'm more familiar with the theory around a mixed effects model (and that was my first inclination), I'll start there.

I created [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/03-TTR-analysis.Rmd) for my analysis. First, I formatted my data and made some exploratory plots (found [here](https://github.com/yaaminiv/cold-green-crab/tree/main/output/03-TTR-analysis/figures)):

```
modTTR <- rawTTR %>%
  dplyr::select(., -notes) %>%
  left_join(., crabMetadata, by = c("crab.ID", "tank", "carapace.width")) %>%
  filter(., is.na(trial.1) == FALSE) %>%
  mutate(., integument.cont = case_when(integument.color == "B/G" ~ 0.5,
                                        integument.color == "G" ~ 1.0,
                                        integument.color == "Y/G" ~ 1.5,
                                        integument.color == "Y" ~ 2.0,
                                        integument.color == "Y/O" ~ 2.5,
                                        integument.color == "O" ~ 3,
                                        integument.color == "R/O" ~ 3.5,
                                        integument.color == "R" ~ 4)) %>%
  mutate(., treatment = case_when(tank == "T01" ~ "0C",
                                  tank == "T02" ~ "0C",
                                  tank == "T03" ~ "0C",
                                  tank == "T05" ~ "5C",
                                  tank == "T06" ~ "5C",
                                  tank == "T07" ~ "5C")) %>%
  mutate(., missing.swimmer = case_when(missing.swimmer == "Y" ~ missing.swimmer,
                                        missing.swimmer == "N" ~ missing.swimmer,
                                        missing.swimmer == "Y " ~ "Y")) %>%
  mutate(., trial.1 = na_if(trial.1, 91.00)) %>%
  mutate(., trial.2 = na_if(trial.2, 91.00)) %>%
  mutate(., trial.3 = na_if(trial.3, 91.00)) %>%
  rowwise(.) %>%
  mutate(., TTRavg = mean(c(trial.1, trial.2, trial.3), na.rm = TRUE)) %>%
  # filter(., is.na(TTRavg) == FALSE) %>%
  mutate(., TTRSE = std.error(c(trial.1, trial.2, trial.3), na.rm = TRUE)) %>%
  # filter(., is.na(TTRSE) == FALSE) %>%
  ungroup(.) #Remove sampling ID and notes columns. Use case_when to create a column of continuous integument color metrics. Change all 91 to NA, calculate average and SE using data from the three TTR trials using rowwise operations. Ungroup. Join with genotype data.
```

### Average TTR anlaysis

I then constructed a mixed effects model with crab ID as an random effect:

```
TTRmodel <- lmer(log(TTRavg) ~ treatment + timepoint + treatment:timepoint +
                   as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                   (1|crab.ID),
                 data = (modTTR %>% filter(., is.na(TTRavg) == FALSE)), REML = FALSE) #Run model with treatment, timepoint, their interaction, sex, integument color, carapace width, weight, and missing swimmer information as fixed effects, and individual as a random effect. Change the likelihood estimator (REML = FALSE) to compare models with LRT later. Use data without rows with NAs
summary(TTRmodel) #Look at model output.
#Residuals explain more random effect variation than crab ID, crab ID explains very little variation
```

```
Linear mixed model fit by maximum likelihood  ['lmerMod']
Formula:
log(TTRavg) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) +  
    integument.cont + carapace.width + weight + as.factor(missing.swimmer) +  
    (1 | crab.ID)
   Data: (modTTR %>% filter(., is.na(TTRavg) == FALSE))

     AIC      BIC   logLik deviance df.resid
   457.5    494.7   -217.7    435.5      207

Scaled residuals:
    Min      1Q  Median      3Q     Max
-3.6693 -0.5443 -0.1364  0.4404  3.3219

Random effects:
 Groups   Name        Variance Std.Dev.
 crab.ID  (Intercept) 0.04731  0.2175  
 Residual             0.39013  0.6246  
Number of obs: 218, groups:  crab.ID, 82

Fixed effects:
                              Estimate Std. Error t value
(Intercept)                  3.586e-01  6.912e-01   0.519
treatment5C                 -4.648e-02  1.297e-01  -0.358
timepoint                    6.602e-01  4.583e-02  14.405
as.factor(sex)M              3.201e-03  1.104e-01   0.029
integument.cont              3.978e-02  7.697e-02   0.517
carapace.width              -2.067e-05  1.860e-02  -0.001
weight                       4.872e-03  1.044e-02   0.467
as.factor(missing.swimmer)Y  1.809e-01  1.624e-01   1.114
treatment5C:timepoint       -4.323e-01  5.749e-02  -7.520

Correlation of Fixed Effects:
            (Intr) trtm5C timpnt as.()M intgm. crpc.w weight
treatment5C -0.220                                          
timepoint   -0.146  0.390                                   
as.fctr(s)M -0.040 -0.014 -0.029                            
intgmnt.cnt -0.401  0.088  0.051  0.201                     
carapc.wdth -0.942  0.119  0.055 -0.062  0.133              
weight       0.824 -0.104 -0.007 -0.056 -0.087 -0.928       
as.fctr(.)Y  0.128 -0.117 -0.066 -0.009 -0.132 -0.121  0.108
trtmnt5C:tm  0.101 -0.597 -0.796  0.045 -0.076 -0.027  0.006
            a.(.)Y
treatment5C       
timepoint         
as.fctr(s)M       
intgmnt.cnt       
carapc.wdth       
weight            
as.fctr(.)Y       
trtmnt5C:tm  0.119
```

Interestingly, it seems like crab ID explains very little variation in TTR. My next step was to assess statistical significance. To do this, I needed to create null models without the factor I was interested in, and compare the models using LRT and AIC. I decided to keep demographic variables like sex, carapace width, and integument color in the null model to account for any variation they may contribute:

```
TTRnullTreatment <- lmer(log(TTRavg) ~ timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1|crab.ID), data = (modTTR %>% filter(., is.na(TTRavg) == FALSE)), REML = FALSE) #Run null model without treatment to identify significance of predictor
anova(TTRnullTreatment, TTRmodel) #Treatment is a significant predictor
```

```
Data: (modTTR %>% filter(., is.na(TTRavg) == FALSE))
Models:
TTRnullTreatment: log(TTRavg) ~ timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
TTRmodel: log(TTRavg) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
                 npar    AIC    BIC  logLik deviance  Chisq Df Pr(>Chisq)    
TTRnullTreatment    9 529.02 559.48 -255.51   511.02                         
TTRmodel           11 457.48 494.71 -217.74   435.48 75.536  2  < 2.2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```
```
TTRnullTimepoint <- lmer(log(TTRavg) ~ treatment + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1|crab.ID), data = (modTTR %>% filter(., is.na(TTRavg) == FALSE)), REML = FALSE) #Run null model without timepoint to identify significance of predictor
anova(TTRnullTreatment, TTRmodel) #Timepoint is a significant predictor
```

```
Data: (modTTR %>% filter(., is.na(TTRavg) == FALSE))
Models:
TTRnullTreatment: log(TTRavg) ~ timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
TTRmodel: log(TTRavg) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
                 npar    AIC    BIC  logLik deviance  Chisq Df Pr(>Chisq)    
TTRnullTreatment    9 529.02 559.48 -255.51   511.02                         
TTRmodel           11 457.48 494.71 -217.74   435.48 75.536  2  < 2.2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

```
TTRnullInteraction <- lmer(log(TTRavg) ~ treatment + timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1|crab.ID), data = (modTTR %>% filter(., is.na(TTRavg) == FALSE)), REML = FALSE) #Run model with interaction to identify significance of predictor. The full model constructed does not include an interaction
anova(TTRnullInteraction, TTRmodel) #Timepoint is a significant predictor
```

```
Data: (modTTR %>% filter(., is.na(TTRavg) == FALSE))
Models:
TTRnullInteraction: log(TTRavg) ~ treatment + timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
TTRmodel: log(TTRavg) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
                   npar    AIC    BIC  logLik deviance  Chisq Df Pr(>Chisq)    
TTRnullInteraction   10 505.31 539.16 -242.66   485.31                         
TTRmodel             11 457.48 494.71 -217.74   435.48 49.835  1  1.672e-12 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

Looks like all of my factors are significant! I am still a little puzzled on how to do the null model comparisons with the interaction term. When testing for just the effects of treatment and timepoint, I removed the interaction term because it those variables are included in the interaction. However, if I keep the interaction term and just remove the parent term in the null model, I get different results for significance.

For now, I created a table with my ANOVA output and saved the table [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/03-TTR-analysis/ttr-model-comparison-stat-output.csv).

```
TTRModelComparisons<- rbind(broom::tidy(anova(TTRnullTreatment, TTRmodel)),
                            broom::tidy(anova(TTRnullTimepoint, TTRmodel)),
                            broom::tidy(anova(TTRnullInteraction, TTRmodel))) #Create table for model comparison output
```

I topped off this analysis by making my classic TTR boxplot:

```
modTTR %>%
  dplyr::select(., c(timepoint, treatment, TTRavg)) %>%
  filter(., is.na(TTRavg) == FALSE) %>%
  distinct(.) %>%
  ggplot(mapping = aes(x = as.character(timepoint), y = TTRavg, color = treatment, shape = treatment)) +
  geom_boxplot(outlier.shape = NA) +
  geom_jitter(position = position_dodge(width = 0.75)) +
  ylab("Average Time-to-Right (s)") +
  scale_x_discrete(name = "Exposure Time (Hours)",
                   breaks = unique(modTTR$timepoint),
                   labels = c(0, 4, 22, 28, 46, 94)) +
  scale_color_manual(values = c(plotColors[1], plotColors[2]),
                     name = "Temperature (ºC)",
                     breaks = c("0C", "5C"),
                     labels = c("0", "5")) +
  scale_shape_manual(values = c(15, 19),
                     name = "Temperature (ºC)",
                     breaks = c("0C", "5C"),
                     labels = c("0", "5")) +
  theme_classic(base_size = 15) #Select unique timepoint, treatment, and TTRavg data, and remove rows where TTRavg = NA. Plot average TTR for each crab in a boxplot. Do not show outliers with the OG boxplot, but add them in with geom_jitter. Assign colors and shapes to each treatment. Increase base font size.
ggsave("figures/time-to-right-avg-boxplot.pdf", height = 8.5, width = 11)
```

<img width="1139" alt="Screenshot 2024-07-19 at 10 31 40 AM" src="https://github.com/user-attachments/assets/183ca755-2c1b-41c1-b296-696f0986afe8">

**Figure 1**. Average TTR at 0ºC and 5ºC for the duration of the experiment

It's interesting to see the trends in TTR acclimation! Around 24 hours of exposure, the crabs at 0ºC really slowed down, but they seem to recover by the end of the experiment. The same kind of thing happens with the 5ºC crabs: there's a much less pronounced slow down after the exposure starts, and they stablilize pretty quickly.

### Threshold analysis

I think a big reason why time point is significant in the model above is because of my uneven sample sizes! I had a lot of crabs in the 0ºC treatment that did not flip within the 90 second threshold. We recorded those times as 91 seconds for our trials, but converted that number to "NA" for the above analysis. I didn't want the 91s inflating averages in aw ay that would be difficult to interpret. Now, I want to look at the data for the number of crabs that didn't flip.

My first step was to define a threshold, where 0 = crab didn't flip in 90 seconds for any trial and 1 = crab flipped within 90 seconds for all trials:

```
modTTR <- modTTR %>%
  mutate(., TTRthresh = case_when(!(is.na(trial.1 & trial.2 & trial.3) == FALSE) ~ 0,
                                  is.na(trial.1 & trial.2 & trial.3) == FALSE ~ 1)) #Create a new column where 1 = righted within 90 seconds in all trials and 0 = failure to right within that threshold
```

Then, I constructed a binomial mixed effects model with crab ID as a random effect:

```
TTRthreshModel <- glmer(I(TTRthresh == 1) ~ treatment + timepoint + treatment:timepoint +
                          as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                          (1|crab.ID),
                        family = binomial(),
                        data = modTTR) #Create a binomial model, where 1 = success of righting wihtin the defined threshold
summary(TTRthreshModel) #Only timepoint is significant
```

```
Generalized linear mixed model fit by maximum likelihood (Laplace Approximation) [glmerMod]
 Family: binomial  ( logit )
Formula: I(TTRthresh == 1) ~ treatment + timepoint + treatment:timepoint +  
    as.factor(sex) + integument.cont + carapace.width + weight +  
    as.factor(missing.swimmer) + (1 | crab.ID)
   Data: modTTR

     AIC      BIC   logLik deviance df.resid
   140.1    176.0    -60.1    120.1      257

Scaled residuals:
    Min      1Q  Median      3Q     Max
-2.1759  0.0014  0.0027  0.1782  3.5572

Random effects:
 Groups  Name        Variance Std.Dev.
 crab.ID (Intercept) 1.845    1.358   
Number of obs: 267, groups:  crab.ID, 82

Fixed effects:
                            Estimate Std. Error z value Pr(>|z|)    
(Intercept)                  8.66534    7.04934   1.229    0.219    
treatment5C                  8.04878   30.80642   0.261    0.794    
timepoint                   -1.30306    0.27214  -4.788 1.68e-06 ***
as.factor(sex)M              0.65010    0.74632   0.871    0.384    
integument.cont             -0.10090    0.52152  -0.193    0.847    
carapace.width              -0.14569    0.19844  -0.734    0.463    
weight                       0.05439    0.11262   0.483    0.629    
as.factor(missing.swimmer)Y  0.47774    1.07250   0.445    0.656    
treatment5C:timepoint        1.57582   16.62811   0.095    0.924    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Correlation of Fixed Effects:
            (Intr) trtm5C timpnt as.()M intgm. crpc.w weight a.(.)Y
treatment5C  0.000                                                 
timepoint   -0.317  0.011                                          
as.fctr(s)M  0.032  0.000 -0.139                                   
intgmnt.cnt -0.305  0.001  0.195  0.094                            
carapc.wdth -0.972 -0.002  0.200 -0.068  0.128                     
weight       0.905  0.002 -0.146 -0.014 -0.122 -0.960              
as.fctr(.)Y  0.291 -0.001 -0.226  0.152 -0.215 -0.280  0.275       
trtmnt5C:tm  0.005 -0.646 -0.016  0.003 -0.004 -0.003  0.003  0.005
optimizer (Nelder_Mead) convergence code: 0 (OK)
```

Instead of taking the z-values from the table, I decided to conduct ANOVA for model comparison again:

```
TTRthreshNullTreatment <- glmer(I(TTRthresh == 1) ~ timepoint +
                          as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                          (1|crab.ID),
                        family = binomial(),
                        data = modTTR) #Create null model without treatment
anova(TTRthreshNullTreatment, TTRthreshModel) #Compare models
```

```
Data: modTTR
Models:
TTRthreshNullTreatment: I(TTRthresh == 1) ~ timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
TTRthreshModel: I(TTRthresh == 1) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
                       npar    AIC    BIC  logLik deviance  Chisq Df Pr(>Chisq)
TTRthreshNullTreatment    8 189.32 218.02 -86.662   173.32                     
TTRthreshModel           10 140.13 176.00 -60.064   120.13 53.196  2   2.81e-12

TTRthreshNullTreatment    
TTRthreshModel         ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

```
TTRthreshNullTimepoint <- glmer(I(TTRthresh == 1) ~ treatment +
                          as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                          (1|crab.ID),
                        family = binomial(),
                        data = modTTR) #Create null model without timepoint
anova(TTRthreshNullTimepoint, TTRthreshModel) #Compare models
```

```
Data: modTTR
Models:
TTRthreshNullTimepoint: I(TTRthresh == 1) ~ treatment + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
TTRthreshModel: I(TTRthresh == 1) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
                       npar    AIC    BIC  logLik deviance  Chisq Df Pr(>Chisq)
TTRthreshNullTimepoint    8 199.37 228.07 -91.685   183.37                     
TTRthreshModel           10 140.13 176.00 -60.064   120.13 63.242  2   1.85e-14

TTRthreshNullTimepoint    
TTRthreshModel         ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

```
TTRthreshNullInteraction <- glmer(I(TTRthresh == 1) ~ treatment + timepoint +
                          as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                          (1|crab.ID),
                        family = binomial(),
                        data = modTTR) #Create null model without interaction
anova(TTRthreshNullInteraction, TTRthreshModel) #Compare models
```

```
Data: modTTR
Models:
TTRthreshNullInteraction: I(TTRthresh == 1) ~ treatment + timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
TTRthreshModel: I(TTRthresh == 1) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + (1 | crab.ID)
                         npar    AIC    BIC  logLik deviance Chisq Df Pr(>Chisq)
TTRthreshNullInteraction    9 138.09 170.37 -60.043   120.09                    
TTRthreshModel             10 140.13 176.00 -60.064   120.13     0  1          1
```

I saved the model output [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/03-TTR-analysis/ttr-binomial-model-comparison-output.csv).

Interestingly, I'm getting model convergence issues when I include timepoint as a variable. I'm not really sure why! I need to dig into these issues a bit more when I return to this analysis to see if there's a way I can rescale the data.

In the meantime, I made a stacked barplot to visualize the number of crabs that did not flip in the first, second, and third trials at each timepoint:

```
treatment_labels <- c("0C" = "0ºC",
                      "5C" = "5ºC") #Modify facet labels for treatment
timepoint_labels <- c("0" = "0",
                      "1" = "4",
                      "2" = "22",
                      "3" = "28",
                      "4" = "46",
                      "5" = "94") #Modify facet labels for timepoint
```

```
modTTR %>%
  group_by(., treatment, timepoint) %>%
  summarize(., total = n(),
            trial1No = sum(is.na(trial.1) == TRUE),
            trial2No = sum(is.na(trial.2) == TRUE),
            trial3No = sum(is.na(trial.3) == TRUE)) %>%
  mutate(., trial1Yes = total - trial1No,
         trial2Yes = total - trial2No,
         trial3Yes = total - trial3No) %>%
  pivot_longer(., cols = c(trial1No, trial1Yes,
                           trial2No, trial2Yes,
                           trial3No, trial3Yes),
               values_to = "count", names_to = "condition") %>%
  mutate(., trial = case_when(condition == "trial1No" ~ 1,
                              condition == "trial1Yes" ~ 1,
                              condition == "trial2No" ~ 2,
                              condition == "trial2Yes" ~ 2,
                              condition == "trial3No" ~ 3,
                              condition == "trial3Yes" ~ 3)) %>%
  mutate(., condition = gsub(pattern = "trial1", replacement = "", x = condition),
         condition = gsub(pattern = "trial2", replacement = "", x = condition),
         condition = gsub(pattern = "trial3", replacement = "", x = condition)) %>%
  ggplot(., aes(x = trial, y = count, fill = condition)) +
  geom_bar(position = "fill", stat = "identity") +
  facet_grid(treatment~timepoint, labeller = labeller(treatment = treatment_labels,
                                                      timepoint = timepoint_labels)) +
  labs(x = "Trial", y = "Proportion") +
  scale_fill_manual(name = "",
                    values = c("grey70", "grey10")) +
  theme_classic(base_size = 15) + theme(strip.background = element_rect(colour = "white"))
ggsave("figures/time-to-right-noFlips.pdf", height = 8.5, width = 11)

#Take TTR data and group by timepoint. Count the number of crabs that did not right within 90 seconds during each trial. Subtract that value from the total to get the number of crabs that did right within 90 seconds. Pivot the data longer. Create a trial column based on the condition column. Remove trial information from the text in the condition column. Pipe modified data into ggplot to create a stacked barplot. Facet by treatment and timepoint using facet labels. Relabel axes. Modify fill scale. Use specific base size with classic theme. For facet labels, use a white border (no border).
```

<img width="1139" alt="Screenshot 2024-07-19 at 10 51 14 AM" src="https://github.com/user-attachments/assets/b8c774f6-f3ce-4b86-8b93-13dc19b20c86">

**Figure 2**. Proportion of crabs that did not flip in the first, second, or third trials at each time point.

### Going forward

1. Troubleshoot genotyping
2. Clarify methods for average TTR analysis
2. Individual-level TTR data analysis
3. Develop lipid assay protocol
3. Develop heart rate protocol
4. Conduct lipid assay with pilot crabs
5. Conduct lipid assay with remaining crabs

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
