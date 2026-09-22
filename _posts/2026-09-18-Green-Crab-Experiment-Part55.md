---
layout: post
comments: true
title: Green Crab Experiment Part 55
tags: green-crab glmm
editor_options:
  markdown:
    wrap: 72
---

## Remaining analytical updates

One overarching comment I got from reviewers was that in the discussion, I discuss trends that aren't visualized in the main text! While there is some information in the supplemental figures, it is not clear. I am tweaking the figures such that everything that is discussed in the discussion section is present and clear in the results and figures.

### Metabolomics figures

All metabolomics work was done in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/05-metabolomics-analysis.Rmd) or in [this InDesign file](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metabolomics-multipanel.indd).

I did the following for the metabolomics figures and analyses:

- Removed the enrichment ratio panels from Figure 5. One reviewer rightly pointed out that they don't add much, and the bulk of the figure shows pathways that are represented but not significantly enriched
- Decided to repeat the ASCA analysis, but this time use the metabolites that contributed to 50% of the cumulative variance within the PC instead of just choosing the top 20 molecules
  - Didn't change the results, which is nice! It's also good to have a more programmatic solution
  - Output can be found in [this folder](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/ASCA)
  - Seems like MetaboAnalyst got an update! Also, I used hypergeometric tests with relative-betweenness centrality. I will add this information to the methods.
- Created a new figure to show the abundances of metabolites in enriched pathways or pathways of interest.
  - I have BCAA biosynthesis/catabolism (significantly enriched for ASCA-derived PC 1 and 13ºC vs. 30ºC at day 22), arginine biosynthesis (ASCA-derived PC 4), and phenylalanine, tyrosine, and tryptophan biosynthesis (ASCA-derived PC 4)
  - Also added lactic acid, and the three VIP found in the citric acid cycle (alpha-ketoglutarate, malic acid, and fumaric acid). I'm keeping the big pathway diagram with abundances in the supplement so it is clear how these molecules interact with eachother.
  - One thing that was helpful to make this figure was learning how to modify plots that are stiched together with `patchwork`
  - I also learned how to add a y-axis label across multiple panels using a `textGrob` with `grid`
  - Used the alpha symbol to shorten some facet labels by adding in the unicode for alpha ("\u03B1-ketoglutarate"), then saving the figure as a `cairo_pdf` to retain the unicode conversion

```
enrichmentFigure <-(valineBiosynthesisFigure /
                    arginineBiosynthesisFigure /
                    phenylalanineBiosynthesisFigure /
                    streamlinedTCAFigure) + #Combine figures vertically
  plot_layout(guides = "collect") & #Collect identical legends
  theme(legend.position = "bottom", #Have legend on the bottom
        legend.key.width = unit(0.05, "cm")) & #Reduce legend spacing
  guides(
    fill = guide_legend(nrow = 1), #Force legend to be on one row
    color = guide_legend(nrow = 1) #Force legend to be on one row
  )

y_axis_label <- textGrob("Metabolite Abundance", rot = 90, gp = gpar(fontsize = 15))
enrichmentFigureWithLabel <- wrap_elements(y_axis_label) + enrichmentFigure + plot_layout(widths = c(1, 40))

ggsave("ASCA/figures/enriched-pathways-metabolite-abundance.pdf", width = 9, height = 11, device = cairo_pdf) #Save figure
```
<img width="1378" height="517" alt="Image" src="https://github.com/user-attachments/assets/506b7870-6a21-4987-b47a-d7ea8b467353" />

**Figure 1**. Updated PLSDA and ASCA plot

<img width="1105" height="1343" alt="Image" src="https://github.com/user-attachments/assets/aac2aa07-8d7c-46d2-bc03-3e3e43364b5f" />

**Figure 2**. New figure showing metabolite abundance

I did the following within the text of the manuscript:

- Removed any mention of calcium signaling. I don't discuss calcium signaling molecules in the discussion, so it seemed weird to point out which metabolites were involved in calcium signaling.
- I'm not sure what to do with cold protection molecules, but that section may also need to be cut.
- Cleaned up flow and made sure that the correct figures were being referenced in each section.

### Lipidomics figures

### Integration analysis

### Demographic analysis

I was also asked to list and check assumptions for the demographic analysis. Very fair. I thought this would be a quick fix, but it was not! Here's what I did:

- Temperature analysis
  - Assumptions checked: homogeneity of variance, normality of residuals
  - Violated assumption of equal variances, switched to a Welch's test
  - Log transformed temperature to meet assumptions of normality and homoscedasticity
  - As expected, there were significant differences between temperatures.
- Demographic analysis
  - Modified the analysis to examine temperature, day, and their interaction simultaneously for each variable.
  - Once again I encountered the pesky issue of non-independence in the data. I used Gemini to help figure out the best models to use for the data analysis with tank as a random effect
  - Weight and carapace width: Linear mixed effects models
    - Log transformed weight to meet assumptions of normality and homoscedasticity
    - Assumptions checked: normality of residuals
  - Integument color: Cumulative link mixed effects model
    - This model accounts for the ordinality of the data (B goes to BG, BG goes to G, etc.) in a more robust way than the numeric recode I did for other models
    - Assumptions checked: Proportional odds, normality of surrogate residuals
    - As expected, this was the only variable that was significantly impacted by temperature, time, and their interaction
  - Sex ratio: Binomial generalized linear mixed effects model
    - Assumptions checked: Normality and dispersion of simulated residuals

All my assumptions were met! Yay! I also modified the figure to remove any statistical information.

<img width="1077" height="817" alt="Image" src="https://github.com/user-attachments/assets/04e276b6-e0e4-471f-a686-22e0c3ae0e09" />

**Figure**. Demographic data without statistical information.

### Going forward

1. Address remaining methods comments
2. Address results comments
3. Address figure comments
3. Address introduction comments
4. Address discussion comments
5. Modify supplementary material section
5. Send revised manuscript to co-authors

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
