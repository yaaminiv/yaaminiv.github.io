---
layout: post
comments: true
title: Green Crab Experiment Part 53
tags: green-crab glmm
editor_options:
  markdown:
    wrap: 72
---

## Perfecting my TTR model and interpretation

[Previously](https://yaaminiv.github.io/Green-Crab-Experiment-Part52/), I revised the TTR analysis to use a GLMM instead of a GLM! I then went through the process of interpreting the model. Temperature, day, and their interaction significantly impacted righting response but interestingly, so did sex. Which got me thinking...........I should probably investigate the potential of a sex:temperature or sex:day interaction on the model. Back to the...modeling board?

### Investigating the potential for an interaction

First I poured one out for my new model which required revision almost immediately. Then I started to dig down into the model. My first question was whether or not there was a significant difference in TTR between female and male crabs only due to sample size. I looked at the sample size differences by sex, sex and temperature, sex and day, and sex, temperature, and day. There weren't glaring differences in sample size that would impact my ability to detect a true biological signal. However, I think my ability to detect a sex:temperature:day signal is really limited by sample size, so I don't think pursuing the three-way interaction is in the cards for this dataset.

I then plotted TTR by sex for the entire experiment. Looks like females had significantly faster righting time when compared to males. Of course, this could be due in part to the fact female crabs were smaller than males, both in carapace width and weight. However, since weight was dropped from the model during model selection, then it is possible that sex encompasses not only size differences between crabs, but maybe some inherent molecular differences how they respond to temperature. This dovetails nicely with Arrigo et al. (2025), which showed differences in enzyme activity in female crabs vs. male crabs exposed to a MHW. I'll dig into this paper more when writing elements of the discussion.

At this point, I was pretty sure that adding some sort of interaction effect with either temperature or day made sense.

### Revised model

I added a sex:temperature and sex:day interaction effect to the initial model, then proceeded with backwards deletion model selection using `drop1`.

```
TTRmodelSex <- glmmTMB( #Generalized linear mixed effect model
  TTRavg ~ as.factor(treatment) + day.sc + as.factor(treatment):day.sc +  #Treatment, day, and their interaction
    as.factor(sex) + as.factor(sex):as.factor(treatment) + as.factor(sex):day.sc + #Sex, interaction between sex and temperature, and interaction between sex and day
    integument.sc + carapace.width.sc + number.swimmers.sc + #Demographic variables: integument color, carapace width, and number of swimmers
    (1 | treatment.tank), #Use tank as a random effect to account for potential dependence between data
  data = modTTR,
  family = Gamma(link = "log") #Gamma model with a log link
)
```

Very fun update: I learned that you could use the function `update` to remove predictors from a model without having to retype the model over and over!

```
drop1(TTRmodelSex) #Dropping carapace with and number of swimmers leads to the same AIC. I will remove carapace width since that is less central to the core hypothesis, and I want to ensure missing swimmers did not impact righting response.
```

My final model included temperature, day, their interaction, sex, and the interaction between sex and temperature as significant predictors. But of course, when I went to check assumptions, there were significant differences in outlier distribution and dispersion with this revised model.

<img width="868" height="537" alt="Image" src="https://github.com/user-attachments/assets/d93f8d57-cfc3-40ff-8401-c95a1d7a4ab4" />

**Figure 1**. Residual plot for revised (sex:temperature) model.

I started by troubleshooting the same way Gemini suggested with my previous model: 1) adding different dispersion terms by temperature and 2) adding day as a polynomial term. When I added different dispersion by temperature, I did not see any significant improvement in the residual plot, and AIC of the model increased from 918.90 to 922.62.

<img width="874" height="543" alt="Image" src="https://github.com/user-attachments/assets/a3178721-9924-4097-afb3-6e094ccc420b" />

**Figure 2**. Residual plot for sex:temperature model with dispersion modeled by temperature

When I added day as a polynomial term, it was dropped in the second or third round of model selection. I once again used Gemini as a resource to understand if there were other tweaks I could make to the model to improve the model fit. It suggested modeling dispersion by sex, or by sex and temperature additively.

<img width="863" height="532" alt="Image" src="https://github.com/user-attachments/assets/8ac23cb7-8bd0-41a0-92c4-a4463f578b0c" />

**Figure 3**. Residual plot for sex:temperature model with dispersion modeled by sex

<img width="861" height="535" alt="Image" src="https://github.com/user-attachments/assets/bdc61b73-d378-4782-ac5d-9ce7cebe1ebd" />

**Figure 4**. Residual plot for sex:temperature model with dispersion modeled by sex and temperature

Modeling dispersion by sex didn't improve residuals, but it did bring AIC down to 906.24. However, modeling dispersion by sex and temperature both improved the residual plot and lowered AIC to 905.82! I decided to proceed with the following model:

```
TTRmodelSex5 <- glmmTMB( #Generalized linear mixed effect model
  TTRavg ~ as.factor(treatment) + day.sc + as.factor(treatment):day.sc +  #Treatment, day, and their interaction
    as.factor(sex) + as.factor(sex):as.factor(treatment) + #Sex, interaction between sex and temperature
    (1 | treatment.tank), #Use tank as a random effect to account for potential dependence between data
  data = modTTR,
  dispformula = ~ as.factor(sex) + as.factor(treatment),
  family = Gamma(link = "log") #Gamma model with a log link
)
```

I checked out all my assumptions for this revised model. Nothing notable to report outside of the residual distribution throwing a significant K-S test p-value. My model bootstrapping was consistent with the model summary and my Wald test showed that all predictors were significantly impacting righting response. The output from this revised model can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/03-TTR-analysis), with filenames starting with "TTRmodelSex5".

Unlike a standard LMM, I can't estimate the percent of total variance explained by the random effect. There are apparently tests to calculate equivalent parameters for GLMMs, but they cannot be implemented on my model since I have custom dispersion estimates. I used a combination of LRT and AIC comparisons to determine the "importance" of the random effect:

```
# Fit equivalent model with the random intercept
TTRmodelSex5_noRE <- glmmTMB(
  TTRavg ~ as.factor(treatment) * day.sc + as.factor(sex) * as.factor(treatment),
  dispformula = ~ as.factor(sex) + as.factor(treatment),
  data = modTTR,
  family = Gamma(link = "log")
)

# Likelihood Ratio Test
anova(TTRmodelSex5, TTRmodelSex5_noRE) #Non-significant LRT output (chisq = 0.7861, P = 0.3753)

# Compare AICs
AIC(TTRmodelSex5, TTRmodelSex5_noRE) #Lower AIC without random effect (904.6060 vs. 905.8199)
```

I also calculated the standard deviation and variance associated with the random effect:

```
summary(TTRmodelSex5)$varcor #Random effect associated with 0.073579 SD
0.073579^2 #Variance = 0.005413869
```

The random effect was not significant to the model! That is interesting, but I think also a sign that the random effect explains very little variance. I included this information in the results.

### Estimated marginal means

Now to understand differences in pairwise conditions. This is where things got a bit tricky for me to understand. I know I've done this before, but I wanted to go back to the [`emmeans` manual](https://rvlenth.github.io/emmeans/index.html) to ensure that I was going about the statistical tests correctly, especially because I had multiple interactions to investigate. I found this [Stack Overflow post](https://stats.stackexchange.com/questions/497512/glmm-pairwise-contrasts-with-time-interactions-via-emmeans) somewhat helpful for deciphering parts of the manual. Here are my high-level takeaways:

1. If there is a significant interaction, it makes sense to conduct pairwise tests for the interaction *only*, as investigating the significant main effects will not be fully accurate
2. Always create a table with the estimated marginal means (EMM), *then* conduct the pairwise comparisons. Both myself and apparently everyone else try and lump these two steps into one, but that can 1) lead to statistical inaccuracies and 2) is harder to interpret when reading someone's code.
3. The difference between `pairs` and `contrasts` in `emmeans` still isn't clear to me...but because I am investigating pairwise differences between various conditions, I used `pairs` to get the statistical output I needed

I first examined treatment differences at each day.

```
TTRmodel_EMM_tempDay <- emmeans(TTRmodelSex5, #Model name
                                ~ treatment | day.sc, #Significant predictor to investigate
                                at = list(day.sc = scale(c(4, 8, 11, 15, 18, 22),
                                                         center = mean(modTTR$day),
                                                         scale = sd(modTTR$day))), #Days where TTR was measured. Include scale to automatically back-transform scaled predictor variable, and specify the center and scale variables used in the initial scaling for back-transformation.
                                type = "response", #Convert back to the response scale since a log link was used
                                adjust = "bonferroni") #Use Bonferroni correction for multiple comparisons
TTRmodel_EMM_tempDay #Estimated marginal means

pairs(TTRmodel_EMM_tempDay) %>% #Get pairwise comparisons for treatment x day
  broom::tidy(.) %>% #Tidy output
  filter(., contrast != "30C / 5C") %>% #Remove comparisons between 30C and 5C
  mutate(., day = c(rep(4, times = 2),
                    rep(8, times = 2),
                    rep(11, times = 2),
                    rep(15, times = 2),
                    rep(18, times = 2),
                    rep(22, times = 2))) %>% #Create a column with actual date, not scaled date
  dplyr::select(day, contrast:adj.p.value) %>% #Retain columns of interest
  write.csv(., "TTRmodelSex5-emmeans-tempDay.csv", quote = FALSE, row.names = FALSE) #Save as a csv
```

Unsurprisingly, there were significant differences between control and cold treatments throughout the experiment, but there were only significant differences between control and warm treatments through day 11. After day 15, crabs in the warm treatment were no longer faster than their counterparts in control conditions. I also needed to understand how individual treatments changed over the course of time. To do this, I needed to recode day as a factor (something I did for [the 2024 crab experiment!](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part43/)), then calculate the EMM:

```
modTTR$day_factor <- as.factor(modTTR$day) #Code day as a factor
TTRmodelSex5_factor <- glmmTMB( #Generalized linear mixed effect model
  TTRavg ~ as.factor(treatment) + day_factor + as.factor(treatment):day_factor + #Temperature, time, and their interaction
    as.factor(sex) + as.factor(treatment):as.factor(sex) + #Sex and the interaction between temperature and sex
    (1 | treatment.tank), #Use tank as a random effect to account for potential dependence between data
  data = modTTR,
  dispformula = ~ as.factor(sex) + as.factor(treatment), #Model dispersion by additive sex and temperature
  family = Gamma(link = "log") #Gamma model with a log link
) #Run the original model, but use day as a factor


TTRmodel_EMM_dayTemp <- emmeans(TTRmodelSex5_factor, #Model name
                                ~ day_factor | treatment, #Significant predictor to investigate
                                type = "response", #Convert back to the response scale since a log link was used
                                adjust = "bonferroni") #Use Bonferroni correction for multiple comparisons
TTRmodel_EMM_dayTemp #Estimated marginal means


pairs(TTRmodel_EMM_dayTemp) %>% #Get pairwise comparisons for day x treatment
  broom::tidy(.) %>% #Tidy output
  dplyr::select(treatment, contrast:adj.p.value) %>% #Retain columns of interest
  write.csv(., "TTRmodelSex5_factor-emmeans-dayTemp.csv", quote = FALSE, row.names = FALSE) #Save as a csv
```

These were a bit harder to parse, with day 11 looking like a turning point for both the 5ºC and 30ºC treatments. Once again, output can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/03-TTR-analysis), with filenames starting with "TTRmodelSex5".

### Figure revision

After spending way too much time adding supplemental tables of `emmeans` output to the manuscript, it was time to revise the TTR figure. Mainly, I needed to show that there was a significant sex:temperature interaction! After waffling on how to do this, I settled on a multipanel plot, with one plot showing the temperature:day interaction (similar to what I already had) and another plot showing the sex:temperature interaction. I started by modifying my existing boxplot (one important thing I learned is how to actually use `patchwork` and inset specificiations with `ggplot` properly to collect legends):

```
mainPlot <- modTTR %>%
  dplyr::select(., c(day, treatment, TTRavg)) %>%
  distinct(.) %>%
  mutate(., treatment = gsub(x = treatment, pattern = "5C", replacement = "05C")) %>%
  mutate(., day = gsub(x = day, pattern = 4, replacement = "04")) %>%
  mutate(., day = gsub(x = day, pattern = 8, replacement = "08")) %>%
  mutate(., day = gsub(x = day, pattern = 11, replacement = "11")) %>%
  mutate(., day = gsub(x = day, pattern = 108, replacement = "18")) %>%
  mutate(., day = gsub(x = day, pattern = 15, replacement = "15")) %>%
  mutate(., day = gsub(x = day, pattern = 22, replacement = "22")) %>%
  ggplot(mapping = aes(x = day, y = TTRavg, color = treatment, shape = treatment)) +
  geom_boxplot(outlier.shape = NA) + geom_jitter(position = position_dodge(width = 0.75)) +
  ylab("Average Time-to-Right (s)") +
  scale_x_discrete(name = "Day",
                   breaks = c("04", "08", "11", "18", "15", "22"),
                   labels = c("4", "8", "11", "18", "15", "22")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  ggtitle("A. Temperature x day") +
  theme_classic(base_size = 15) + theme(legend.position = "bottom") #Select unique day, treatment, and TTRavg data. Replace numbers with characters to facilitate better ordering. Plot average TTR for each crab in a boxplot. Do not show outliers with the OG boxplot, but add them in with geom_jitter. Assign colors to each treatment. Add annotations with Wald test output for significant predictors. Increase base font size.

insetPlot <-  modTTR %>%
  dplyr::select(., c(day, treatment, TTRavg)) %>%
  distinct(.) %>%
  filter(., treatment != "5C") %>%
  mutate(., day = gsub(x = day, pattern = 4, replacement = "04")) %>%
  mutate(., day = gsub(x = day, pattern = 8, replacement = "08")) %>%
  mutate(., day = gsub(x = day, pattern = 11, replacement = "11")) %>%
  mutate(., day = gsub(x = day, pattern = 108, replacement = "18")) %>%
  mutate(., day = gsub(x = day, pattern = 15, replacement = "15")) %>%
  mutate(., day = gsub(x = day, pattern = 22, replacement = "22")) %>%
  ggplot(mapping = aes(x = day, y = TTRavg, color = treatment, shape = treatment)) +
  geom_boxplot(outlier.shape = NA) + geom_jitter(position = position_dodge(width = 0.75)) +
  ylab("") +
  scale_x_discrete(name = "",
                   breaks = c("04", "08", "11", "18", "15", "22"),
                   labels = c("4", "8", "11", "18", "15", "22")) +
  scale_color_manual(values = c(plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("13C", "30C"),
                     labels = c("13", "30")) +
  scale_shape_manual(values = c(19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("13C", "30C"),
                     labels = c("13", "30")) +
  scale_y_continuous(limits = c(0,3)) +
  guides(color = "none", shape = "none") +
  theme_classic() + theme(legend.position = "none",
                          axis.line.x.bottom = element_line(linewidth = 0),
                          axis.line.y.left = element_line(linewidth = 0),
                          panel.border = element_rect(colour = "black", fill = NA, size = 1))

plotWithInset <- mainPlot +
  inset_element(insetPlot, left = 0.35, bottom = 0.655, right = 1, top = 0.95)

sexPlot <- modTTR %>%
  dplyr::select(., c(sex, treatment, TTRavg)) %>%
  distinct(.) %>%
  mutate(., treatment = gsub(x = treatment, pattern = "5C", replacement = "05C")) %>%
  ggplot(mapping = aes(x = sex, y = TTRavg, color = treatment, shape = treatment)) +
  geom_boxplot(outlier.shape = NA) + geom_jitter(position = position_dodge(width = 0.75)) +
  ylab("Average Time-to-Right (s)") +
  scale_x_discrete(name = "Sex") +
  scale_y_continuous(name = "") +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  ggtitle("B. Temperature x sex") +
  theme_classic(base_size = 15) + theme(legend.position = "bottom") #Select unique day, treatment, and TTRavg data. Replace numbers with characters to facilitate better ordering. Plot average TTR for each crab in a boxplot. Do not show outliers with the OG boxplot, but add them in with geom_jitter. Assign colors to each treatment. Add annotations with Wald test output for significant predictors. Increase base font size.

sexInsetPlot <-  modTTR %>%
  dplyr::select(., c(sex, treatment, TTRavg)) %>%
  filter(., treatment != "5C") %>%
  ggplot(mapping = aes(x = sex, y = TTRavg, color = treatment, shape = treatment)) +
  geom_boxplot(outlier.shape = NA) + geom_jitter(position = position_dodge(width = 0.75)) +
  ylab("") +
  scale_x_discrete(name = "") +
  scale_color_manual(values = c(plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("13C", "30C"),
                     labels = c("13", "30")) +
  scale_shape_manual(values = c(19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("13C", "30C"),
                     labels = c("13", "30")) +
  scale_y_continuous(limits = c(0,3)) +
  guides(color = "none", shape = "none") +
  theme_classic() + theme(legend.position = "none",
                          axis.line.x.bottom = element_line(linewidth = 0),
                          axis.line.y.left = element_line(linewidth = 0),
                          panel.border = element_rect(colour = "black", fill = NA, size = 1))

sexPlotWithInset <- sexPlot +
  inset_element(sexInsetPlot, left = 0.40, bottom = 0.655, right = 1, top = 0.95)

(plotWithInset + sexPlotWithInset) +
  plot_layout(guides = "collect") &
  theme(legend.position = "bottom")
ggsave("figures/time-to-right-multipanel.pdf", width = 11, height = 8.5)
```

<img width="1593" height="1220" alt="Image" src="https://github.com/user-attachments/assets/ff0e146c-ed18-4a49-9f45-1fa2492d1730" />

**Figure 5**. Multipanel boxplot

Just when I thought I was done, I realized that in the boxplots, the males looked faster than the females at 13ºC and 30ºC! This is the opposite of what the model found. I realized it's because the boxplots show *medians* whereas the model works with *means*. I wanted to have a plot that reflected both the data used and the average values used in the model. Ariana suggested plotting average ± SE values, then having some transparent data points underneath to show the distribution of data. I messed around with an existing average ± SE plot I had to produce this:

<img width="1528" height="1164" alt="Image" src="https://github.com/user-attachments/assets/0ef13699-2864-4e31-9454-c406ec63fad7" />

**Figure 6**. Multipanel plot with averages

My issue with this iteration was that I wasn't confident with my standard error calculations. After confirming that I did in fact calculate them correctly, I wasn't sure what to do next. Chhaya suggested plotting EMM and the associated SE instead of averages ± SE calculated from the data. This is because the model takes into account variance explained by the random effect, and by plotting the EMM, I can show these updated estimates. This was easy enough to do. I first took my EMM tables (good thing I have those now) and saved them as new objects. I then used `left_join` to bind them with my `modTTR` dataset and use the information for plotting:

```
tempDay_EMM <- TTRmodel_EMM_tempDay %>% #Take emmeans output
  broom::tidy(.) %>% #Tidy it
  mutate(day = c(rep(4, times = 3),
                 rep(8, times = 3),
                 rep(11, times = 3),
                 rep(15, times = 3),
                 rep(18, times = 3),
                 rep(22, times = 3))) %>% #Add a column with real, not scaled dates
  dplyr::select(treatment, day, response, std.error) %>% #Keep columns of interest
  mutate(., LCL = response - std.error,
         UCL = response + std.error) #Create columns for upper and lower confidence intervals
```

```
tempSex_EMM <- TTRmodel_EMM_tempSex %>% #Take emmeans output
  broom::tidy(.) %>% #Tidy it
  dplyr::select(treatment, sex, response, std.error) %>% #Keep columns of interest
  mutate(., LCL = response - std.error,
         UCL = response + std.error) #Create columns for upper and lower confidence intervals
head(tempSex_EMM) #Confirm formatting
```

```
avgPlot <- modTTR %>%
  left_join(., y = tempDay_EMM, by = c("treatment", "day")) %>% #Join with EMM
  dplyr::select(., c(day, treatment, TTRavg, response, LCL, UCL)) %>% #Select columns of interest
  distinct(.) %>% #Remove duplicate rows
  mutate(., treatment = gsub(x = treatment, pattern = "5C", replacement = "05C")) %>% #Replace 5 with 05 so that temperatures are presented in ascending order
  mutate(., day = gsub(x = day, pattern = 4, replacement = "04")) %>% #Add a 0 in front of days for proper ordering
  mutate(., day = gsub(x = day, pattern = 8, replacement = "08")) %>%
  mutate(., day = gsub(x = day, pattern = 11, replacement = "11")) %>%
  mutate(., day = gsub(x = day, pattern = 108, replacement = "18")) %>% #Fix data entry order
  mutate(., day = gsub(x = day, pattern = 15, replacement = "15")) %>%
  mutate(., day = gsub(x = day, pattern = 22, replacement = "22")) %>%
  ggplot(., mapping = aes(x = day,
                          y = response,
                          color = treatment,
                          shape = treatment)) + #Define main plot aesthetics
  geom_pointrange(data = . %>% dplyr::select(day, treatment, response, LCL, UCL) %>% distinct(.),
                  aes(ymin = LCL,
                      ymax = UCL),
                  size = 1,
                  linewidth = 0.8,
                  position = position_jitterdodge(jitter.width = 0.12),
  ) + #Add means and standard errors to the plot
  geom_point(aes(x = day,
                 y = TTRavg,
                 color = treatment),
             position = position_jitterdodge(jitter.width = 0.12),
             size = 1.5,
             alpha = 0.3) + #Add underlying data points behind
  scale_x_discrete(name = "Day",
                   breaks = c("04", "08", "11", "18", "15", "22"),
                   labels = c("4", "8", "11", "18", "15", "22")) + #Define x axis
  scale_y_continuous(name = "Average Time-to-Right (s)",
                     limits = c(0, 50)) + #Define y axis
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  ggtitle("A. Temperature x day") +
  theme_classic(base_size = 15) + theme(legend.position = "bottom")  #Plot average TTR and add lm fits without SE bars. Add vertical line for boxplot outlier threshold. Modify x-axis to only show experimental days where TTR was measured. Scale y axis to include 91 = indication that crabs did not right within 90 s. Assign colors to each treatment. Increase base font size.

avgInsetPlot <- modTTR %>%
  left_join(., y = tempDay_EMM, by = c("treatment", "day")) %>% #Join with EMM
  dplyr::select(., c(day, treatment, TTRavg, response, LCL, UCL)) %>% #Select columns of interest
  distinct(.) %>% #Remove duplicate rows
  filter(., treatment != "5C") %>% #Remove 5C data
  mutate(., day = gsub(x = day, pattern = 4, replacement = "04")) %>% #Add a 0 in front of days for proper ordering
  mutate(., day = gsub(x = day, pattern = 8, replacement = "08")) %>%
  mutate(., day = gsub(x = day, pattern = 11, replacement = "11")) %>%
  mutate(., day = gsub(x = day, pattern = 108, replacement = "18")) %>% #Fix data entry order
  mutate(., day = gsub(x = day, pattern = 15, replacement = "15")) %>%
  mutate(., day = gsub(x = day, pattern = 22, replacement = "22")) %>%
  ggplot(., mapping = aes(x = day,
                          y = response,
                          color = treatment,
                          shape = treatment)) + #Define main plot aesthetics
  geom_pointrange(data = . %>% dplyr::select(day, treatment, response, LCL, UCL) %>% distinct(.),
                  aes(ymin = LCL,
                      ymax = UCL),
                  # size = 0.75,
                  linewidth = 0.8,
                  position = position_jitterdodge(jitter.width = 0.12),
  ) + #Add means and standard errors to the plot
  geom_point(aes(x = day,
                 y = TTRavg,
                 color = treatment),
             position = position_jitterdodge(jitter.width = 0.12),
             alpha = 0.3) + #Add underlying data points behind
  scale_x_discrete(name = "Day",
                   breaks = c("04", "08", "11", "18", "15", "22"),
                   labels = c("4", "8", "11", "18", "15", "22")) + #Define x axis
  ylab("") +
  scale_y_continuous(limits = c(0,3.5)) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  guides(color = "none", shape = "none", pointrange = "none") +
  theme_classic() + theme(legend.position = "none",
                          axis.line.x.bottom = element_line(linewidth = 0),
                          axis.line.y.left = element_line(linewidth = 0),
                          panel.border = element_rect(colour = "black", fill = NA, size = 1))


avgPlotWithInset <- avgPlot +
  inset_element(avgInsetPlot, left = 0.40, bottom = 0.555, right = 1, top = 0.85)

avgPlotWithInset

avgSexPlot <- modTTR %>%
 left_join(., y = tempSex_EMM, by = c("treatment", "sex")) %>% #Join with EMM
  dplyr::select(., c(sex, treatment, TTRavg, response, LCL, UCL)) %>% #Select columns of interest
  distinct(.) %>% #Remove duplicate rows
  mutate(., treatment = gsub(x = treatment, pattern = "5C", replacement = "05C")) %>% #Replace 5 with 05 so that temperatures are presented in ascending order
  ggplot(., mapping = aes(x = sex,
                          y = response,
                          color = treatment,
                          shape = treatment)) + #Define main plot aesthetics
  geom_pointrange(data = . %>% dplyr::select(sex, treatment, response, LCL, UCL) %>% distinct(.),
                  aes(ymin = LCL,
                      ymax = UCL),
                  size = 1,
                  linewidth = 0.8,
                  position = position_jitterdodge(jitter.width = 0.12),
  ) + #Add means and standard errors to the plot
  geom_point(aes(x = sex,
                 y = TTRavg,
                 color = treatment),
             position = position_jitterdodge(jitter.width = 0.12),
             size = 1.5,
             alpha = 0.3) + #Add underlying data points behind
  scale_x_discrete(name = "Sex") + #Define x axis
  scale_y_continuous(name = "",
                     limits = c(0, 50)) + #Define y axis
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  ggtitle("B. Temperature x sex") +
  theme_classic(base_size = 15) + theme(legend.position = "bottom") #Select unique day, treatment, and TTRavg data. Replace numbers with characters to facilitate better ordering. Plot average TTR for each crab in a boxplot. Do not show outliers with the OG boxplot, but add them in with geom_jitter. Assign colors to each treatment. Add annotations with Wald test output for significant predictors. Increase base font size.

avgSexInsetPlot <-  modTTR %>%
 left_join(., y = tempSex_EMM, by = c("treatment", "sex")) %>% #Join with EMM
  dplyr::select(., c(sex, treatment, TTRavg, response, LCL, UCL)) %>% #Select columns of interest
  distinct(.) %>% #Remove duplicate rows
  filter(., treatment != "5C") %>% #Remove 5C data
  ggplot(., mapping = aes(x = sex,
                          y = response,
                          color = treatment,
                          shape = treatment)) + #Define main plot aesthetics
  geom_pointrange(data = . %>% dplyr::select(sex, treatment, response, LCL, UCL) %>% distinct(.),
                  aes(ymin = LCL,
                      ymax = UCL),
                  # size = 1,
                  linewidth = 0.8,
                  position = position_jitterdodge(jitter.width = 0.12),
  ) + #Add means and standard errors to the plot
  geom_point(aes(x = sex,
                 y = TTRavg,
                 color = treatment),
             position = position_jitterdodge(jitter.width = 0.12),
             alpha = 0.2) + #Add underlying data points behind
  scale_x_discrete(name = "Sex") + #Define x axis
  ylab("") +
  scale_y_continuous(name = "",
                     limits = c(0, 50)) + #Define y axis
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  scale_y_continuous(limits = c(0,3.5)) +
  guides(color = "none", shape = "none", pointrange = "none") +
  theme_classic() + theme(legend.position = "none",
                          axis.line.x.bottom = element_line(linewidth = 0),
                          axis.line.y.left = element_line(linewidth = 0),
                          panel.border = element_rect(colour = "black", fill = NA, size = 1))

avgSexPlotWithInset <- avgSexPlot +
  inset_element(avgSexInsetPlot, left = 0.40, bottom = 0.555, right = 1, top = 0.85)

avgSexPlotWithInset

(avgPlotWithInset + avgSexPlotWithInset) +
  plot_layout(guides = "collect") &
  theme(legend.position = "bottom")
ggsave("figures/time-to-right-averages-multipanel.pdf", width = 11, height = 8.5)
```

<img width="1524" height="1151" alt="Image" src="https://github.com/user-attachments/assets/05d284f5-8ecc-4492-b1af-5963600130e1" />

**Figure 7**. Multipanel plot with EMM

I think the plot with EMM looks good! It is also interesting to see how the EMM from the model don't include a dip in average TTR at day 11 like the boxplot and average TTR plot did. Time to leave this analysis for now and proceed with the rest of my reviewer comments.

### Going forward

1.  Address remaining methods comments
2.  Address results comments
3.  Address figure comments
4.  Address discussion comments
5.  Send revised manuscript to co-authors

{% if page.comments %}

::: {#disqus_thread}
:::

```{=html}
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
```

<noscript>Please enable JavaScript to view the
<a href="https://disqus.com/?ref_noscript">comments powered by
Disqus.</a></noscript>

{% endif %}

```{=html}
<script id="dsq-count-scr" src="//the-responsible-grad-student.disqus.com/count.js" async></script>
```
