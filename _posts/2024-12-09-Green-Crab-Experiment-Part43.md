---
layout: post
comments: true
title: Green Crab Experiment Part 43
tags: green-crab TTR metabolomics lipidomics WCNA
---

## Interpreting metabolomic and lipidomic WCNA correlations

I've been trying to figure out what to do with [my metabolite and lipid module correlations](https://yaaminiv.github.io/Green-Crab-Experiment-Part39/) for a while now and I haven't settled on anything that seems useful. Today I followed a suggestion for Ariana to try and understand if there's a specific temperature condition driving correlations.

The idea was to repeat the Pearson's correlation coefficient calculation process using data only from a specific temperature. Then, I could plot all of the correlations together to see if there was a specific temperature driving the overall correlation pattern.

For each temperature, I recalculated the correlations:

```
metabLipidCor13 <- correlationInput %>%
  mutate(., treatment = gsub(pattern = " 13", replacement = "13", x = treatment)) %>%
  filter(., treatment == "13") %>%
  dplyr::select(., -c(1:4)) %>%
  cor(., method = "pearson") %>%
  as.data.frame(.) %>%
  dplyr::select(., starts_with("lipid_")) %>%
  filter(., row_number() %in% 1:7) %>%
  filter(., rownames(.) != "metabolite_0") %>%
  dplyr::select(-lipid_0) %>%
  as.matrix(.) #Take correlation input and filter data for 13ºC. Remove metadata columns and calculate Pearson's correlation coefficients. Convert to dataframe. Keep correlations where lipids are correlated with metabolites. Remove "0" modules. Convert back to a matrix

nSamples13 <- nrow(metabolomicsMetadata %>% filter(., treatment == " 13")) #Count the number of samples at 13ºC
metabLipidPvalue13 <- corPvalueStudent(metabLipidCor13, nSamples13) #Calculate p-values for metabolite-lipid module correlations
head(metabLipidPvalue13 %>% as.data.frame()) #Confirm output
```

All the correlation and p-value dataframes can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/07-integration-analysis). I then proceeded to write a monster chunk of `dplyr code` (s/o to ChatGPT for helping me with the regex syntax for that final `pivot_longer`):

```
metabLipidCorNo0 %>%
  rownames_to_column(., var = "metabolite") %>%
  pivot_longer(cols = 2:13, names_to = "lipid", values_to = "correlation_overall") %>%
  left_join(.,
            (metabLipidPvalueNo0 %>%
               rownames_to_column(., var = "metabolite") %>%
               pivot_longer(cols = 2:13, names_to = "lipid", values_to = "pvalue_overall")),
            by = c("metabolite", "lipid")) %>%
  filter(., pvalue_overall < 0.05) %>%
  left_join(.,
            (metabLipidCor13 %>%
               as.data.frame(.) %>%
               rownames_to_column(., var = "metabolite") %>%
               pivot_longer(cols = 2:13, names_to = "lipid", values_to = "correlation_13") %>%
               left_join(.,
                         (metabLipidPvalue13 %>%
                            as.data.frame(.) %>%
                            rownames_to_column(., var = "metabolite") %>%
                            pivot_longer(cols = 2:13, names_to = "lipid", values_to = "pvalue_13")),
                         by = c("metabolite", "lipid"))),
            by = c("metabolite", "lipid")) %>%
  left_join(.,
            (metabLipidCor30 %>%
               as.data.frame(.) %>%
               rownames_to_column(., var = "metabolite") %>%
               pivot_longer(cols = 2:13, names_to = "lipid", values_to = "correlation_30") %>%
               left_join(.,
                         (metabLipidPvalue30 %>%
                            as.data.frame(.) %>%
                            rownames_to_column(., var = "metabolite") %>%
                            pivot_longer(cols = 2:13, names_to = "lipid", values_to = "pvalue_30")),
                         by = c("metabolite", "lipid"))),
            by = c("metabolite", "lipid")) %>%
  left_join(.,
            (metabLipidCor5 %>%
               as.data.frame(.) %>%
               rownames_to_column(., var = "metabolite") %>%
               pivot_longer(cols = 2:13, names_to = "lipid", values_to = "correlation_05") %>%
               left_join(.,
                         (metabLipidPvalue5 %>%
                            as.data.frame(.) %>%
                            rownames_to_column(., var = "metabolite") %>%
                            pivot_longer(cols = 2:13, names_to = "lipid", values_to = "pvalue_05")),
                         by = c("metabolite", "lipid"))),
            by = c("metabolite", "lipid")) %>%
  pivot_longer(.,
               cols = 3:10,
               names_to = c(".value", "condition"),
               names_pattern = "(.*)_(.*)") %>%
  ggplot(., aes(x = condition, y = correlation, color = condition, shape = condition)) +
  geom_point(size = 3) +
  facet_grid(metabolite ~ lipid, scales = "free_y") +
  labs(y = "Pearson's Correlation Coefficient") +
  scale_color_manual(name = "Condition",
                     values = c(rev(plotColors), "black"),
                     labels = c("5ºC", "13ºC", "30ºC", "Overall")) +
  scale_shape_manual(values = c(15, 19, 17, 3),
                     name = "Condition",
                     labels = c("5ºC", "13ºC", "30ºC", "Overall")) +
  theme_classic(base_size = 15) + theme(axis.title.x = element_blank(),
                                        axis.ticks.x = element_blank(),
                                        axis.text.x = element_blank(),
                                        strip.background = element_rect(colour = "white"))
```

So now I have this...plot?

<img width="1140" alt="Screenshot 2024-12-09 at 9 59 09 PM" src="https://github.com/user-attachments/assets/6ddce609-ea17-4650-8dd9-04aa747951b1">

**Figure 1**. Correlations for each signiciant metabolite-lipid module.

There are some modules where the individual temperature correlations are close to the overall, and some where there are different. But it doesn't really give me a lot of useful information. However, during this process I thought a bit more about the purpose of this analysis. I want to understand combined metabolite-lipid pathways that may be influencing temperature acclimation. I think at most, this WCNA integration shows me that there are two different patterns that are prominent across several modules: positive correlation or negative correlation. Perhaps a more useful next step would be to use a DIABLO analysis to identify correlations between individual metabolites and lipids, but I could split it up into VIP from the general positive correlation chunk and VIP from the general negative correlation chunk. OR, just toss in all my VIP into one analysis and just see if that analysis pulls out patterns similar to what I see in the WCNA correlation heatmap.

In any case, I think the individual correlations are the way to go to get anything meaningful from this analysis.

### Going forward

1. Integrate physiology and -omics data with WCNA
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
