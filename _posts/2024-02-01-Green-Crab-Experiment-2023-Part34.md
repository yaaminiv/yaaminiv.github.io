---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 34
tags: green-crab-wc TTR respirometry genotype
---

## Individual-level analyses

Carolyn and I have been talking a lot about the different individual-level analyses we wish we could have done with the 2022 paper (because naturally, you can't do an individual-level analysis without individual-level data). While all of these suggestions were fresh in my mind, I figured I'd give them a go with the 2023 data!

### TTR

The first thing I did was open [this R Markdown notebook](https://github.com/yaaminiv/wc-green-crab/blob/main/code/03-TTR-analysis.Rmd) and modify my TTR analyses to 1) use carapace width instead of size, 2) recode integument color as a continuous variable and 3) use a GLM for the analysis. Basically, all of the things I did with the 2022 data based on Carolyn's suggestions. The output from the TTR GLM can be found [here](https://github.com/yaaminiv/wc-green-crab/blob/main/output/03-TTR-analysis/TTR-glm-output.csv). Interestingly, integument color and weight were important factors for the 2023 data. I wonder how much integument color is driven by faster intermolt timing in the warmer treatment. I also modified the figure to use a boxplot for TTR instead of points with SE bars.

![Screenshot 2024-01-31 at 3 14 03 PM](https://github.com/yaaminiv/wc-green-crab/assets/22335838/4fb9ff5f-b885-45dd-ba64-6ba749aafba3)

**Figure 1**. Revised TTR boxplot for 2023 data.

The next thing I wanted to do with the TTR data was do a individual-level Q10 analysis. In the experiment, I had data from day 0 before crabs were exposed to any treatment temperatures. I then had data for subsequent days when crabs were in treatment conditions. I decided to calculate Q10 metrics for days 2, 7, 21, and 42. Days 2 and 7 could represent shorter-term changes, especially because crabs in 5ºC conditions could maintain TTR at day 2 but slowed down considerably at day 7. Day 21 was a nice parallel between this experiment and the 2022 experiment, which lasted 22 days. I chose day 42 as the final time point because all crabs were monitored for 42 days, as opposed to the cold crabs that were monitored for 49 days. I could theoreticaly add Q10 value for day 49 since I have that data, but I kept it simpler for now.

To actually calculate Q10 values, I used the `Q10` function in the `respirometry` package after some dataframe modifications:

```
modTTRQ10 <- modTTR %>%
  select(., day, crab.ID, TTRavg, treatment, final.genotype) %>%
  mutate(., presenceC = case_when(final.genotype == "CC" ~ "Y",
                                  final.genotype == "CT" ~ "Y",
                                  final.genotype == "TT" ~ "N")) %>%
  mutate(., presenceT = case_when(final.genotype == "CC" ~ "N",
                                  final.genotype == "CT" ~ "Y",
                                  final.genotype == "TT" ~ "Y")) %>%
  filter(., day == 0 | day == 2 | day == 7 | day == 21 | day == 42) %>%
  filter(., treatment != "15C") %>%
  pivot_wider(., names_from = day, values_from = TTRavg, names_prefix = "day") %>%
  filter(., is.na(day0) == FALSE) %>%
  mutate(temperature = case_when(treatment == "15C" ~ 15,
                                 treatment == "25C" ~ 25,
                                 treatment == "5C" ~ 5)) %>%
  arrange(., temperature) %>%
  mutate(., day2_Q10 = Q10(R1 = day0,
                           R2 = day2,
                           T1 = 15,
                           T2 = temperature)) %>%
  mutate(., day7_Q10 = Q10(R1 = day0,
                           R2 = day7,
                           T1 = 15,
                           T2 = temperature)) %>%
  mutate(., day21_Q10 = Q10(R1 = day0,
                           R2 = day21,
                           T1 = 15,
                           T2 = temperature)) %>%
  mutate(., day42_Q10 = Q10(R1 = day0,
                           R2 = day42,
                           T1 = 15,
                           T2 = temperature)) %>%
  select(., crab.ID, treatment, final.genotype, presenceC, presenceT, day2_Q10, day7_Q10, day21_Q10, day42_Q10) %>%
  pivot_longer(., cols = !c(crab.ID, treatment, final.genotype, presenceC, presenceT), names_to = "day", values_to = "Q10") %>%
  filter(., is.na(Q10) == FALSE) %>%
  mutate(., day = gsub(x = day, pattern = "_Q10", replacement = "")) %>%
  mutate(., day = gsub(x = day, pattern = "day", replacement = "")) %>%
  mutate(., day = case_when(day == "2" ~ "02",
                            day == "7" ~ "07",
                            day != "2" | day != "7" ~ day)) %>%
  mutate(., treatment = case_when(treatment == "5C" ~ "05C",
                                  treatment != "5C" ~ treatment))
#Select columns of interest. Add columns for presence of C or T alleles. Filter days of interest, remove 15C crabs, and pivot wider. Remove crabs without a day 0 measurement. Add a column for temperature based on the treatment column, and arrange data by temperature. Use the Q10 function within the respirometry package to calculate Q10 values for day 0 vs. day 2, 7, 21, and 42. Select columns of interest and pivot longer. Remove any rows without a Q10 value. Use a series of gsub to modify day column and convert to a character format that would be organized numerically. Use a similar approach for treatment.
```

I then plotted the Q10 data.

```
modTTRQ10 %>%
  ggplot(., mapping = aes(x = day, y = Q10, color = treatment, shape = treatment, group = crab.ID)) +
  geom_point(size = 2) + geom_line(lty = 3) +
  facet_wrap(. ~ treatment, scales = "free_y", labeller = labeller(treatment = facet_labels)) +
  scale_x_discrete(name = "Day",
                   breaks = c("02", "07", "21", "42"),
                   labels = c("2", "7", "21", "42")) +
  scale_color_manual(values = c(plotColors[3], plotColors[1]),
                     guide = "none") +
  scale_shape_manual(values = c(15, 17),
                     guide = "none") +
  theme_classic(base_size = 15) #Plot Q10 data with day on the x-axis, Q10 on the y-axis, and color and shape defined by treatment. Group by crab.ID. Add points and lines connecting values for each crab. Facet by treatment, allow y-axis to vary by each facet, and include facet labels. Add x-axis label and specify x-axis tick labels. Add shape and color values for each treatment but do not include a legend.
```

<img width="1150" alt="Screenshot 2024-02-01 at 8 52 40 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/7d4f8728-e503-4418-a5d5-25ef39f36023">

**Figure 2**. Q10 for TTR

Generally, Q10 is around 2 for biological processes. Q10 values greater than 1 signify temperature dependence, values around 1 signifiy temperature independence, and values less than 1 signify declines in performance. Performance will continue declining until a critical threshold is met. Interestingly, Q10 is higher for crabs at 5ºC than at 25ºC. Q10 also drops considerably between day 2 and 7 for the 5ºC crabs, which could be related to the TTR differences at those timepoints. I also noticed that Q10 was a lot lower for crabs at 25ºC, and most values were under 1. This could mean that crab performance is declining at 25ºC, even though mortality isn't significantly higher. When I sequence more crabs, it'll be interesting to see how genotype adds to this story.

I wanted to understand how genotype influenced TTR data, but I had a limited number of crabs to work with. I pulled from the approach Julia used to look at her crabs: differences in TTR between two timepoints. Unfortunately, not every crab as a day 0 metric (that's something to consider for subsequent experiments), so I couldn't use day 0. However, a lot of crabs had day 2 metrics, so I used that as a base to compare against days 7, 21, and 42. I calculated differences in TTR then plotted them using a multipanel boxplot facetted by genotype.

```
modTTRSubsetDiff <- modTTRSubset %>%
  select(., day, crab.ID, TTRavg, treatment, final.genotype, presenceC, presenceT) %>%
  filter(., day == 0 | day == 2 | day == 7 | day == 21 | day == 42) %>%
  pivot_wider(., names_from = day, values_from = TTRavg, names_prefix = "day") %>%
  mutate(., day2v0 = day2 - day0) %>%
  mutate(., day7v0 = day7 - day0) %>%
  mutate(., day21v0 = day21 - day0) %>%
  mutate(., day42v0 = day42 - day0) %>%
  mutate(., day7v2 = day7 - day2) %>%
  mutate(., day21v2 = day21 - day2) %>%
  mutate(., day42v2 = day42 - day2) #For subset of the crabs, select day, crab ID, average TTR, treatmet, and genotype information. Filter days of interest and pivot wider, using days as columns and values from TTRavg. Add "day" as a prefix to all columns. Get the difference in TTR for comparisons of interest.
```

```
day7vs2Plot <- modTTRSubsetDiff %>%
  mutate(., treatment = case_when(treatment == "5C" ~ "05C",
                                  treatment != "5C" ~ treatment)) %>%
  ggplot(mapping = aes(x = treatment, y = day7v2, color = treatment)) +
  geom_boxplot(outlier.shape = NA) + geom_point(aes(shape = treatment), size = 2) +
  facet_wrap(. ~ final.genotype) +
  labs(x = "", y = "", title = "A. Day 7 vs. Day 2") +
  scale_y_continuous(breaks = seq(-5, 15, 5),
                     limits = c(-5,15)) +
  scale_x_discrete(breaks = c("05C", "15C", "25C"),
                   labels = c("5", "15", "25")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  theme_classic(base_size = 15) + theme(legend.position = "none",
                                        axis.ticks.x = element_blank(),
                                        axis.text.x = element_blank())
```

```
day21vs2Plot <- modTTRSubsetDiff %>%
  mutate(., treatment = case_when(treatment == "5C" ~ "05C",
                                  treatment != "5C" ~ treatment)) %>%
  ggplot(mapping = aes(x = treatment, y = day21v2, color = treatment)) +
  geom_boxplot(outlier.shape = NA) + geom_point(aes(shape = treatment), size = 2) +
  facet_wrap(. ~ final.genotype) +
  labs(x = "Temperature (ºC)", y = "Difference in Individual TTR (s)", title = "B. Day 21 vs. Day 2") +
  scale_y_continuous(breaks = seq(-1,5,1),
                     limits = c(-1,5)) +
  scale_x_discrete(breaks = c("05C", "15C", "25C"),
                   labels = c("5", "15", "25")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  theme_classic(base_size = 15) + theme(legend.position = "none",
                                        axis.ticks.x = element_blank(),
                                        axis.text.x = element_blank())
```

```
day42v2Plot <- modTTRSubsetDiff %>%
  mutate(., treatment = case_when(treatment == "5C" ~ "05C",
                                  treatment != "5C" ~ treatment)) %>%
  ggplot(mapping = aes(x = treatment, y = day42v2, color = treatment)) +
  geom_boxplot(outlier.shape = NA) + geom_point(aes(shape = treatment), size = 2) +
  facet_wrap(. ~ final.genotype) +
  labs(x = "Temperature (ºC)", y = "", title = "C. Day 42 vs. Day 2") +
  scale_y_continuous(breaks = seq(-1,6,1),
                     limits = c(-1,6)) +
  scale_x_discrete(breaks = c("05C", "15C", "25C"),
                   labels = c("5", "15", "25")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "15C", "25C"),
                     labels = c("5", "15", "25")) +
  theme_classic(base_size = 15) + theme(legend.position = "none")
```

```
day7vs2Plot / day21vs2Plot / day42v2Plot
ggsave("figures/time-to-right-average-individuals-genotype-multipanel.pdf", height = 11, width = 8.5)
```

<img width="686" alt="Screenshot 2024-02-01 at 11 09 58 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/fd4d1cfc-e9e5-4e78-907f-d85e8cd96133">

**Figure 3**. Differences in TTR between day 2 and either day 7, 21, and 42 by genotype.

The first thing that jumps out to me is that I haven't sequenced any CC crabs at 5ºC! I know that I don't have any CC respirometry crabs at this temperature, but hopefully future sequencing will show me that I do have some CC in that treatment. Secondly, I see that TTR differences aren't large for crabs at 15ºC or 25ºC. This makes sense because TTR at these temperatures didn't really vary over time. However, there is large variability at 5ºC, with CT crabs having more variability. But when you compare the differences at day 7 vs. 21 vs. 42, it seems like the TTR difference is shrinking for CT and TT crabs at 5ºC, with more consistent responses for TT crabs. TT is the putative cold-adapted allele, so it's interesting to see that being a heterozygote still may confer some level of advantage.

### Respirometry

Moving on to respirometry in [this R Markdown notebook](https://github.com/yaaminiv/wc-green-crab/blob/main/code/04-respirometry-analysis.Rmd)! First things first, I used carapace weight and continuous integument color to look at significant differences in the data. I also decided to use a GLM to analyze differences in respirometry data. This is because I wanted to test a three-fold interaction between treatment, day, and genotype, and didn't think an ANOVA was appropriate for doing that.

```
lmAllSlopeResults <- glm(log(-slope_nmol_hr_corrected) ~ treatment*as.numeric(day)*final.genotype +
                           sex + weight + carapace.width + integument.cont + missing.legs,
                         data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Examine the influence of treatment, day, genotype, and various demographic factors on respiration rate. Remove outliers based on blank ratios prior to analysis.
lmAllSlopeResultsStep <- step(lmAllSlopeResults, direction = "backward") #Use backwards-deletion approach to identify the most significant model (lowest AIC)
summary(lmAllSlopeResultsStep) #The most significant model is one that includes treatment and day, but not the interaction between the two. None of the other demographic terms are significant
```

```
lmAllSlopeResultsSig <- glm(log(-slope_nmol_hr_corrected) ~ treatment + as.numeric(day) + final.genotype +
                              treatment:as.numeric(day) + treatment:final.genotype + as.numeric(day):final.genotype +
                              treatment:as.numeric(day):final.genotype,
                            data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Run the most significant model identified by step
```

Next, I looked at genotype a different way. Instead of CC, CT, and TT, what if I coded for presence of a C or T allele?

```
lmBinarySlopeResults <- glm(log(-slope_nmol_hr_corrected) ~ treatment*as.numeric(day)*hasC + treatment*as.numeric(day)*hasT +
                              sex + weight + carapace.width + integument.cont + missing.legs,
                            data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Examine the influence of treatment, day, and various demographic factors on respiration rate. Remove outliers based on blank ratios prior to analysis.
lmBinarySlopeResultsStep <- step(lmBinarySlopeResults, direction = "backward") #Use backwards-deletion approach to identify the most significant model (lowest AIC)
summary(lmBinarySlopeResultsStep) #The most significant model is one that includes treatment, day, the interaction, and hasT
```

```
lmBinarySlopeResultsSig <- glm(log(-slope_nmol_hr_corrected) ~ treatment + as.numeric(day) + hasC + hasT +
                                 treatment:as.numeric(day) + treatment:hasC +
                                 as.numeric(day):hasC + treatment:hasT +
                                 treatment:as.numeric(day):hasC,
                               data = (all_MO2_slope_results_demTTRdata_genotype %>% filter(., blank_ratio < 0.10))) #Run the most significant model identified by step
```

I then compared AIC of the two models to decide which model to use moving forward.

```
AIC(lmAllSlopeResultsSig, lmBinarySlopeResultsSig) #Compare AIC of both models. Model with binary genotype has lower AIC so that will be the final model
write.csv(broom::tidy(lmBinarySlopeResultsSig), "all-slope-results-model-output.csv", quote = FALSE, row.names = FALSE)
```

The final model includes binary genotype, which has marginal significance of presence of the T allele.

I also modified the boxplot for respirometry to remove the dashed line connecting medians.

![Screenshot 2024-02-01 at 1 45 46 PM](https://github.com/yaaminiv/wc-green-crab/assets/22335838/9938b98a-f1f4-419f-a792-bb2c10c47c71)

**Figure 4**. Revised respirometry boxplot

Since I didn't take respirometry measurements at day 0 (we didn't even know if we had a respirometer we could use yet!) I couldn't do a Q10 analysis with the respirometry data. However, I could look at differences in oxygen consumption rates by genotype between day 7 (first day respirometry was done) and either day 14, 21, day 42.

```
all_MO2_slope_results_diff <- all_MO2_slope_results_reorder %>%
  ungroup(.) %>%
  arrange(., treatment) %>%
  select(., day, crab.ID, slope_nmol_hr_corrected, treatment, final.genotype) %>%
  filter(., day != "49") %>%
  mutate(hasC = case_when(final.genotype == "CC" ~ "Y",
                          final.genotype == "CT" ~ "Y",
                          final.genotype == "TT" ~ "N"),
         hasT = case_when(final.genotype == "CC" ~ "N",
                          final.genotype == "CT" ~ "Y",
                          final.genotype == "TT" ~ "Y")) %>%
  pivot_wider(., names_from = day, values_from = slope_nmol_hr_corrected, names_prefix = "day") %>%
  mutate(., day14v7 = -day14 + day07) %>%
  mutate(., day21v7 = -day21 + day07) %>%
  mutate(., day28v7 = -day28 + day07) %>%
  mutate(., day42v7 = -day42 + day07) #Select day, crab ID, slope, treatmet, and genotype information. Filter days of interest. Add binary genotype information. Pivot wider, using days as columns and values from slope and add "day" as a prefix to all columns. Get the difference in negative slope for comparisons of interest.
```

```
day14v7Plot <- all_MO2_slope_results_diff %>%
  filter(., is.na(final.genotype) == FALSE) %>%
  ggplot(mapping = aes(x = treatment, y = day14v7, color = treatment)) +
  geom_boxplot(outlier.shape = NA) + geom_point(aes(shape = treatment), size = 2) +
  facet_wrap(. ~ final.genotype) +
  labs(x = "", y = "", title = "A. Day 14 vs. Day 7") +
  scale_x_discrete(breaks = c("05C", "15C", "25C"),
                   labels = c("5", "15", "25")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     guide = "none") +
  scale_shape_manual(values = c(15, 19, 17),
                     guide = "none") +
  theme_classic(base_size = 15) + theme(axis.text.x = element_blank(),
                                        axis.ticks.x = element_blank())
```

```
day21v7Plot <- all_MO2_slope_results_diff %>%
  filter(., is.na(final.genotype) == FALSE) %>%
  ggplot(mapping = aes(x = treatment, y = day21v7, color = treatment)) +
  geom_boxplot(outlier.shape = NA) + geom_point(aes(shape = treatment), size = 2) +
  facet_wrap(. ~ final.genotype) +
  labs(x = "", y = "", title = "B. Day 21 vs. Day 7") +
  scale_x_discrete(breaks = c("05C", "15C", "25C"),
                   labels = c("5", "15", "25")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     guide = "none") +
  scale_shape_manual(values = c(15, 19, 17),
                     guide = "none") +
  theme_classic(base_size = 15) + theme(axis.text.x = element_blank(),
                                        axis.ticks.x = element_blank())
```

```
day42v7Plot <- all_MO2_slope_results_diff %>%
  filter(., is.na(final.genotype) == FALSE) %>%
  ggplot(mapping = aes(x = treatment, y = day42v7, color = treatment)) +
  geom_boxplot(outlier.shape = NA) + geom_point(aes(shape = treatment), size = 2) +
  facet_wrap(. ~ final.genotype) +
  labs(x = "Temperature (ºC)", y = "", title = "C. Day 42 vs. Day 7") +
  scale_x_discrete(breaks = c("05C", "15C", "25C"),
                   labels = c("5", "15", "25")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     guide = "none") +
  scale_shape_manual(values = c(15, 19, 17),
                     guide = "none") +
  theme_classic(base_size = 15)
```

```
#pdf("figures/oxygen-consumption-individuals-genotype-multiday-multipanel.pdf", height = 11, width = 8.5)

day14v7Plot / day21v7Plot / day42v7Plot
grid::grid.draw(grid::textGrob("Difference in Oxygen Consumption (nmol/hr)", x = 0.02, rot = 90, gp = gpar(fontsize = 15)))

#dev.off()
```

<img width="683" alt="Screenshot 2024-02-01 at 8 29 07 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/0d0a12f4-f759-43f9-b8e1-e973a3214a1d">

**Figure 5**. Differences in respiration rate across time and by genotype.

I gotta admit...this plot is not as interesting as the TTR plot was. However, I see that the differences in respiration for CT and TT may underlie the variability in respiration rate at 25ºC. I also see changes in respiration rate over time in the control. I'm not sure why this is the case...could this be a signal of captivity, handling, or not fully digesting? More things to consider when fine-tuning these analyses.

### Going forward

1. Work with Vanessa to leave decontamination station
2. Work with Vanessa to finish extracting respirometry samples
2. Work with Vanessa to continue with Chelex extractions, PCRs, and gels for TTR crabs
3. Perform Qubit assay with any sample consistently not showing up on a gel
4. Genotype remaining samples
4. Examine HOBO data from 2023 experiment
5. Determine best statistical approach for analyzing performance data
5. Demographic data analysis for 2023 paper
6. Update methods and results of 2023 paper

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
