---
layout: post
comments: true
title: Green Crab Experiment Part 25
tags: green-crab
---

## Simplifying the demographic data analysis

Carolyn (rightfully so) pointed out that my [demographic data PCA](https://yaaminiv.github.io/Green-Crab-Experiment-Part14/) was confusing to interpret! First off, I know that there are likely differences between temperature, so adding temperature into the PCA would just confirm what we already thought. It also doesn't really answer the question, which is are there differences between demographic variables before and after the experiment. Carolyn suggested I do some simpler one-variable tests to get at these questions.

I returned to [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/02-demographic-data.Rmd), since I knew that Sara was performed one-variable analyses. When poking through her code, I realized that she was using data from all of the time-to-right measurements, which didn't include data for day 1 of the experiment! I imported this data when I ran the PCA, so I just modified the code chunk and moved it up in the script for what I needed:

```
all_crab <- read.csv("../../data/crab-metadata.csv") %>%
  mutate(., date = rep("7/5/2022", times = nrow(.))) %>%
  select(., c(date, treatment.tank, carapace.width, carapace.length, weight, integument.color, sex)) %>%
  rbind(., (read.csv("../../data/time-to-right.csv") %>%
              select(., c(date, treatment.tank, carapace.width, carapace.length, weight, integument.color, sex)))) %>%
  mutate(., integument.cont = recode(integument.color,
                                     "B" = 0,
                                     "BG" = 0.5,
                                     "G" = 1,
                                     "YG" = 1.5,
                                     "Y" = 2,
                                     "YO" = 2.5 ,
                                     "O" = 3,
                                     "RO" = 3.5)) %>%
  mutate(., treatment = case_when(treatment.tank == 1 ~ "13",
                                  treatment.tank == 2 ~ "05",
                                  treatment.tank == 3 ~ "30",
                                  treatment.tank == 4 ~ "13",
                                  treatment.tank == 5 ~ "05",
                                  treatment.tank == 6 ~ "30")) %>%
  mutate(day = recode(date,
                      "7/5/2022" = 1,
                      "7/8/2022" = 4,
                      "7/12/2022" = 8,
                      "7/15/2022" = 11,
                      "7/19/2022" = 15,
                      "7/22/2022" = 18,
                      "7/26/2022" = 22))
#Import original metadata. Add a date column and select the necessary columns. Combine by columns (rbind) with demographic information from the time-to-right datasheet. Recode integument color into numerical values since it is continuous. No quotes around numbers = will be recognized as numeric values. Use case_when to reassign tanks as treatment temperatures. Recode dates as experimental day
```

Specifically, I recoded integument color as a continuous variable, specified treatment from tanks, and added a column for experimental day. With this modified dataset, I could create figures for each variable of interest: carapace width, carapace length, weight, integument color, and sex ratio. Knowing that I needed to put each day in a different facet, I created some overall facet labels:

```
facet_labels <- c("1" = "Day 1",
                  "22" = "Day 22") #Assign labels for facets
```

Then I could plot data from days 1 and 22:

```
all_crab %>%
  select(., day, treatment, carapace.width) %>%
  filter(., day == 1 | day == 22) %>%
  ggplot(., aes(x = treatment, y = carapace.width, color = treatment, shape = treatment)) +
  geom_boxplot() + geom_point() +
  facet_wrap( ~ day, labeller = labeller(day = facet_labels)) +
  stat_compare_means() +
  ylim(35, 60) +
  labs(x = "", y = "Carapace Width (mm)") +
  scale_x_discrete(labels = c("", "", "")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05", "13", "30"),
                     labels = c("5", "13", "30")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05", "13", "30"),
                     labels = c("5", "13", "30")) +
  theme_classic(base_size = 15) + theme(axis.ticks.x = element_blank(),
                                        axis.title.x = element_blank(),
                                        strip.text.x = element_text(size = 15)) #Select day 1 and day 22 data for factor of interest. Plot boxplot and points with treatment on the x-axis and factor on the y-axis, with shape and color defined  by treatment. Facet wrap by day and add specific facet labels. Add stats for comparisons between means and modify y-axis limits to fit statistical testing output at the top. Relabel x and y axes. Remove x-axis labels. Manually add color and shape legend. Set theme and modify bckground colors and strip text size.
```

For carapace width, carapace length, and weight, I could easily use `stat_compare_means` to run a Kruskal-Wallis test and include the p-value on the plot. Unfortunately, a similar functionality doesn't exist for chi-squared tests, which is what I need for integument color and sex ratio. The goal with the chi-squared is similar to DML enrichment tests I've run in the past: is the distribution of crabs of various colors or sexes consistent between treatments within each day? Carolyn asked if factors make sense for integument color since green is closer to yellow than red, but since the goal isn't to track progression but instead to just see if the compositions are the same, I think factors do make sense.

For integument color and sex ratio, I first needed to group the data by variables of interest, ungrouped, then used `summarize` to run a chi-squared test:

```
integumentColorDay1 <- all_crab %>%
  select(., day, treatment, integument.cont) %>%
  filter(., day == 1 ) %>%
  arrange(day, treatment, integument.cont) %>%
  group_by(day, treatment, integument.cont) %>%
  ungroup %>%
  summarize(., pvalDay1 = chisq.test(x = treatment, y = as.factor(integument.cont))$p.value) %>%
  as.data.frame() #Take day 1 data and arrange and group by relevant variables. Ungroup and use summarize to perform a chi-squared test examining differences in integument color by treatment. Extract p-value and save as a dataframe
```

I initially tried using `annotate` to add text, but this added the same text to each panel! Instead, I had to take the output of each chi-squared test and add it to a new object. This object could be called within `geom_text` to add different text to each facet panel. The object defined the text, which facet to assign it to, as well as x- and y-coordinates:

```
integumentColor_text <- data.frame(label = c("Chi-squared, p = 0.74", "Chi-squared, p = 0.003"),
                                   day = c(1, 22),
                                   x = c(1.65, 1.7),
                                   y = c(1.07, 1.07))
```

Then, I could create a stacked barplot with the custom annotation, using `summarize` to get counts for each group of variables as the input for the stacked barplot:

```
all_crab %>%
  select(., day, treatment, integument.cont) %>%
  filter(., day == 1 | day == 22) %>%
  arrange(day, treatment, integument.cont) %>%
  group_by(day, treatment, integument.cont) %>%
  summarize(., count = n()) %>%
  ggplot(., aes(x = treatment, y = count, fill = as.factor(integument.cont))) +
  geom_bar(position = "fill", stat = "identity") +
  facet_wrap( ~ day, labeller = labeller(day = facet_labels)) +
  labs(y = "Percent of Crabs") + coord_cartesian(ylim = c(0, 1.06)) +
  scale_x_discrete(breaks = c("05", "13", "30"),
                   labels = c("5ºC ", "13ºC ", "30ºC")) +
  scale_fill_manual(name = "Integument Color",
                    values = c("darkturquoise", "#66C2A5", "#A6D96A", "#D9EF8B", "#FFFF99", "#FDAE61", "#F46D43", "#D53E4F"),
                    labels = c("B", "BG", "G", "YG", "Y", "YO", "O", "RO")) +
  geom_text(data = integumentColor_text, mapping = aes(x = x, y = y, label = label), inherit.aes = FALSE) +
  theme_classic(base_size = 15) + theme(axis.ticks.x = element_blank(),
                                        axis.title.x = element_blank(),
                                        strip.text.x = element_text(size = 15)) #Select day, treatment, and integument color (continuous) for days 1 and 22. Arrange and group by selected variables. Summarize by grouped variables to get counts. Plot counts as a stacked barplot. Modify labels and specify a color scale.
```

And of course, once I had all of my individual plots, I decided to toss four of them into a multipanel plot. I excluded carapace length, since there's a standard size ratio and carapace width is the one most commonly used as a demographic variable.

<img width="1062" alt="Screenshot 2024-01-24 at 11 02 21 AM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/c54988b5-2f2f-4e12-a3e6-1636064a0283">

**Figure 1**. Multipanel with carapace width, weight, integument color, and sex

As expected, there is no difference between treatments for carapace width, weight, or sex ratio on days 1 or 22. There isn't a difference in integument color on day 1, but there is on day 22. This supports what we saw: crabs at 30ºC went through the intermolt cycle quicker.

### Going forward

1. Modify temperature condition figure
2. Create experimental design figure
3. Repeat lipidomics WCNA with temperature and day separately
4. Look into MetaboAnalyst discussion comments
5. Determine how to get more common lipid names that match MetaboAnalyst formatting
6. Reach out to Shelly to see if she tried anything else for lipid analysis
7. Try RaMP enrichment instead of KEGG for lipid sets
6. Normalize for "captivity" effect
7. Look into individual lipid classes from earlier papers
8. Determine if I can add physiology data to `mixOmics` analyses
9. Integrate metabolomics and lipidomics data

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
