---
layout: post
comments: true
title: Green Crab Experiment Part 29
tags: green-crab metabolomics
---

## Refining a priori metabolomics analysis

[Last time](https://yaaminiv.github.io/Green-Crab-Experiment-Part28/) I worked on this analysis I pulled out metabolites of interest involved in aerobic respiration, anaerobic respiration, and calcium signaling for figures. I'm going to review these figures and add additional information in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/05-metabolomics-analysis.Rmd).

### Respiration

I used ChatGPT to give me lists of metabolites involved in glycolysis, anaerobic respiration, the TCA cycle. To organize them into pathway order, I cross-referenced my list of metabolites with pathway diagrams. That's when I realized I had some extra metabolites in the ChatGPT list that may be byproducts of glycolysis, but not part of the canonical pathway! I removed these metabolites and was left with the following for glycolysis:

1. Glucose: Glucose is the starting substrate for glycolysis. It is phosphorylated to form glucose-6-phosphate by hexokinase or glucokinase.
2. Glucose-1-phosphate: Glucose-1-phosphate is an intermediate in the breakdown of glycogen, which can be converted into glucose-6-phosphate before entering glycolysis.
3. Glucose-6-phosphate: It is the first intermediate in glycolysis, formed from glucose by hexokinase. It is subsequently converted into fructose-6-phosphate.
4. Fructose-6-phosphate: It is an intermediate in glycolysis, formed from glucose-6-phosphate by phosphoglucose isomerase. It is subsequently phosphorylated to fructose-1,6-bisphosphate.
5. Fructose-1-phosphate: Fructose-1-phosphate is an intermediate in fructose metabolism. It is converted into glyceraldehyde-3-phosphate (an intermediate in glycolysis) and dihydroxyacetone phosphate by aldolase.
6. 3-Phosphoglycerate: It is an intermediate in glycolysis, formed from 1,3-bisphosphoglycerate by phosphoglycerate kinase. It is subsequently converted into 2-phosphoglycerate.

I then needed my plot to reflect this order. I created a set of facet labels that matched numerical order with a metabolite. Then, I added a new column in my dataset with this order, and faceted the plot by this new column.

```
glycolysis_facets <- c("1" = "glucose",
                       "2" = "glucose-1-phosphate",
                       "3" = "glucose-6-phosphate",
                       "4" = "fructose-6-phosphate",
                       "5" = "fructose-1-phosphate",
                       "6" = "3-phosphoglycerate") #Assign labels for facets
```

```
transMetabData %>%
  dplyr::select(., c(1:4,
                     `glucose `, `glucose-1-phosphate`, `glucose-6-phosphate`, `fructose-6-phosphate `, `fructose-1-phosphate`, `3-phosphoglycerate`)) %>%
  pivot_longer(., cols = c(5:10), names_to = "metabolite", values_to = "value") %>%
  mutate(., treatment_day = gsub(pattern = "_3", replacement = "_03", x = treatment_day)) %>%
  mutate(., treatment_day = gsub(pattern = "5_", replacement = "05_", x = treatment_day)) %>%
  mutate(., order = case_when(metabolite == "glucose " ~ 1,
                              metabolite == "glucose-1-phosphate" ~ 2,
                              metabolite == "glucose-6-phosphate" ~ 3,
                              metabolite == "fructose-6-phosphate " ~ 4,
                              metabolite == "fructose-1-phosphate" ~ 5,
                              metabolite == "3-phosphoglycerate" ~ 6)) %>%
  ggplot(., mapping = aes(x = treatment_day, y = value, color = treatment_day)) +
  facet_wrap(~order, scales = "free_y", labeller = labeller(order = glycolysis_facets)) +
  ylab("Metabolite Abundance") +
  geom_boxplot(width = 0.5, position = position_dodge(width = 0.5), alpha = 0.7) +
  geom_point(pch = 21, position = position_dodge(width = 0.5)) +
  scale_fill_manual(name = "Temperature (ºC) x Day",
                    values = c("lightblue", "#2171B5", "grey80", "#525252", "salmon", "#CB181D"),
                    labels = c("5ºC on Day 3", "5ºC on Day 22", "13ºC on Day 3", "13ºC on Day 22", "30ºC on Day 3", "30ºC on Day 22")) +
  scale_colour_manual(name = "Temperature (ºC) x Day",
                      values = c("lightblue", "#2171B5", "grey80", "#525252", "salmon", "#CB181D"),
                      labels = c("5ºC on Day 3", "5ºC on Day 22", "13ºC on Day 3", "13ºC on Day 22", "30ºC on Day 3", "30ºC on Day 22")) +
  xlab("") + #Axis titles
  theme_classic(base_size = 15) + theme(axis.text.x = element_blank(),
                                        axis.ticks.x = element_blank(),
                                        legend.position = "bottom",
                                        legend.text = element_text(color = "black", size = 12),
                                        strip.text.x = element_text(size = 10, color = "black", face="bold"))
```

<img width="1146" alt="Screenshot 2024-08-26 at 12 47 57 PM" src="https://github.com/user-attachments/assets/ec4b303b-2d41-4175-b2da-67cf26df3b78">

**Figure 1**. Metabolites involved in glycolysis

I made a similar plot for lactic acid fermentation and for the citric acid cycle! The pyruvate from glycolysis is used in the citric acid cycle if there is oxygen, or in lactic acid fermentation for anaerobic respiration. So the anaerobic respiration plot the same thing as the glycolysis plot, but with an extra panel.

<img width="1146" alt="Screenshot 2024-08-26 at 12 54 01 PM" src="https://github.com/user-attachments/assets/a971478d-fc14-48dc-b406-62f0955f9df7">

**Figure 2**. Metabolites involved in lactic acid fermentation

<img width="1146" alt="Screenshot 2024-08-26 at 12 51 27 PM" src="https://github.com/user-attachments/assets/f8d59e97-c3fa-4ede-9515-150236fb9f0a">

**Figure 3**. Metabolites involved in the citric acid cycle

My next step was to create a pathway diagram in [this InDesign document](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/figures/combined-respiration-plot.indd). Apart from changing the layout, I wanted to see if any of the metabolites in this plot were significant VIP. To do this, I filtered metabolites of interest from my t-test output with [`%in%`]():

```
respirationMetabolites <- c("glucose ", "glucose-1-phosphate", "glucose-6-phosphate", "fructose-6-phosphate ", "fructose-1-phosphate", "3-phosphoglycerate",
                            "lactic acid",
                            "citric acid", "alpha-ketoglutarate", "fumaric acid", "malic acid") #Make a vector with metabolites of interest
```

```
VIP_respirationMetabolites <- rbind(t.test_13v30_day3 %>%
                                      mutate(., comparison = "13v30_day3") %>%
                                      filter(., p.adj < 0.05) %>%
                                      filter(., metabolite %in% respirationMetabolites),
                                    t.test_13v5_day3 %>%
                                      mutate(., comparison = "13v5_day3") %>%
                                      filter(., p.adj < 0.05) %>%
                                      filter(., metabolite %in% respirationMetabolites),
                                    t.test_13v30_day22 %>%
                                      mutate(., comparison = "13v30_day22") %>%
                                      filter(., p.adj < 0.05) %>%
                                      filter(., metabolite %in% respirationMetabolites),
                                    t.test_13v5_day22 %>%
                                      mutate(., comparison = "13v5_day22") %>%
                                      filter(., p.adj < 0.05) %>%
                                      filter(., metabolite %in% respirationMetabolites),
                                    t.test_5_day3v22 %>%
                                      mutate(., comparison = "5_day3v22") %>%
                                      filter(., p.adj < 0.05) %>%
                                      filter(., metabolite %in% respirationMetabolites),
                                    t.test_13_day3v22 %>%
                                      mutate(., comparison = "13_day3v22") %>%
                                      filter(., p.adj < 0.05) %>%
                                      filter(., metabolite %in% respirationMetabolites),
                                    t.test_30_day3v22 %>%
                                      mutate(., comparison = "30_day3v22") %>%
                                      filter(., p.adj < 0.05) %>%
                                      filter(., metabolite %in% respirationMetabolites))
```

The output is saved in [this file](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/combined-respiration-VIP.txt). Interestingly, only three metabolites were identified as pairwise VIP for any contrast, and all of them were involved in the citric acid cycle!

<img width="1136" alt="Screenshot 2024-08-26 at 1 13 36 PM" src="https://github.com/user-attachments/assets/3a87c5d8-f7a7-474c-acd8-267b0b52a0fd">

**Figure 4**. Combined respiration pathway diagram with significance information for VIPs

### Calcium signaling

Finally, I wanted to review the calcium signaling plot. There are many calcium signaling pathways, so it doesn't make sense to organize the plot into a diagram the way I did with respiration. However, I pulled out metabolites that were significant pairwise VIP (table saved [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/calcium-signaling-VIP.txt)) and used that information to annotate the plot in [this InDesign document](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/figures/calcium-signaling-plot-annot.indd).

<img width="1147" alt="Screenshot 2024-08-26 at 10 43 28 AM" src="https://github.com/user-attachments/assets/e51864d7-38d3-4a44-85b9-72785fe3aaa7">

**Figure 5**. Metabolites involved in calcium signaling

### Going forward

1. Revise metabolomics enrichment analyses
2. Repeat lipidomics WCNA with temperature and day separately
3. Repeat lipidomics WCNA with all lipid data
7. Try lipid-specific enrichment with LION
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
