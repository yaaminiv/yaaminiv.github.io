---
layout: post
comments: true
title: CEABiGR Part 15
tags: virginica CEABiGR topGO
---

## Enrichment, enrichment

Last set of analyses for CEABiGR! I know I keep saying that, but it's actually real let's time so don't start with me.

### A quick detour to some figures

In the perfect reenactment of [the meme from this goals post](https://yaaminiv.github.io/December-2023-Goals/) I decided that a really important use of my time would be to combine two transcriptional noise figures into one. In theory this is a good idea and it came about as I was doing a closer read of the manuscript. HOWEVER IT WAS NOT A PRIORITY TASK. And yet, make a figure is what I did.

In [this R Markdown script](https://github.com/sr320/ceabigr/blob/main/code/42-predominant-isoform.Rmd), I mashed eight different plots together. I played around with the final plot size and text size to get everything to fit.

```
#FEMALES

#A. Treatment

femTreatmentPlot <- femMethExpCV %>%
  mutate(., treatment = case_when(treatment == "control" ~ "Control",
                                  treatment == "exposed" ~ "Exposed")) %>%
  ggplot(aes(x = treatment, y = log10(CoV_FPKM))) + geom_violin(aes(fill = treatment)) +   
  scale_fill_manual(values = c(plotColors[4], plotColors[8]),
                    guide = "none") +
  scale_alpha_continuous(guide = "none") +
  geom_boxplot(width = 0.1, colour = "grey20", alpha = 0.2) +
  labs(x = "Treatment", y = "Transcriptional Noise", subtitle = expression(paste("A. ", beta, " = -8.62 x", 10^-3, ", t = -3.17, p = 0.002")), title = "Females") +
  stat_summary(aes(group = 1), fun = median, geom = "line", col = "black", lty = 2) +
  theme_classic(base_size = 12) + theme(plot.margin = margin(t = 5, r = 10, b = 5, l = 0, unit = "pt"),)

#C. FPKM

femFPKMPlot <- femMethExpCV %>%
  ggplot(aes(x = log2(avg_exp + 1), y = log10(CoV_FPKM))) + geom_point(aes(colour = treatment, alpha = 0.5)) +    
  scale_colour_manual(values = c(plotColors[4], plotColors[8]),
                      guide = "none") +
  scale_alpha_continuous(guide = "none") +
  labs(x = "FPKM", y = "", subtitle = expression(paste("C. ", beta, " = -0.10, t = -51.19, p < ", 10^-15)), title = "") +
  geom_smooth(data = (. %>% filter(treatment == "control")), method = lm, colour = plotColors[7], linewidth = 2, na.rm = TRUE) +
  geom_smooth(data = (. %>% filter(treatment == "exposed")), method = lm, colour = plotColors[9], linewidth = 2, na.rm = TRUE) +
  theme_classic(base_size = 12) + theme(plot.margin = margin(t = 5, r = 10, b = 5, l = 0, unit = "pt"),)

#E. Methylation

femMethPlot <- femMethExpCV %>%
  ggplot(aes(x = avg_meth, y = log10(CoV_FPKM))) + geom_point(aes(colour = treatment, alpha = 0.5)) +    
  scale_colour_manual(values = c(plotColors[4], plotColors[8]),
                      guide = "none") +
  scale_alpha_continuous(guide = "none") +
  labs(x = "Methylation (%)", y = "", subtitle = expression(paste("E. ", beta, " = -3.85 x ", 10^-3, ", t = -56.32, p < ", 10^-15)), title = "") +
  geom_smooth(data = (. %>% filter(treatment == "control")), method = lm, colour = plotColors[7], linewidth = 2, na.rm = TRUE) +
  geom_smooth(data = (. %>% filter(treatment == "exposed")), method = lm, colour = plotColors[9], linewidth = 2, na.rm = TRUE) +
  theme_classic(base_size = 12) + theme(plot.margin = margin(t = 5, r = 10, b = 5, l = 0, unit = "pt"),)

#G. CV Methylation

femCVMethPlot <- femMethExpCV %>%
  ggplot(aes(x = log10(CoV_Meth), y = log10(CoV_FPKM))) + geom_point(aes(colour = treatment, alpha = 0.5)) +    
  scale_colour_manual(values = c(plotColors[4], plotColors[8]),
                      guide = "none") +
  scale_alpha_continuous(guide = "none") +
  labs(x = "CV Methylation", y = "", subtitle = expression(paste("G. ", beta, " = 0.10, t = 35.44, p < ", 10^-15)), title = "") +
  geom_smooth(data = (. %>% filter(treatment == "control")), method = lm, colour = plotColors[7], linewidth = 2, na.rm = TRUE) +
  geom_smooth(data = (. %>% filter(treatment == "exposed")), method = lm, colour = plotColors[9], linewidth = 2, na.rm = TRUE) +
  theme_classic(base_size = 12) + theme(plot.margin = margin(t = 5, r = 10, b = 5, l = 0, unit = "pt"),)

#MALES

#B. Treatment

maleTreatmentPlot <- maleMethExpCV %>%
  mutate(., treatment = case_when(treatment == "control" ~ "Control",
                                  treatment == "exposed" ~ "Exposed")) %>%
  ggplot(aes(x = treatment, y = log10(CoV_FPKM))) + geom_violin(aes(fill = treatment)) +   
  scale_fill_manual(values = c(plotColors[4], plotColors[8]),
                    guide = "none") +
  scale_alpha_continuous(guide = "none") +
  geom_boxplot(width = 0.1, colour = "grey20", alpha = 0.2) +
  labs(x = "Treatment", y = "Transcriptional Noise", subtitle = expression(paste("B. ", beta, " = 0.15, t = 7.81, p = 6.19 x ", 10^-15)), title = "Males") +
  stat_summary(aes(group = 1), fun = median, geom = "line", col = "black", lty = 2) +
  theme_classic(base_size = 12) + theme(plot.margin = margin(t = 5, r = 10, b = 5, l = 0, unit = "pt"),)

#D. FPKM

maleFPKMPlot <- maleMethExpCV %>%
  ggplot(aes(x = log2(avg_exp + 1), y = log10(CoV_FPKM))) + geom_point(aes(colour = treatment, alpha = 0.5)) +    
  scale_colour_manual(values = c(plotColors[4], plotColors[8]),
                      guide = "none") +
  scale_alpha_continuous(guide = "none") +
  labs(x = "FPKM", y = "", subtitle = expression(paste("D. ", beta, " = -0.41, t = -47.87, p < ", 10^-15)), title = "") +
  geom_smooth(data = (. %>% filter(treatment == "control")), method = lm, colour = plotColors[7], linewidth = 2, na.rm = TRUE) +
  geom_smooth(data = (. %>% filter(treatment == "exposed")), method = lm, colour = plotColors[9], linewidth = 2, na.rm = TRUE) +
  theme_classic(base_size = 12) + theme(plot.margin = margin(t = 5, r = 10, b = 5, l = 0, unit = "pt"),)

#F. Methylation

maleMethPlot <- maleMethExpCV %>%
  ggplot(aes(x = avg_meth, y = log10(CoV_FPKM))) + geom_point(aes(colour = treatment, alpha = 0.5)) +    
  scale_colour_manual(values = c(plotColors[4], plotColors[8]),
                      guide = "none") +
  scale_alpha_continuous(guide = "none") +
  labs(x = "Methylation (%)", y = "", subtitle = expression(paste("F. ", beta, " = -1.40 x ", 10^-3, ", t = -14.88, p < ", 10^-15)), title = "") +
  geom_smooth(data = (. %>% filter(treatment == "control")), method = lm, colour = plotColors[7], linewidth = 2, na.rm = TRUE) +
  geom_smooth(data = (. %>% filter(treatment == "exposed")), method = lm, colour = plotColors[9], linewidth = 2, na.rm = TRUE) +
  theme_classic(base_size = 12) + theme(plot.margin = margin(t = 5, r = 10, b = 5, l = 0, unit = "pt"),)

#H. CV Methylation

maleCVMethPlot <- maleMethExpCV %>%
  ggplot(aes(x = log10(CoV_Meth), y = log10(CoV_FPKM))) + geom_point(aes(colour = treatment, alpha = 0.5)) +    
  scale_colour_manual(values = c(plotColors[4], plotColors[8]),
                      guide = "none") +
  scale_alpha_continuous(guide = "none") +
  labs(x = "CV Methylation", y = "", subtitle = expression(paste("H. ", beta, " = 0.14, t = 27.46, p = < ", 10^-15)), title = ) +
  geom_smooth(data = (. %>% filter(treatment == "control")), method = lm, colour = plotColors[7], linewidth = 2, na.rm = TRUE) +
  geom_smooth(data = (. %>% filter(treatment == "exposed")), method = lm, colour = plotColors[9], linewidth = 2, na.rm = TRUE) +
  theme_classic(base_size = 12) + theme(plot.margin = margin(t = 5, r = 10, b = 5, l = 0, unit = "pt"),)

(femTreatmentPlot | femFPKMPlot | femMethPlot | femCVMethPlot) / (maleTreatmentPlot | maleFPKMPlot | maleMethPlot | maleCVMethPlot) #Create patchwork plot
ggsave("transcriptional-noise/TN-multipanel.pdf", height = 8.5, width = 15)
```

![Screenshot 2024-03-08 at 4 33 27 PM](https://github.com/sr320/ceabigr/assets/22335838/d4d94f48-d86e-4edc-b2b1-b4ea3ce21f05)

**Figure 1**. Multipanel plot with transcriptional noise results for both sexes

### DML enrichment

Now to the task at hand: enrichment for genes containing DML. I returned to [this R Markdown script](https://github.com/sr320/ceabigr/blob/main/code/Genomic-Location-of-DML.Rmd) (made before my `tidyverse` stan days) and of course, slightly deviated to update the color scheme for a plot.

<img width="1124" alt="Screenshot 2024-03-09 at 12 26 25 PM" src="https://github.com/sr320/ceabigr/assets/22335838/775b118d-c1cf-44ab-809a-70840bfdad52">

**Figure 2**. DML location plot with updated color scheme

I then performed gene enrichment with `topGO` using the same code I've used two other times for this paper. Nothing notable for the methods, except that past Yaamini created gene background files in [this Jupyter notebook](https://github.com/sr320/ceabigr/blob/main/code/Genomic-Location-of-DML.ipynb) that got lost on my local machine and could not be found on `gannet`. So I took a very long detour to download large sample files and remake them. BUT now the enrichment is done and I got to close [this issue](https://github.com/sr320/ceabigr/issues/95) so it doesn't matter! There were 85 enriched BP GO terms in females, and 359 terms in males.

### ASCA

Ariana was able to get the ASCA to work for our exon data, and she identified genes that were alternatively spliced by treatment! The next step in this analysis was to understand how methylation influenced splicing, and if genes that were alternatively spliced were enriched for any biological processes. Steven started to investigate the influence of methylation on alternative splicing in [this script](https://github.com/sr320/ceabigr/blob/main/code/78-asca-methdiff.qmd). There were no DML found in alternatively spliced genes in females, and only one not-spliced gene had a DML. In males, six alternatively spliced genes contained DML while 13 not-spliced genes had DML.

I also wanted to understand how many alternatively spliced genes had changes in the maximum number of transcripts expressed or a shift in the predominant transcript. After thinking about it some more, I decided not to investigate the predominant transcript shifts because by definition, alternatively spliced genes have a change in the predominant transcript. Five female genes and four male genes had a change in the maximum number of transcripts expressed.

So obviously, I did a little detour with some figures. I modified figures Steven made during Science Hour looking at the lack of methylation influence on alternative splicing then closed [this issue](https://github.com/sr320/ceabigr/issues/89).

<img width="1138" alt="Screenshot 2024-03-10 at 4 12 35 AM" src="https://github.com/sr320/ceabigr/assets/22335838/9724c01a-91f9-4340-8dd7-a880911e2ed7">

**Figure 3**. Correlation between average gene methylation in control and exposed samples

Even though I was pretty confident that there wasn't an effect of methylation on alternative splicing, I used a binomial GLM to evaluate if average gene methylation in control or exposed samples was a significant predictor of splicing:

```
femSpliceMethModel <- glm(I(desc == "f_diff") ~ average_control + average_exposed + log10(length),
                          family = binomial(),
                          data = f_allSplice_methLengths) #Create a binomial model, where 1 = success = different splicing by treatment. Use average control methylation, average exposed methylation, and gene length as explanatory variables
summary(femSpliceMethModel) #No significant terms
```

Interestingly, none of the terms, including gene length, were significant in either sex! I didn't want to add change in methylation to the model since a treatment effect was already included in the model (the splicing). I also didn't add gene expression because expression levels were used to identify genes that were alternatively spliced. The output for both models can be found [here](https://github.com/sr320/ceabigr/tree/main/output/78-asca-methdiff).

Finally, to the enrichment. I initially ran the enrichment using the top 20 genes from each PC as a background. This lead to no enriched processes. I wasn't sure if I should have used a list of all genes in the ASCA analysis instead, so I commented on [this issue](). Steven said to use all genes. When I did that, I found 38 enriched processes in females and 26 in males. The enrichment output can be found [here](https://github.com/sr320/ceabigr/tree/main/output/78-asca-methdiff).

And of course, [issue closed](https://github.com/sr320/ceabigr/issues/102).

BUT I opened [this new issue](https://github.com/sr320/ceabigr/issues/105) to remind myself to look for common GO terms across all enrichment tests. I think this will help with the funtional aspect of the discussion. For now, I'm stepping away from R and diving into paper land.

### Going forward

1. Update methods and results
2. Compare with Aranda and Liew models in discussion section
4. Read recent papers
3. Write discussion
4. Write abstract
5. Outline introduction
6. Write introduction
7. Send to co-authors for approval
8. Submit to journal special issue

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
