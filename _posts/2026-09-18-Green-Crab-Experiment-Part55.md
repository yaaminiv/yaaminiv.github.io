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

Obviously the lipidomics dataset is tougher to work with than the metabolomics dataset. I started by modifying the multipanel figure to remove the enrichment data in [my R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/06-lipidomics-analysis.Rmd):

<img width="1410" height="630" alt="Image" src="https://github.com/user-attachments/assets/473ef44a-3422-415a-8055-ea900eaded03" />

**Figure 3**. Simplified PLS-DA and ASCA figure

- Did some creative R wrangling to get enriched terms associated with lipids, then got abundance data for those lipids
- My initial thought was to average lipid data within a LION term and plot, but…no. Each lipids had different baseline abundances, so I couldn't compare lipids with orders of magnitude differences in abundance!
  - Chhaya suggested I calculate percent differences for each individual lipid to be able to plot them on the same scale. This worked really well!
    - For the 13 v 30 comparison, there were lots of lipids with very high abundance in 30
    - For the 13 v 5 comparison, there were some VIP lipids with high abundance, but mainly low abundance at 5ºC. Decent amount of overlap with 13 v 30 VIP too
  - I then went to make a similar plot for the enriched lipids of ASCA P2...BUT THEN REALIZED I HAED TO REDO THE ENRICHMENT USING 50% CUMULATIVE PERCENT DIFFERNCE CUT-OFF FML
      - When I redid the enrichment with the 50% cumulative difference cutoff, there were no more significantly enriched pathways! So I guess that solved itself? The revised output can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/ASCA/LION-enrichment-report-PC2).
  - I made a multi panel figure for percent difference. When I showed it to Ariana, she suggested that I add text to each panel that quantifies the number of lipids with higher abundance at 30ºC, lower abundance at 13ºC, etc.
    - Thankfully for me, I just removed similar lines of code from my [demographic data analysis](https://github.com/yaaminiv/green-crab-metabolomics/commit/77e2e45d5a1582447088308a15f9a0e369a2f909)! I needed to create a vector with the label information (label, x position, y position) and then use `geom_text` to add the information to each facet. It worked splendidly

<img width="651" height="840" alt="Image" src="https://github.com/user-attachments/assets/7082415c-e01a-4843-b42e-9a70f7c23ba2" />

**Figure 4**. Difference in lipid abundance by enriched LION term

- The last thing I needed to do was visualize differences in lipid saturation state. I had previous figures that examined this, so I just needed to modify existing code
  - Before I did that...I wanted to confirm which lipids ended up in the cell membrane! Those are the ones I wanted to visualize to relate to homeoviscous adaptation
  - I found [this very helpful review](https://pmc.ncbi.nlm.nih.gov/articles/PMC2642958/) that had the information I need. Some important notes/snippets (note that the snippets are directly copied from the text!)
    - Glycerophospholipids: phosphatidylcholine (PtdCho), phosphatidylethanolamine (PtdEtn), phosphatidylserine (PtdSer), phosphatidylinositol (PtdIns) and phosphatidic acid (PA)
    - Breakdown products of membrane lipids serve as lipid second messengers. The glycerolipid-derived signalling molecules include lysoPtdCho (LPC), lysoPA (LPA), PA and DAG.
    - The sphingolipids constitute another class of structural lipids. he major sphingolipids in mammalian cells are sphingomyelin (SM) and the glycosphingolipids (GSLs). Sphingolipids have saturated (or trans-unsaturated) tails so are able to form taller and narrower cylinders than PtdCho lipids of the same chain length and pack more tightly, adopting the solid ‘gel’ or so phase
    - The adopted phase depends on lipid structure: long, saturated hydrocarbon chains are found in sphingomyelin (SM), so SM-rich mixtures tend to adopt solid-like phases; unsaturated hydrocarbon chains are found in most biomembrane glycerophospholipids, so these tend to be enriched in liquid phases. Sterols by themselves do not form bilayer phases, but together with a bilayer-forming lipid, the liquid-ordered phase can form. This remarkable phase has the high order of a solid but the high translational mobility of a liquid.
    - ER: main site of lipid synthesis
      - The major glycerophospholipids assembled in the endoplasmic reticulum (ER) are phosphatidylcholine (PtdCho; PC), phosphatidylethanolamine (PtdEtn; PE), phosphatidylinositol (PtdIns; PI), phosphatidylserine (PtdSer; PS) and phosphatidic acid (PA). In addition, the ER synthesizes Cer, galactosylceramide (GalCer), cholesterol and ergosterol. Both the ER and lipid droplets participate in steryl ester and triacylglycerol (TG) synthesis.
    - mitochondrion: synthesizes some lipids
      - Approximately 45% of the phospholipid in mitochondria (mostly PtdEtn, PA and cardiolipin (CL)) is autonomously synthesized by the organelle
  - I decided to include glycerophospholipids and sphingomyelin in my figure based on the information in the paper
  - I modified the figure to facet by saturation stage so I could more easily see which saturation stages had the most changes. I then used color to indicate lipid classes
  - Similar to the other figure, I added text to indicate how many lipids were more or less abundant by saturation stage
  - I smashed these panels with my lipid enrichment panels into one figure to rule them all.

<img width="1645" height="1317" alt="Image" src="https://github.com/user-attachments/assets/4dc5fa1a-450f-4306-af5b-00093d0a4c0d" />

**Figure 5**. Multipanel figure with lipid abundance by saturation state and enriched LION term

### Integration analysis

I wanted to find a way to present the module eigengene expression alongside average TTR or oxygen consumption. I decided to make dot plots similar to [those I made for TTR and OC](https://yaaminiv.github.io/Green-Crab-Experiment-Part53/).

- First, I fixed an installation issue I was having with WGCNA. I couldn't load the package because I was missing `preprocessCore`. I installed it: BiocManager::install(“preprocessCore”) -> finally was able to install WGCNA again
- In [this script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/07-integration-analysis.Rmd) I made dot plots, using `geom_pointrange` to add vertical standard error and `geom_linerange` to add horizontal error. Using `geom_errorbar` and `geom_errorbarh` led to an extremely ugly error bar on my plot that were all different widths. It was gross.

```
mME_all %>% #Take module eigengene information
  filter(., Module == "3" | Module == "6" | Module == "13") %>% #Filter significant modules
  mutate(order = case_when(Module == "3" ~ "1",
                           Module == "6" ~ "2",
                           Module == "13" ~ "4")) %>% #Mutate modules in order
  left_join(x = .,
            y = datTraitsPhysOnly %>%
              rownames_to_column(var = "crab.ID") %>%
              dplyr::select(-`Oxygen Consumption`) %>%
              dplyr::rename(avgTTR = `Average TTR`),
            by = "crab.ID") %>% #Jon with physiology data. Modify physiology data to include a crab.ID column and rename columns
  group_by(treatment, order) %>% #Group by treatment and module
  summarize(moduleAvgValue = mean(value), #Average module eigengene expression
            moduleSEValue = std.error(value), #SE for module eigengene expression
            moduleAvgTTR = mean(avgTTR, na.rm = TRUE), #Avg TTR
            moduleSETTR = std.error(avgTTR, na.rm = TRUE) #TTR SE
  ) %>%
  ggplot(aes(x = moduleAvgTTR, y = moduleAvgValue, colour = treatment, fill = treatment)) + #Create a new plot with TTR on the x and ME values on the y. Color and fill by treatment
  geom_pointrange(aes(ymin = moduleAvgValue - moduleSEValue,
                      ymax = moduleAvgValue + moduleSEValue), #Add ME value limits
                  size = 2,
                  width = 0.2,
                  linewidth = 0.8,
  ) + #Add standard errors to the plot
  geom_linerange(aes(xmin = moduleAvgTTR - moduleSETTR,
                     xmax = moduleAvgTTR + moduleSETTR), #Add TTR value limits
                 size = 2,
                 width = 0.2,
                 linewidth = 0.8,
  ) + #Add means and standard errors to the plot
  geom_hline(yintercept = 0, linetype = "dashed", color = "grey")+ #Add zero line
  facet_wrap(~ order, labeller = labeller(order = module_facets)) + #Facet by module and add facet labels
  scale_x_continuous(name = "Average TTR (s)",
                     breaks = seq(0, 8, by = 1)) + #Modify x-axis
  ylab("Module Expression") + #y axis label
  scale_fill_manual(name = "Temperature (ºC)",
                    values = c(plotColors[3], plotColors[2], plotColors[1]),
                    labels = c("5", "13", "30")) + #Modify scale
  scale_colour_manual(name = "Temperature (ºC)",
                      values = c(plotColors[3], plotColors[2], plotColors[1]),
                      labels = c("5", "13", "30")) + #Modify scale
  theme_classic(base_size = 15) + theme(legend.position = "bottom", #Legend on the bottom
                                        legend.text = element_text(color = "black", size = 12), #Modify legend text size
                                        strip.text.x = element_text(size = 15, color = "black", face="bold"),
                                        strip.background = element_rect(color = "white")) #Modify facet labels
```

- Once I had my individual TTR and OC figures, I modified the multipanel figure on InDesign

<img width="1021" height="877" alt="Image" src="https://github.com/user-attachments/assets/b7330242-7b8a-4a58-91c2-21c79712a748" />

**Figure 6**. Revised integration multipanel figure

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

Those are all of the methods and results comments! Looks like it's time to dig into the interpretation and supplementary material.

### Going forward

1. Address remaining methods comments
2. Address results comments
3. Address figure comments
3. Address introduction comments
4. Address discussion comments
5. Modify supplementary material section
5. Send revised manuscript to co-authors

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
