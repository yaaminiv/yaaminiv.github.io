---
layout: post
comments: true
title: Green Crab Experiment Part 39
tags: green-crab metabolomics lipidomics
---

## Correlating WCNA modules

Let's get this party started! Based on [my plan](https://yaaminiv.github.io/Green-Crab-Experiment-Part38/), the first step was to integrate the metabolomics and lipidomics WCNA modules. Shoutout to Ariana for telling me how to do this! I created [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/07-integration-analysis.Rmd).

The first thing I did was import my data. I needed the metadata for the metabolomics and lipidomics datasets, the data used to create the WCNA, and the data used to create the module expression matrices. I also imported the data used for the PLSDA for future analysis. I imported the specific objects from my saved RData files:

```
attach("../05-metabolomics-analysis/05-metabolomics-analysis.RData") #Attach metabolomics RData
metabolomicsMetadata <- metabolomicsMetadata #Save metadata
transMetabData <- transMetabData #Save transposed data used for PLSDA
rowwiseMetabData <- rowwiseMetabData #Rowwise data used for WCNA
mME_metabolomics <- mME_meta #Data used to create module expression matrix
detach("file:../05-metabolomics-analysis/05-metabolomics-analysis.RData") #Detach metabolomics RData
```

```
attach("../06-lipidomics-analysis/06-lipidomics-analysis.RData") #Attach lipidomics RData
lipidomicsMetadata <- lipidomicsMetadata #Save metadata
knownTransLipidData <- knownTransLipidData #Save transposed data used for PLSDA
rowwiseLipidData <- rowwiseLipidData #Rowwise data used for WCNA
mME_lipidomics <- mME_meta #Data used to create module expression matrix
detach("file:../06-lipidomics-analysis/06-lipidomics-analysis.RData") #Detach lipidomics RData
```

I then used `left_join` to combine the module expression matrices in a wide format. I needed the module eigengene information to be in a separate column for each sample. Once I had this dataframe, I removed the columns associated with metabolite or lipid WCNA modules that were not significantly correlated with any temperature treatment. I did this because I am interested in understanding patterns of co-expressed metabolites and lipids that are involved in thermal acclimation:

```
correlationInput <- left_join(mME_metabolomics %>%
            pivot_wider(., names_prefix = "metabolite_", names_from = "Module", values_from = "value"),
          mME_lipidomics %>%
            pivot_wider(., names_prefix = "lipid_", names_from = "Module", values_from = "value"),
          by = c("crab.ID")) %>%
  dplyr::select(., -c(treatment.y, day.y, treatment_day.y)) %>%
  dplyr::rename("treatment" = treatment.x, "day" = day.x, "treatment_day" = treatment_day.x) %>%
  dplyr::select(., -c("metabolite_1", "metabolite_8", "lipid_13", "lipid_14", "lipid_15"))
  #Left join modified versions of module information tables from metabolomics and lipidomics data. Remove duplicate metadata information and rename columns to match standard naming conventions. Remove columns that were not significantly correlated to a treatment condition.
```

I then calculated Pearson's correlation coefficients and associated p-values. Since the correlations included metabolites correlated with other metabolites, and lipids correlated with other lipids, I pared down the data to only include only metabolites correlated with lipids:

```
metabLipidCor <- cor(correlationInput[,5:24], method = "pearson") #Calculate Pearson's correlation coefficients

metabLipidCorClean <- as.data.frame(metabLipidCor) %>%
  dplyr::select(., starts_with("lipid_")) %>%
  filter(., row_number() %in% 1:7) #Take correlation data and keep columns where metabolite modules and correlated with lipid modules

nSamples <- nrow(rowwiseMetabData)
metabLipidPvalue <- corPvalueStudent(metabLipidCor, nSamples) #Calculate p-values for metabolite-lipid module correlations

metabLipidPvalueClean <- as.data.frame(metabLipidPvalue) %>%
  dplyr::select(., starts_with("lipid_")) %>%
  filter(., row_number() %in% 1:7) #Take p-values and keep columns where metabolite modules and correlated with lipid modules
```

I saved the [correlation coefficients](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/07-integration-analysis/metabolite-lipid-module-correlations.csv) and [associated p-values](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/07-integration-analysis/metabolite-lipid-module-pvalues.csv) and individual files.

The last step of this preliminary analysis was to create a figure! I created a heatmap very similar to the output used for standard WCNA, where colors are associated with correlation values and the p-values are presented inside each cell:

```
pdf(file = "figures/metabolite-lipid-module-relationship-heatmap.pdf", height = 8.5, width = 8.5)
ht <- Heatmap(as.matrix(metabLipidCorClean), name = "Eigengene", column_title = "Metabolomics-Lipidomics Eigengene Correlation",
              col = blueWhiteRed(50),
              row_names_side = "left",
              row_dend_side = "left",
              width = unit(5, "in"),
              height = unit(4.5, "in"),
              column_dend_reorder = TRUE,
              cluster_columns = col_dend,
              row_dend_reorder = FALSE,  
              #column_split = 3,
              #row_split = 3,
              #column_dend_height = unit(.5, "in"),
              cluster_rows = row_dend,
              #row_gap = unit(2.5, "mm"),
              border = TRUE,
              cell_fun = function(j, i, x, y, w, h, col) {
                if(heatmappval[i, j] < 0.05) {
                  grid.text(sprintf("%s", heatmappval[i, j]), x, y, gp = gpar(fontsize = 10, fontface = "bold"))
                }
                else {
                  grid.text(sprintf("%s", heatmappval[i, j]), x, y, gp = gpar(fontsize = 10, fontface = "plain"))
                }},
              column_names_rot = 45,
              column_names_gp =  gpar(fontsize = 12, border = FALSE),
              column_title_gp = gpar(fontsize = 20),
              row_names_gp = gpar(fontsize = 12, alpha = 0.75, border = FALSE)) #Create a heatmap object
draw(ht) #Draw heatmap
dev.off()
```

![Screenshot 2024-11-04 at 10 44 20 AM](https://github.com/user-attachments/assets/b88dcee7-e597-404d-9daf-99b8143f88da)

**Figure 1**. Correlations between metabolomics and lipidomics WCNA module eigengenes.

Broadly, there seems to be one cluster of metabolomics WCNA modules positively associated with lipid abundance, and another cluster negatively associated with lipid abundance. Ariana pointed out that metabolite_0 and lipid_0 are included in this heatmap. These are the module eigengenes were the WCNA algorithm places metabolites or lipids that aren't co-expressed with any other molecules. I decided to rerun the correlations without these modules.

![Screenshot 2024-11-04 at 11 03 29 AM](https://github.com/user-attachments/assets/dca4c0ea-d33b-4a21-8f75-42f66475c2e9)

**Figure 2**. Correlations between metabolomics and lipidomics WCNA module eigengenes without module 0s.

The overall patterns don't change! Seeing how lipid_0 wasn't significantly correlated with any metabolite modules, and metabolite_0 was only correlated with two lipid modules, I think it's appropriate to remove metabolite_0 and lipid_0 from this analysis.

Now that I have these correlations, the next step is to figure out how to make meaning out of all of it. Here are some potential approaches:

- Identify VIP in each module, and see which correlated modules have the most VIP.
- Get lists of metabolites and lipids in each module so I can understand what is correlated with what
- Create a plot that shows abundances of co-expressed metabolites and lipids for broad patterns

### Going forward

1. Interpret metabolomic and lipidomic WCNA correlations
2. Integrate physiology and -omics data with WCNA
3. Metabolomics and lipidomics DIABLO analysis
3. Address comments on paper draft
4. Outline discussion
5. Outline abstract

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
