---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 28
tags: green-crab-cold TTR mixed-effects-models
---

## Revising the MA TTR analysis

Now that I have all my genotypes for the MA data (well.......all samples have a genotype value and TBD if some of those are accurate or not), I want to add the genotype information to my TTR mixed effects models. All analyses were conducted in [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/MA/03-TTR-analysis-MA.Rmd).

### Adding genotype to the average TTR model

The first thing I needed to do was add genotype to the average TTR model! To do this, I used the existing model with temperature, time, and the interaction between the two as the null model, and added genotype or the precense of specific alleles as individual factors:

```
TTRfullGenotype <- lmer(log(TTRavg) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + as.factor(genotype) + (1|crab.ID), data = (modTTR %>% filter(., is.na(TTRavg) == FALSE)), REML = FALSE) #Run model with genotype to identify significance of predictor
anova(TTRmodel, TTRfullGenotype) #No significant impact of genotype
```

```
TTRpresenceC <- lmer(log(TTRavg) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + as.factor(presenceC) + (1|crab.ID), data = (modTTR %>% filter(., is.na(TTRavg) == FALSE)), REML = FALSE) #Run model with genotype to identify significance of predictor
anova(TTRmodel, TTRpresenceC) #No significant impact of C allele
```

```
TTRpresenceT <- lmer(log(TTRavg) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + as.factor(presenceT) + (1|crab.ID), data = (modTTR %>% filter(., is.na(TTRavg) == FALSE)), REML = FALSE) #Run model with genotype to identify significance of predictor
anova(TTRmodel, TTRpresenceT) #No significant impact of T allele
```

I also tested the impact of population source (New Bedford, MA or Eel Pond, MA) on righting response. This is because all of my Eel Pond crabs are CTs!

```
TTRsource <- lmer(log(TTRavg) ~ treatment + timepoint + treatment:timepoint + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + as.factor(source) + (1|crab.ID), data = (modTTR %>% filter(., is.na(TTRavg) == FALSE)), REML = FALSE) #Run model with genotype to identify significance of predictor
anova(TTRmodel, TTRsource) #No significant impact of source on TTR
```

As expected based on the 2023 data, there was no significant impact of genotype or the presence of a specific allele on average TTR. Thankfully, there wasn't a significant impact of source population either. All of my statistical output can be found [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/MA/03-TTR-analysis/ttr-model-comparison-stat-output.csv).

### Genotype in the threshold analysis

The next step was identifying if genotype played a role in crabs not being able to right at the colder temperature. I had initially created a binomial mixed-effects model to understand the impact of temperature, time, and their interaction on binary righting response. Temperature and time were significant, but the interaction was not. Using a similar approach as above, I added genotype or the presence of specific alleles as covariates and tested their significance:

```
TTRthreshGenotype <- glmer(I(TTRthresh == 1) ~ treatment + timepoint +
                             as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                             as.factor(genotype) +
                             (1|crab.ID),
                           family = binomial(),
                           data = modTTR) #Create null model without interaction
anova(TTRthreshModelRevised, TTRthreshGenotype) #Compare models
```

```
TTRthreshpresenceC <- glmer(I(TTRthresh == 1) ~ treatment + timepoint +
                              as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                              as.factor(presenceC) +
                              (1|crab.ID),
                            family = binomial(),
                            data = modTTR) #Create null model without interaction
anova(TTRthreshModelRevised, TTRthreshpresenceC) #Compare models
```

```
TTRthreshpresenceT <- glmer(I(TTRthresh == 1) ~ treatment + timepoint +
                              as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                              as.factor(presenceT) +
                              (1|crab.ID),
                            family = binomial(),
                            data = modTTR) #Create null model without interaction
anova(TTRthreshModelRevised, TTRthreshpresenceT) #Compare models
```

```
TTRthreshSource <- glmer(I(TTRthresh == 1) ~ treatment + timepoint +
                           as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                           as.factor(source) +
                           (1|crab.ID),
                         family = binomial(),
                         data = modTTR) #Create null model without interaction
anova(TTRthreshModelRevised, TTRthreshSource) #Compare models
```

There were significant effects of both the presence of the T allele and source on binary righting response! The statistical output can be found [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/MA/03-TTR-analysis/ttr-binomial-model-comparison-output.csv). I then set about plotting to see what was going on. I re-familiarized myself with my initial plot of binary righting response over time for data for each individual trial:

<img width="1360" alt="Screenshot 2024-12-19 at 2 35 53 PM" src="https://github.com/user-attachments/assets/00f8315c-4ec6-4ce3-8b33-d3edd6e2494e" />

**Figure 1**. Binary righting response plot

I then created a bunch of plots exploring the various sub-effects.

<img width="1360" alt="Screenshot 2024-12-19 at 2 36 07 PM" src="https://github.com/user-attachments/assets/fce6d14d-b13d-4984-a494-3067fe6d690b" />

<img width="1360" alt="Screenshot 2024-12-19 at 2 36 53 PM" src="https://github.com/user-attachments/assets/9ae8c31e-e1d8-4079-9e27-15f13a140751" />

**Figures 2-3**. Binary righting response plots examining the impact of source population

It seems like source population is likely significant because again, all my Eel Pond crabs are CTs. It seems like the proportion of CC and CT crabs from New Bedford are varying more than the Eel Pond ones, but that could also be an effect of our random sampling at each time point.

<img width="1360" alt="Screenshot 2024-12-19 at 2 36 32 PM" src="https://github.com/user-attachments/assets/7cfc2310-acd5-495b-a697-eb8d86b872b8" />

<img width="1360" alt="Screenshot 2024-12-19 at 2 37 12 PM" src="https://github.com/user-attachments/assets/ef0a6174-976d-4049-a530-4ce67715c46a" />

**Figure 4-5**. Binary righting response plots examining the impact of genotype only

When I look just at the impact of genotype (CC vs. CT or TT), it does seem like there is a slight increase in the proportion of crabs not flipping in subsequent trials and at subsequent time points. Perhaps the sample size was too low for there to be a significant impact of temperature and time. I considered examining interactions between genotype and time or temperature, but I think that would be too confusing to interpret.

<img width="1360" alt="Screenshot 2024-12-19 at 2 37 02 PM" src="https://github.com/user-attachments/assets/2d7b329c-9e8b-495c-90f1-5666d7b3787f" />

**Figure 6**. Genotype breakdown of crabs that did not right

Finally, I subset the crabs that did not right and broke it down by genotype. Again, there seems to be something happening with genotype ratios at time goes on, but that could be due to our random sampling.

### Individual-level analyses

The next thing I wanted to was understand individual-level patterns over time. I had 21 individuals I followed throughout the course of the experiment. I first calculated Q10 values at each time point for these crabs, then plotted these values:

```
modTTRQ10 <- modTTR %>%
  select(., timepoint, crab.ID, TTRavg, treatment, presenceT) %>%
  group_by(., crab.ID) %>%
  pivot_wider(., names_from = timepoint, values_from = TTRavg, names_prefix = "timepoint") %>%
  mutate_all(~ifelse(is.nan(.), 91, .)) %>%
  filter(., is.na(timepoint0) == FALSE) %>%
  filter(., is.na(timepoint1) == FALSE) %>%
  filter(., is.na(timepoint2) == FALSE) %>%
  filter(., is.na(timepoint3) == FALSE) %>%
  filter(., is.na(timepoint4) == FALSE) %>%
  filter(., is.na(timepoint5) == FALSE) %>%
  mutate(temperature = case_when(treatment == "0C" ~ 1.5,
                                 treatment == "5C" ~ 5)) %>%
  arrange(., temperature) %>%
  mutate(., timepoint1_Q10 = Q10(R2 = timepoint0,
                                 R1 = timepoint1,
                                 T2 = 15,
                                 T1 = temperature)) %>%
  mutate(., timepoint2_Q10 = Q10(R2 = timepoint0,
                                 R1 = timepoint2,
                                 T2 = 15,
                                 T1 = temperature)) %>%
  mutate(., timepoint3_Q10 = Q10(R2 = timepoint0,
                                 R1 = timepoint3,
                                 T2 = 15,
                                 T1 = temperature)) %>%
  mutate(., timepoint4_Q10 = Q10(R2 = timepoint0,
                                 R1 = timepoint4,
                                 T2 = 15,
                                 T1 = temperature)) %>%
  mutate(., timepoint5_Q10 = Q10(R2 = timepoint0,
                                 R1 = timepoint5,
                                 T2 = 15,
                                 T1 = temperature)) %>%
  select(., crab.ID, treatment, presenceT,
         timepoint1_Q10, timepoint2_Q10, timepoint3_Q10, timepoint4_Q10, timepoint5_Q10) %>%
  pivot_longer(., cols = !c(crab.ID, treatment, presenceT), names_to = "timepoint", values_to = "Q10") %>%
  mutate(., timepoint = gsub(x = timepoint, pattern = "_Q10", replacement = "")) %>%
  mutate(., timepoint = gsub(x = timepoint, pattern = "timepoint", replacement = "")) %>%
  mutate(., timepoint = as.numeric(timepoint))
#Select columns of interest. Add columns for presence of C or T alleles. Pivot wider. Change NaN to 91 and remove any NA measurements. Add a column for temperature based on the treatment column, and arrange data by temperature. Use the Q10 function within the respirometry package to calculate Q10 values for timepoint 0 vs. all other timepoints. Select columns of interest and pivot longer. Use a series of gsub to modify timepoint column and convert to a character format that would be organized numerically. Use a similar approach for treatment.
```

<img width="1360" alt="Screenshot 2024-12-19 at 2 47 42 PM" src="https://github.com/user-attachments/assets/689b5ebb-129d-4a6a-9632-947ec22084cb" />

<img width="1360" alt="Screenshot 2024-12-19 at 2 47 53 PM" src="https://github.com/user-attachments/assets/68868aeb-be36-49fa-9c47-f68c7fe507bc" />

**Figures 7-8**. Q10 values for crabs

It's interesting to see such low Q10 values for this analysis! Most biological processes have a Q10 of 2, with a Q10 of 1 indiciating increased temperature dependence. I need to look into was values < 1 mean. It's interesting to see how the colder temperature leads to an "acclimation response" for Q10, where most crabs have similar patterns compared to the crabs at 5ºC. Any variation in those patterns seems to be related to genotype.

I also wanted to see if there was evidence of recovery: crabs that did not right at one time point, but acclimated over time and were able to right at a later time point. I was also interested in seeing patterns of "failed" recovery. To do this, I plotted the binary values for whether or not a crab righted over time, and then categorized those patterns:

```
left_join(modTTRQ10, modTTR, by = c("crab.ID", "treatment", "presenceT", "timepoint")) %>%
  select(., -c(date:trial.3, sex:TTRSE, genotype:presenceC)) %>%
  filter(., treatment != "5C") %>%
  ggplot(., mapping = aes(x = timepoint, y = TTRthresh, color = presenceT, group = crab.ID)) +
  geom_point(size = 2) + geom_line(lty = 3) +
  facet_wrap(~crab.ID) +
  scale_x_continuous(name = "Exposure Time (Hours)",
                   breaks = 1:5,
                   labels = c(4, 22, 28, 46, 94)) +
  scale_y_continuous(name = "Right?",
                     breaks = c(0, 1),
                     labels = c("No", "Yes")) +
  scale_colour_manual(name = "Genotype",
                    values = c("grey70", "grey10"),
                    labels = c("CC", "CT")) +
  theme_classic(base_size = 15) + theme(strip.background = element_rect(colour = "white")) #Join TTR data with Q10 data to get individuals that were tracked throughout time. Remove superfluous columns and entries at 5ºC. Create point and line graph and wrap by crab ID.
```

```
left_join(modTTRQ10, modTTR, by = c("crab.ID", "treatment", "presenceT", "timepoint")) %>%
  select(., -c(Q10:trial.3, sex:TTRSE, genotype:presenceC)) %>%
  filter(., treatment != "5C") %>%
  pivot_wider(., names_from = "timepoint", names_prefix = "timepoint", values_from = "TTRthresh") %>%
  mutate(., pattern = case_when(timepoint2 == 0 & timepoint3 == 0 & timepoint4 == 0 & timepoint5 == 0 ~ "4",
                                timepoint2 == 1 & timepoint3 == 1 & timepoint4 == 1 & timepoint5 == 1 ~ "1",
                                (timepoint2 == 0 | timepoint3 == 0) & timepoint5 == 1 ~ "2",
                                (timepoint3 == 1 | timepoint4 == 1) & timepoint5 == 0 ~ "3")) %>%
  group_by(., presenceT, pattern) %>%
  summarize(., count = n()) %>%
  ggplot(., mapping = aes(x = presenceT, y = count, fill = pattern)) +
  geom_bar(position = "fill", stat = "identity") +
  labs(y = "Proportion") +
  scale_x_discrete(name = "Genotype",
                 labels = c("CC", "CT")) +
  scale_fill_manual(name = "Pattern",
                    labels = c("Always", "Recovered", "Failed", "Never"),
                    values = c("grey90", "grey60", "grey30", "grey0")) +
  theme_classic(base_size = 15) #Take data and pivot timepoint and threshold information. Assign patterns where 1 = always righting, 2 = recovered, 3 = failure to recover, and 4 = never righting. Group data by genotype and pattern and generate counts. Create stacked barplot.
ggsave("figures/time-to-right-average-individuals-Q10-genotype-patterns.pdf", height = 8.5, width = 11)
```

<img width="1360" alt="Screenshot 2024-12-19 at 2 54 12 PM" src="https://github.com/user-attachments/assets/100c8d1c-caba-48d7-9fe3-8f69e62a3e0e" />

<img width="1360" alt="Screenshot 2024-12-19 at 2 54 25 PM" src="https://github.com/user-attachments/assets/d8cdb6e3-d120-4746-93c4-56b73d89d357" />

**Figures 9-10**. Patterns of righting response for crabs tracked over the course of the experiment.

I then did this again for a larger sample size of crabs. Instead of crabs that were tracked throughout the experiment, I picked crabs with at least three time points of data thanks to a handy `filter(n() > 2)` argument:

```
modTTR %>%
  dplyr::select(., crab.ID, treatment, presenceT, timepoint, source, TTRthresh) %>%
  filter(., timepoint != 0) %>%
  filter(., treatment == "0C") %>%
  group_by(., crab.ID) %>%
  arrange(., crab.ID) %>%
  filter(n() > 2) %>%
  ggplot(., mapping = aes(x = timepoint, y = TTRthresh, color = presenceT, shape = source, group = crab.ID)) +
  geom_point(size = 2) + geom_line(lty = 3) +
  facet_wrap(~crab.ID) +
  scale_x_continuous(name = "Exposure Time (Hours)",
                   breaks = 1:5,
                   labels = c(4, 22, 28, 46, 94)) +
  scale_y_continuous(name = "Right?",
                     breaks = c(0, 1),
                     labels = c("No", "Yes")) +
  scale_colour_manual(name = "Genotype",
                    values = c("grey70", "grey10"),
                    labels = c("CC", "CT")) +
  scale_shape_manual(name = "Population",
                     values = c(15, 19),
                     labels = c("Eel Pond, MA", "New Bedford, MA")) +
  theme_classic(base_size = 15) + theme(strip.background = element_rect(colour = "white"))
ggsave("figures/time-to-right-noFlips-individuals.pdf", height = 8.5, width = 11)
```

```
modTTR %>%
  dplyr::select(., crab.ID, treatment, presenceT, timepoint, source, TTRthresh) %>%
  filter(., timepoint != 0) %>%
  filter(., treatment == "0C") %>%
  group_by(., crab.ID) %>%
  arrange(., crab.ID) %>%
  filter(n() > 2) %>%
  pivot_wider(., names_from = "timepoint", names_prefix = "timepoint", values_from = "TTRthresh") %>%
  mutate(., pattern = case_when(timepoint2 == 0 & timepoint3 == 0 & (timepoint4 == 0 | timepoint5 == 0) ~ "4",
                                timepoint2 == 1 & timepoint3 == 1 & timepoint4 == 1 & timepoint5 == 1 ~ "1",
                                (timepoint2 == 0 | timepoint3 == 0) & timepoint5 == 1 ~ "2",
                                (timepoint3 == 1 | timepoint4 == 1) & timepoint5 == 0 ~ "3")) %>%
  filter(., is.na(pattern) == FALSE) %>%
  group_by(., presenceT, pattern) %>%
  summarize(., count = n()) %>%
  ggplot(., mapping = aes(x = presenceT, y = count, fill = pattern)) +
  geom_bar(position = "fill", stat = "identity") +
  labs(y = "Proportion") +
  scale_x_discrete(name = "Genotype",
                 labels = c("CC", "CT")) +
  scale_fill_manual(name = "Pattern",
                    labels = c("Always", "Recovered", "Failed", "Never"),
                    values = c("grey90", "grey60", "grey30", "grey0")) +
  theme_classic(base_size = 15) #Take data and pivot timepoint and threshold information. Assign patterns where 1 = always righting, 2 = recovered, 3 = failure to recover, and 4 = never righting. Group data by genotype and pattern and generate counts. Create stacked barplot.
ggsave("figures/time-to-right-average-individuals-genotype-patterns.pdf", height = 8.5, width = 11)
```

<img width="1360" alt="Screenshot 2024-12-19 at 2 54 40 PM" src="https://github.com/user-attachments/assets/26cab09c-5b29-4966-85ea-a2778ef50ee4" />

<img width="1360" alt="Screenshot 2024-12-19 at 2 54 53 PM" src="https://github.com/user-attachments/assets/70207060-e4f2-433d-a231-4ae09d2165d2" />

**Figures 11-12**. Patterns of righting response for crabs with at least three timepoints of data.

Looking at both sets of plots, it's interesting to see differences in recovery vs. failed recovery by genotype. These sample sizes are quite small, so I wouldn't put too much stock in these differences beyond the fact that there are differences by genotype.

### Going forward

1. Individual-level TTR data analysis for WA samples
2. Genotype WA samples
1. Troubleshoot QC gel discrepancies by rerunning MA CC samples
2. Redo all Chelex samples?
2. Identify samples for confirmation sequencing
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
