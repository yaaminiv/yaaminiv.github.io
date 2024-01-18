---
layout: post
comments: true
title: Green Crab Experiment Part 24
tags: green-crab time-to-right respirometry
---

## Revising analyses

Carolyn gave me some feedback on the methods and results for the physiology analyses! Just some notes on those revisions.

### Time-to-right

Carolyn suggested using a GLM for TTR data instead of an ANOVA. Initially [I tried both GLM and ANOVA](https://yaaminiv.github.io/Green-Crab-Experiment-Part16/) to analyze the TTR data, but settled on an ANOVA following [Blakeslee et al. (2021)](https://doi.org/10.1098/rspb.2021.0703). I went back and used a standard GLM to look at the influence of various demographic variables, temperature, experimental day, and temperature x day on log-transformed averaged TTR for each individual crab:

```
TTRmodel <- glm(log(TTRavg) ~ treatment + day + treatment*day + as.factor(sex) + as.factor(integument.color) + carapace.width + weight + as.factor(missing.legs), data = modTTR) #GLM, with log(TTRavg) as the response variable, and treatment, day, the interaction between treatment and day, sex, integument color, carapace width, weight, and whether or not a crab is missing legs as explanatory variables.
summary(TTRmodel) #5C different than control, 30C marg different than control
```

I also used carapace width instaed of size (carapace width/carapace length), since Carolyn mentioned that carapace width is standard for crab studies! My initial GLM showed that TTR was different between 5ºC and 13ºC, and marginally different between 30ºC and 13ºC. I then used a backwards deletion approach with AIC for model selection:

```
step(TTRmodel, direction = "backward") #Use a backwards deletion approach and AIC to identify best fit model
```

The most parsimonious model included treatment, day, and weight. I ran it separately to assess the effects of these variables:

```
TTRmodel2 <- glm(log(TTRavg) ~ treatment + day + weight, data = modTTR) #Run the best fit model
summary(TTRmodel2) #Both treatments are significantly different from the control. Day and weight are not significant
```

Again, there is a significant difference between 5ºC and 13ºC. TTR is also significantly different between 30ºC and 13ºC. Even though day and weight are included in the model, they do not have a significant influence on TTR. I know sometimes people report the best fit model with non-significant terms because it does have the lowest AIC, while other times people consider the best fit model to have only significant terms. I decided to incluce the non-significant terms only because they do explain variation in TTR, meaning that the final model does not overstate the importance of temperature. I saved the model output [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/03-TTR-analysis/TTR-glm-output.csv).

Carolyn also suggested that a boxplot may better capture variation in TTR than my dot plot with SE bars does, so I made a boxplot. This involved recoding some data as a character format:

```
modTTR %>%
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
  geom_hline(aes(yintercept = boxplot.stats(modTTR$TTRavg)$stats[5]), lty = 2, color = "black") +
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
  theme_classic(base_size = 15) #Select unique day, treatment, and TTRavg data. Replace numbers with characters to facilitate better ordering. Plot average TTR for each crab in a boxplot. Do not show outliers with the OG boxplot, but add them in with geom_jitter. Add horizontal line for boxplot outlier threshold. Assign colors to each treatment. Increase base font size.
ggsave("figures/time-to-right-avg-boxplot.pdf", height = 8.5, width = 11)
```

<img width="1135" alt="Screenshot 2024-01-17 at 5 53 09 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/f5bf3e2d-e733-4622-b68e-7471367504a0">

**Figure 1**. Boxplot of average TTR by treatment and experimental day

I do like the boxplot better! I like how it shows that some crabs can have wildly different TTR than the median. So while they may be statistical outliers, there is certainly no biological reason to remove them, I think. It may be worth figuring out if the crabs that did not right within the 90 s window are the outliers at the top of the graph.

### Respirometry

Less analytical comments on this section! First, I redid the ANOVA using carapace width instead of crab size:

As expected, there was no influence of carapace width on respirometry since they were all about the same size and weight to fit in the chamber. So nothing needed to change downstream.

I also removed the dashed line connecting boxplot medians for each temperature within a day, since Carolyn said it sugggested that the same crabs were exposed to each temperature. That's certainly not the case!

<img width="1135" alt="Screenshot 2024-01-17 at 3 58 17 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/d3a8ee91-b1fd-41e8-b782-267be5def2e7">

**Figure 2**. Boxplot of oxygen consumption by treatment and experimental day

### Going forward

1. Revise demographic analysis
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
