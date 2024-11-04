---
layout: post
comments: true
title: Green Crab Experiment Part 40
tags: green-crab TTR respirometry metabolomics lipidomics
---

## Revising figures

Just a quick lab notebook post to document that I was working on figure revisions this week! Carolyn made some specific comments on various figures so I thought it best to tackle them at once.

![Screenshot 2024-10-25 at 10 27 46 AM](https://github.com/user-attachments/assets/8d03c31c-1a61-456f-8279-ec703810bd82)

**Figure 1**. Experimental timeline with altered title for molecular sampling

<img width="1085" alt="Screenshot 2024-10-28 at 1 51 42 PM" src="https://github.com/user-attachments/assets/43da4483-262f-4801-a048-ef2eb1600c70">

**Figure 2**. TTR figure

Revised code from [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/03-TTR-analysis.Rmd) where I removed the threshold line (no longer a valid analysis), added an inset to plot ([with an outline!](https://stackoverflow.com/questions/26191833/add-panel-border-to-ggplot2)) to show control and warm crab data between 0 and 3 seconds, and added statistical information to the plot:

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
  annotate(geom = "text", x = 4.5, y = 32,
           label = expression(paste(bolditalic("13ºC vs. 30ºC: "), "t = -2.66, p = 0.008")),
           hjust = 0) +
  annotate(geom = "text", x = 4.5, y = 30.5,
           label = expression(paste(bolditalic("13ºC vs. 5ºC: "), "t = 15.48, p = 2.27 x ", 10^-38)),
           hjust = 0) +
    annotate(geom = "text", x = 4.5, y = 29,
           label = expression(paste(bolditalic("Day: "), "t = -1.55, p = 0.12")),
           hjust = 0) +
    annotate(geom = "text", x = 4.5, y = 27.5,
           label = expression(paste(bolditalic("Crab weight: "), "t = 1.47, p = 0.14")),
           hjust = 0) +
  theme_classic(base_size = 15) #Select unique day, treatment, and TTRavg data. Replace numbers with characters to facilitate better ordering. Plot average TTR for each crab in a boxplot. Do not show outliers with the OG boxplot, but add them in with geom_jitter. Assign colors to each treatment. Increase base font size.

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
  theme_classic() + theme(legend.position = "none",
                          axis.line.x.bottom = element_line(linewidth = 0),
                          axis.line.y.left = element_line(linewidth = 0),
                          panel.border = element_rect(colour = "black", fill = NA, size = 1))

plotWithInset <- ggdraw() +
  draw_plot(mainPlot) +
  draw_plot(insetPlot, x = 0.3, y = 0.73, width = 0.52, height = 0.25)

plotWithInset

ggsave("figures/time-to-right-avg-boxplot-noThresh.pdf", height = 8.5, width = 11)
```

<img width="1085" alt="Screenshot 2024-10-28 at 2 22 45 PM" src="https://github.com/user-attachments/assets/b8664d0f-2249-4a8b-acc1-dc233531da73">

**Figure 3**. Respirometry plot

Revised code from [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/04-respirometry-analysis.Rmd) where I changed the boxplot from a faceted plot and added statistical information to the whole plot:

```
all_MO2_slope_results_reorder %>%
  filter(., blank_ratio < 0.10) %>%
  ggplot(., aes(x = day, y = -slope_nmol_hr_corrected, color = treatment, shape = treatment)) +
  geom_boxplot(outlier.shape = NA) + geom_jitter(position = position_dodge(width = 0.75)) +
  xlab("") + ylab("Oxygen Consumption (nmol/hr)") +
  # stat_summary(fun = median,
  #              geom = "line",
  #              aes(group = 1),
  #              col = "black", lty = 2) +
  scale_x_discrete(name = "Day",
                   breaks = c("04", "08", "15", "22"),
                   labels = c("4", "8", "15", "22")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  annotate(geom = "text", x = 1, y = 30000,
           label = expression(paste(bolditalic("13ºC vs. 30ºC: "), "t = 2.38, p = 0.02")),
           hjust = 0) +
  annotate(geom = "text", x = 1, y = 29000,
           label = expression(paste(bolditalic("13ºC vs. 5ºC: "), "t = -8.09, p = 1.50 x", 10^-11)),
           hjust = 0) +
  annotate(geom = "text", x = 1, y = 28000,
           label = expression(paste(bolditalic("Day: "), "t = 3.75, p = 3.72 x", 10^-4)),
           hjust = 0) +
  theme_classic(base_size = 15)
ggsave("figures/multipanel-oxygen-consumption-corrected.pdf", height = 8.5, width = 11) #Create a boxplot for oxygen consumption (µmol/hr). Use -slope_nmol_hr so the larger the slope, the faster the oxygen consumption over the hour. Add a line connecting each boxplot midpoint. Facet wrap and use the labeller to relabel facets. Add consistent color and shape scales.
```

![Screenshot 2024-10-28 at 2 50 36 PM](https://github.com/user-attachments/assets/5e55f468-20ed-4fbe-8078-9079b7242bb3)

**Figure 4**. Multipanel plot with PLS-DA and WCNA information for metabolomics and lipidomcs analysis, modified in [this InDesign file](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/metabolomics-lipidomics-multipanel.indd)

![Screenshot 2024-10-29 at 12 04 36 PM](https://github.com/user-attachments/assets/1d39a009-d6fc-4e1e-9753-c85a0256d3a0)

**Figure 5**. Multipanel plot for significant enrichment results from metabolomics and lipidomisc analysis, modified in [this InDesign file](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/metabolomics-lipidomics-enrichment-multipanel.indd)

I still need to develop a new figure (either for the main text or supplement) showing differences in lipid class saturation/unsaturation at various temperatures, but that's a later problem.

### Going forward

1. Address remaining comments on paper draft
1. Interpret metabolomic and lipidomic WCNA correlations
2. Integrate physiology and -omics data with WCNA
3. Metabolomics and lipidomics DIABLO analysis
4. Outline discussion
5. Write discussion
5. Write abstract

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
