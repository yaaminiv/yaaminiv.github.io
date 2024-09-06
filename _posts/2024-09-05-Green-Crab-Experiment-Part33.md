---
layout: post
comments: true
title: Green Crab Experiment Part 33
tags: green-crab lipidomics WGCNA
---

## Lipidomics WCNA

I [previously determined](https://yaaminiv.github.io/Green-Crab-Experiment-Part32/) that temperature was the only factor significantly altering lipid profiles in my samples. Based on this information, I needed to fix my WCNA.

## Data formatting

The first thing I did in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/06-lipidomics-analysis.Rmd) was reformat my data. I needed samples in columns and the 415 known lipids in rows:

```
knownTransLipidData %>%
  dplyr::select(-c(treatment:treatment)) %>%
  column_to_rownames(var = "crab.ID") %>%
  t(.) %>% as.data.frame(.) %>%
  log(.) %>%
  mutate_if(is.numeric, list(~na_if(., -Inf))) %>%
  replace(is.na(.), 0) #Take transposed lipid data for known lipids. Remove metadata and set crab ID as row names. Transpose and convert into a dataframe such that samples are columns and lipids are rows. Log-transform data. Convert -Inf to NA, then NA to 0.
```

I then performed `pOverA` filtering. I used a `pOverA` of 0.17. This is because I have have 69 samples with a minimum of n = 12 samples per temperature. Since I expect different lipids by temperature (s/o to the PLS-DA), I will accept lipids present in 12/69 = 0.17 of the samples. I also set the minimum value of lipids to 0.01 (median normalized), such that 17% of the samples must have a non-zero normalized metabolite presence in order for the metabolite to remain in the data set. With this filtering, I could keep all 415 known lipid features.

The last formatting step was to check for outliers. I created a clustering dendogram to visually inspect for outliers for lipids:

```
sampleTree <- hclust(dist(rowwiseLipidData), method = "average") #Perform clustering analysis

plot(sampleTree, main = "Sample clustering to detect outliers", sub = "", xlab = "", cex.lab = 0.8, cex.axis = 0.8, cex.main = 2) #Plot cluster dendogram
```

<img width="1090" alt="Screenshot 2024-09-05 at 10 17 53 AM" src="https://github.com/user-attachments/assets/36a31b40-efd2-4196-95bf-dc60886eee4c">

<img width="1090" alt="Screenshot 2024-09-05 at 10 22 44 AM" src="https://github.com/user-attachments/assets/2242f36e-30d3-4fcd-b65a-4391469b9e38">

**Figures 1-2**. Dendograms for lipids with and without potential outlier

There is one lipid, PC 34:1 Isomer A, that looks like an outlier. Even though there are a lot of lipids, it's evident that the clustering patterns don't change with the removal of the potential outlier. Since the outlier doesn't impact the other lipid relationships, I opted to keep it in the data.

I did something similar with the samples:

```
rowwiseLipidData <- t(rowwiseLipidData) %>% as.data.frame(.) #Transpose such that samples are in rows and lipids are in columns.  

sampleTree <- hclust(dist(rowwiseLipidData), method = "average") #Perform clustering analysis

#pdf("WCNA/figures/sample-outliers.pdf", height = 8.5, width = 11)
plot(sampleTree, main = "Sample clustering to detect outliers", sub = "", xlab = "", cex.lab = 1.5, cex.axis = 1.5, cex.main = 2)
#dev.off()
```

<img width="1090" alt="Screenshot 2024-09-05 at 10 18 05 AM" src="https://github.com/user-attachments/assets/0cb9e101-a813-4d42-b279-c9e63c242336">

**Figure 3**. Sample clustering

304, 303, and 606 looked like they could be outliers (all from 30ºC treatment at day 3). I did this analysis with and without those samples and it did not change the sample clustering, so I kept them in the analysis.

### WCNA

With my data formatted, I could proceed to actually running the WCNA. I found that my soft thresholding power did not change at all with the newly formatted data. There were 16 resulting modules. Lipid module assignment can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/06-lipidomics-analysis/WCNA/lipid_modules.csv).

>  0   1   2   3   4   5   6   7   8   9  10  11  12  13  14  15
> 23 136  52  37  25  23  22  20  17  11  10   9   9   8   7   6

After a bunch of data reformatting and correlation calculations, I plotted my module-eigengene correlations.

<img width="718" alt="Screenshot 2024-09-05 at 10 40 27 AM" src="https://github.com/user-attachments/assets/80366d64-88a8-45cc-a148-303c2f6933fa">

**Figure 4**. Module-eigengene correlations

Wow, this is MUCH easier to intepret that the earlier version! Lipid profiles are vastly different at 30ºC than at 5ºC or 13ºC, which matches really well with the PLS-DA. There are some modules that are significantly correlated with 5ºC and seem to have different directionality than 13ºC, so that is an indication of slight differences there. It will be interesting to see if the pairwise 5ºC VIP lipids are in those modules.

### Module eigengene abundance patterns

One of the last thing I did for this part of the analysis was plot module eigengene abundance patterns. I formatted and saved a table of module eigengene abundance along with metadata [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/06-lipidomics-analysis/WCNA/module-expression.csv). I took this table and made a boxplot of eigengene abundance and a dotplot:

```
mME_meta %>%
  ggplot(aes(x = treatment, y = value, colour = treatment)) +
  facet_wrap(~ Module) +
  ylab("Module Expression") +
  geom_hline(yintercept = 0, linetype = "dashed", color = "grey")+
  geom_boxplot(width = 0.5, position = position_dodge(width = 0.5), alpha = 0.7) +
  geom_point(pch = 21, position = position_dodge(width = 0.5)) +
  scale_fill_manual(name = "Temperature (ºC)",
                    values = rev(plotColors),
                    labels = c("5ºC", "13ºC", "30ºC")) +
  scale_colour_manual(name = "Temperature (ºC)",
                      values = rev(plotColors),
                      labels = c("5ºC", "13ºC", "30ºC")) +
  xlab("") + #Axis titles
  theme_classic(base_size = 15) + theme(axis.text.x = element_text(color = "white"),
                                        axis.ticks.x = element_blank())
```

```
mME_meta %>%
  ggplot(aes(x = treatment, y = value)) +
  facet_grid(~Module) +
  ylab("Eigengene Expression") +
  geom_hline(yintercept = 0, linetype ="dashed", color = "grey")+
  geom_point(aes(fill = treatment, color = treatment), size = 3, pch = 21, position = position_dodge(width = 0.5)) +
  geom_smooth(aes(group = 1), se = TRUE, show.legend = NA, color="black") +
  scale_fill_manual(name = "Temperature (ºC)",
                    values = rev(plotColors),
                    labels = c("5ºC", "13ºC", "30ºC")) +
  scale_colour_manual(name = "Temperature (ºC)",
                      values = rev(plotColors),
                      labels = c("5ºC", "13ºC", "30ºC")) +
  xlab("") + #Axis titles
  theme_classic(base_size = 15) + theme(axis.text.x = element_blank(),
                                        axis.ticks.x = element_blank(),
                                        legend.position = "bottom",
                                        legend.text = element_text(color = "black", size = 12),
                                        strip.text.x = element_text(size = 14, color = "black", face="bold"))
```

<img width="1143" alt="Screenshot 2024-09-05 at 10 50 01 AM" src="https://github.com/user-attachments/assets/ac5fc870-3179-4772-9b52-a45440e11ae8">

<img width="1152" alt="Screenshot 2024-09-05 at 10 51 06 AM" src="https://github.com/user-attachments/assets/d647320d-6696-4e36-a6b4-6e98ede93582">

**Figures 5-6**. Boxplot and dotplot of module eigengene abundance for lipids.

I think the temperature-based trends are clearer in the dot plots, but they'll end up being supplementary figures if anything.

I also created a table of relative lipid abundance in relation to 13ºC in case that information is useful, and saved the table [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/06-lipidomics-analysis/WCNA/lipids_relative_change.csv).

### Going forward

1. Lipidomics pathway enrichment
7. Try lipid-specific enrichment with LION
1. Update lipidomics methods
2. Update lipidomics results
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
