---
layout: post
comments: true
title: Green Crab Experiment Part 19
tags: green-crab respirometry
---

## Revising respirometry analysis

### Removing outliers by blank ratios

After showing Chris my data, he mentioned that I should cross-reference my outlier points with the blank ratios. Blank ratios need to be < 10% of the original repirometry data prior to blank correction. I revised my calculations in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/04-respirometry-analysis.Rmd) and only identified a few points with very high blank ratios. Unfortunately, the crabs with high blank ratios weren't the very obvious outlier points. Either way, I filtered the points with high blank ratios out prior to statistical analysis and plotting. This didn't change my results.

### Quantifying variability

Since I had such high variability in my 13ºC and 30ºC treatments, I wanted a way to quantify that! I calculated the oxygen consumption rate mean, SD, and CV for each treatment x day combination:

```{r}
all_MO2_slope_results_meanSDCV <- all_MO2_slope_results_reorder %>%
  group_by(., treatment, day) %>%
  mutate(., slope_nmol_hr_corrected_mean = mean(-slope_nmol_hr_corrected)) %>%
  mutate(., slope_nmol_hr_corrected_SD = sd(-slope_nmol_hr_corrected)) %>%
  mutate(., slope_nmol_hr_corrected_CV = (slope_nmol_hr_corrected_SD/slope_nmol_hr_corrected_mean) * 100) %>%
  ungroup(.) #Take all slope results and group by treatment and day. Calculate the mean, SD, and CV for each treatment x day combination. Ungroup data.
```

The data table is saved [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/04-respirometry-analysis/all-slope-results-mean-SD-CV.csv).

I wanted an easy way to visualize the SD and CV data, so I decided to create line graphs, with day on the x-axis and either SD or CV on the y-axis. I could then use the same color and shape assignments I used in previous graphs:

```{r}
all_MO2_slope_results_meanSDCV %>%
  select(., treatment, day, slope_nmol_hr_corrected_SD) %>%
  ggplot(., aes(x = day, y = slope_nmol_hr_corrected_SD, group = treatment, color = treatment)) +
  geom_line(aes(lty = treatment)) + geom_point(aes(shape = treatment), size = 2) +
  xlab("Day") + ylab("SD of Oxygen Consumption (nmol/hr)") +
  scale_x_discrete(breaks = c("04", "08", "15", "22"),
                   labels = c("4", "8", "15", "22")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  scale_linetype_manual(values = c(2, 1, 6),
                        name = "Temperature (ºC)",
                        breaks = c("05C", "13C", "30C"),
                        labels = c("5", "13", "30")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  theme_classic(base_size = 15)
ggsave("figures/multipanel-oxygen-consumption-corrected-SD.pdf", height = 8.5, width = 11)
#Create a line graph for the SD of oxygen consumption (nmol/hr) over experimental day. Define color, shape, and line type by treatment.
```

```{r}
all_MO2_slope_results_meanSDCV %>%
  select(., treatment, day, slope_nmol_hr_corrected_CV) %>%
  ggplot(., aes(x = day, y = slope_nmol_hr_corrected_CV, group = treatment, color = treatment)) +
  geom_line(aes(lty = treatment)) + geom_point(aes(shape = treatment), size = 2) +
  xlab("Day") + ylab("CV of Oxygen Consumption") +
  scale_x_discrete(breaks = c("04", "08", "15", "22"),
                   labels = c("4", "8", "15", "22")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  scale_linetype_manual(values = c(2, 1, 6),
                        name = "Temperature (ºC)",
                        breaks = c("05C", "13C", "30C"),
                        labels = c("5", "13", "30")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  theme_classic(base_size = 15)
ggsave("figures/multipanel-oxygen-consumption-corrected-CV.pdf", height = 8.5, width = 11)
#Create a line graph for the CV of oxygen consumption over experimental day. Define color, shape, and line type by treatment.
```

<img width="1141" alt="Screenshot 2023-10-10 at 9 08 33 AM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/11243cd9-fcca-478d-891e-6f84654fa3ab">

<img width="1141" alt="Screenshot 2023-10-10 at 9 08 43 AM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/4cf58c78-aeb8-4026-be26-fea0e3501746">

**Figures 1 and 2**. SD (top) and CV (bottom) of mean oxygen consumption for each treatment x day combination.

The SD plot clearly shows that variability is much higher for the 13ºC and 30ºC treatments, especially at day 22. It's interesting that CV for all three treatments is roughly equal at day 8, and that the 13ºC has a much higher CV than the 30ºC treatment at day 22. These patterns seem to  be more influenced by potential outliers.

### Pairwise Q10 calculations

When I was at UMass, Alex mentioned calculating Q10 values as another way to characterize the acclimation response of oxygen consumption to various temperatures. Q10 is a metric used to quantify temperature sensitivity, generally defined as the change in a physiological response over a 10 degree temperature interval. For most organisms, Q10 is around two (a two-fold change in a physiological or chemical response due to a 10 degree temperature change).


While most studies calculate Q10 metrics for individuals, Glandon et al. (2019) calculates treatment-level Q10 metrics to compare oxygen consumption data from their study exposing juvenile blue crabs to combined temperature x pCO<sub>2</sub> treatments with other decapod crustacean oxygen consumption literature:

Q10 = (R2/R1)<sup>(10/(T2-T1))</sup>

where T1 = temperature 1, T2 = temperature 2, R1 = respiration at T1, and R2 = respiration at T2.

I followed the methods from Glandon et al. (2019) and calculated pairwise Q10 metrics for each day with oxygen consumption data using the `Q10` function from the `respirometry` package. First, I selected day, treatment, and mean values, then pivotted the table to a wide format so it was easy to feed columns with my desired metrics to the function. I calculated Q10 metrics between 13ºC and 30ºC, 5ºC and 13ºC, and 5ºC and 30ºC. I'm most interested in the comparisons between the treatment temperatures and the 13ºC control:

```{r}
all_MO2_slope_results_Q10output <- all_MO2_slope_results_meanSDCV %>%
  select(day, treatment, slope_nmol_hr_corrected_mean) %>%
  unique(.) %>%
  mutate(temperature = case_when(treatment == "13C" ~ 13,
                                 treatment == "30C" ~ 30,
                                 treatment == "05C" ~ 5)) %>%
  pivot_wider(., names_from = treatment, values_from = c(slope_nmol_hr_corrected_mean, temperature)) %>%
  mutate(., Q10_13_30 = Q10(R1 = slope_nmol_hr_corrected_mean_13C, R2 = slope_nmol_hr_corrected_mean_30C, T1 = temperature_13C, T2 = temperature_30C)) %>%
  mutate(., Q10_13_5 = Q10(R1 = slope_nmol_hr_corrected_mean_05C, R2 = slope_nmol_hr_corrected_mean_13C, T1 = temperature_05C, T2 = temperature_13C)) %>%
  mutate(., Q10_30_5 = Q10(R1 = slope_nmol_hr_corrected_mean_05C, R2 = slope_nmol_hr_corrected_mean_30C, T1 = temperature_05C, T2 = temperature_30C)) %>%
  select(., -c(temperature_13C, temperature_30C, temperature_05C)) #Take input dataframe and pivot wider. Use treatment as IDs for new colums, and paste values from mean slope and tmeperature. Use the formula to calculate Q10 for each pairwise temperature comparison. Remove temperature columns.
```

The table with calculated Q10 is saved [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/04-respirometry-analysis/mean-slope-pairwise-Q10.csv). To visualize the Q10 values, I created another line graph:

```{r}
all_MO2_slope_results_Q10output %>%
  pivot_longer(., cols = starts_with("Q10_"), names_to = "comparison", values_to = "Q10") %>%
  select(., day, comparison, Q10) %>%
  filter(., comparison != "Q10_30_5") %>%
  ggplot(., aes(x = day, y = Q10, group = comparison)) +
  geom_line(aes(color = comparison, lty = comparison)) + geom_point(aes(color = comparison, shape = comparison), size = 2) +
  geom_line(y = 2, color = "black", lty = 5) +
  xlab("Day") + ylab("Pairwise Q10") +
  scale_y_continuous(lim = c(0,10),
                     breaks = seq(0,10,2)) +
  scale_x_discrete(breaks = c("04", "08", "15", "22"),
                   labels = c("4", "8", "15", "22")) +
  scale_color_manual(values = c(plotColors[3], plotColors[1]),
                     name = "Comparison",
                     breaks = c("Q10_13_5", "Q10_13_30"),
                     labels = c("13ºC vs. 5ºC", "13ºC vs. 30ºC")) +
  scale_linetype_manual(values = c(2, 6),
                        name = "Comparison",
                        breaks = c("Q10_13_5", "Q10_13_30"),
                        labels = c("13ºC vs. 5ºC", "13ºC vs. 30ºC")) +
  scale_shape_manual(values = c(15, 17),
                     name = "Comparison",
                     breaks = c("Q10_13_5", "Q10_13_30"),
                     labels = c("13ºC vs. 5ºC", "13ºC vs. 30ºC")) +
  theme_classic(base_size = 15)
ggsave("figures/pairwise-Q10.pdf", height = 8.5, width = 11)
#Create a line graph with pairwise Q10 metrics for control 13ºC vs. either the 5ºC or 30ºC treatments. First, pivot_longer the Q10 calculation table and select columns such that only day, Q10, and comparison information remains. Remove the 30ºC vs. 5ºC Q10 calculations. Plot the data, grouping by Q10 comparison. Assign colors, line type, and shape by comparison.
```

<img width="1141" alt="Screenshot 2023-10-10 at 9 18 23 AM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/59646ff1-6af5-4395-9164-7761d304e0ee">

**Figure 3**. Pairwise Q10 values over time. The dashed line is at two, which is roughly the Q10 value for most biological processes.

It's interesting that the Q10 values for 5ºC vs. 13ºC comparison are much higher than the 13ºC vs. 30ºC comparison. When Q10 ~ 1, it means that respiration is temperature-independent. As Q10 increases, a process becomes more temperature dependent. Most biological processes have Q10 ~ 2, but processes with large-scale protein conformational changes can have larger Q10 values. Based on this information, it seems like transitioning to a warmer temperature is less temperature dependent, or requires less physiological remodeling, than transitioning to a colder temperature. It'll be really interesting to compare the Q10 values with the metabolomics data to see if there's more metabolic remodeling occurring at 5ºC!

### Going forward

1. Update methods
4. Update results
5. Send draft to Carolyn
6. Review crustacean metabolomics papers to determine analytical methods
7. Analyze metabolomics data

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
