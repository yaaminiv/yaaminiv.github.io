---
layout: post
comments: true
title: Green Crab Experiment Part 22
tags: green-crab metabolomics WGCNA
---

## Functional metabolomics analysis

I returned to my [R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/05-metabolomics-analysis.Rmd) to [continue my metabolomics analysis](https://yaaminiv.github.io/Green-Crab-Experiment-Part21/). After the PLSDA, Ariana took the known metabolites and ran them through a WGCNA for metabolites (a WCNA if you will) to understand metabolite abundance patterns. The premise is the same: metabolites with similar abundance patterns will be grouped into modules, and I can then take those modules and understand what pathways those metabolites are part of since similar abundance patterns suggest similar functionality in response to temperature and time.

### WCNA

The first step of this workflow is to filter the data for metabolite and sample outliers using `pOverA` filtering. Side note: installing the `genefilter` package was a pain and a half, and involved using `homebrew` to separately install `gfortran` then updating `BioCManager` and forcing installations of `genefilter` and `WGCNA` all over again. Taurine and sulfuric acid seemed like outliers, but I included them in the analysis anyways. There were no sample outliers. Both outlier plots can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/WCNA/figures).

I then ran the WCNA analysis with the following parameters:

networkType = "unsigned"
deepSplit = 2
pamRespectsDendro = F
minModuleSize = 5
maxBlockSize = 4000
reassignThreshold = 0
mergeCutHeight = 0.25

I ended up with nine total modules with six to 40 metabolties.

**Table 1**. Number of metabolites in each module

| **Module** | **Number of Metabolites** |
|:----------:|:-------------------------:|
|      0     |             40            |
|      1     |             22            |
|      2     |             22            |
|      3     |             19            |
|      4     |             15            |
|      5     |             10            |
|      6     |             10            |
|      7     |             8             |
|      8     |             6             |

Once I had the merged module eigengenes, I correlated them to my treatment x day information. I went back and forth on how to do this. At first, I correlated the modules to individual temperatures and days (5ºC, 13ºC, 30ºC, Day 3, and Day 22). But the resulting data was hard to make sense of, and because there were only two conditions for day, I'd have to create a contrast for that anyways. I then decided it made more sense to correlate the modules with treatment x day information (5ºC at Day 3, 5ºC at Day 22, 13ºC at Day 3, 13ºC at Day 22, 30ºC at Day 3, 30ºC at Day 22). I used `ComplexHeatmap` to create a heatmap with correlation information:

```
ht <- Heatmap(moduleTraitCor, name = "Eigengene", column_title = "Module-Group Eigengene Correlation",
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
              #column_order = trait_order,
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
```

<img width="847" alt="Screenshot 2023-12-29 at 3 21 23 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/103b0a3e-87a0-448b-bf6b-718f2be75eee">

**Figure 1**. Heatmap with correlations between modules and treatment x day information.

A couple of things jumped out at me from this heatmap. Firstly, the clustering of the treatments! Both 13ºC treatments and 5ºC at Day 3 were clustered together, which suggest those metabolite profiles were similar. It makes sense that the 13ºC treatments are the same over time. The earliest cold treatment being similar to the control treatments suggests that cold response is not mounted very quickly. On the other hand, the 30ºC treatments clustered together! This suggests the opposite: response to heat stress is quick at the metabolite level. Based on the number of correlations between the modules and treatment x day conditions, it seems like the responses at 5ºC and 30ºC are more pronounced at Day 22.

I then plotted module eigengene expression by treatment x day conditions so I could understand the trends in the heatmap better:

```
mME_meta %>%
  ggplot(aes(x = treatment_day, y = value, colour = treatment_day)) +
  facet_wrap(~ Module) +
  ylab("Module Expression") +
  geom_hline(yintercept = 0, linetype = "dashed", color = "grey")+
  geom_boxplot(width = 0.5, position = position_dodge(width = 0.5), alpha = 0.7) +
  stat_summary(fun=mean, geom="line", aes(group = treatment_day, color = treatment_day), position = position_dodge(width = 0.5))  +
  geom_point(pch = 21, position = position_dodge(width = 0.5)) +
  scale_fill_manual(name = "Temperature (ºC) x Day",
                    values = c("lightblue", "#2171B5", "grey80", "#525252", "salmon", "#CB181D"),
                    labels = c("5ºC on Day 3", "5ºC on Day 22", "13ºC on Day 3", "13ºC on Day 22", "30ºC on Day 3", "30ºC on Day 22")) +
  scale_colour_manual(name = "Temperature (ºC) x Day",
                      values = c("lightblue", "#2171B5", "grey80", "#525252", "salmon", "#CB181D"),
                      labels = c("5ºC on Day 3", "5ºC on Day 22", "13ºC on Day 3", "13ºC on Day 22", "30ºC on Day 3", "30ºC on Day 22")) +
  xlab("") + #Axis titles
  theme_classic(base_size = 15) + theme(axis.text.x = element_text(color = "white"),
                                        axis.ticks.x = element_blank())
```

```
mME_meta %>%
  ggplot(aes(x = treatment_day, y = value)) +
  facet_grid(~Module) +
  ylab("Eigengene Expression") +
  geom_hline(yintercept = 0, linetype ="dashed", color = "grey")+
  geom_point(aes(fill = treatment_day, color = treatment_day), size = 3, pch = 21, position = position_dodge(width = 0.5)) +
  geom_smooth(aes(group=1), se=TRUE, show.legend = NA, color="black")+
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
                                        strip.text.x = element_text(size = 14, color = "black", face="bold"))
```

<img width="1336" alt="Screenshot 2023-12-29 at 3 21 48 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/cbdd275b-ac53-4ecd-a0e4-68eabc55fe6a">

<img width="1352" alt="Screenshot 2023-12-29 at 3 22 15 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/2ad8c163-8e88-475b-9a2e-6121042133ea">

**Figures 2-3**. Module eigengene expression by treatment x day.

I think the boxplot makes more sense for a talk or paper, but the trendline patterns in the dotplot are nice to compare and contrast modules.

### MetaboAnalyst

Now for the GUI part of this workflow. Both Ariana and Shelly used MetaboAnalyst to conduct the functional component of their analyses. Following Ariana's workflow, I'm conducted overrepresentation tests for the metabolties in each module: 1) which kinds of compound classes were in each module? and 2) which KEGG pathways were enriched in each module?

To prep for this analysis, I created lists of the metabolites in each module, along with the PubChem and KEGG IDs:

```
module0_metabolites <- module_df %>%
  dplyr::rename(., Module = colors) %>%
  filter(., Module == 0) %>%
  dplyr::select(., Metabolite) %>%
  left_join(., (rawMetabolomicsData %>% dplyr::rename(Metabolite = BinBase.name)), by = "Metabolite") %>%
  dplyr::select(., Metabolite, PubChem, KEGG) #Take dataframe with modules and metabolites, change colors to Module, and select metabolites for a given module. Keep only the metabolite column. Then join with the raw metabolomics data to get PubChem and KEGG ids.
nrow(module0_metabolites) #40 metabolites
```

#### Main Compound Class

I uploaded the list of metabolite names for each module (generated above) to the [MetaboAnalyst interface](https://www.metaboanalyst.ca/MetaboAnalyst/upload/EnrichUploadView.xhtml):

> Enrichment tests are based on the well-established globaltest to test associations between metabolite sets and the outcome. The algorithm uses a generalized linear model to compute a ‘Q-stat’ for each metabolite set. The Q-stat is calculated as the average of the Q values calculated for the each single metabolites; while the Q value is the squared covariance between the metabolite and the outcome. The globaltest has been shown to exhibit similar or superior performance when tested against several other popular methods.

> Metabolite sets: Unlike transcriptomics which allows comprehensive gene expression profiling, targeted metabolomics usually covers only a small percentage of metabolome (the actual coverage is platform/protocol specific). This means that metabolites (defined in our current pathways or metabolite sets) do not have equal probabilities of being measured in your studies, and the enriched functions are the results from both platform/protocol specific effects and biological perturbations. Since the primary interest is to detect the latter, we highly recommend uploading a reference metabolome containing all measurable metabolites from your platform to eliminate the former effects.

I don't have a reference metabolome and I didn't perform targeted metabolomics. I used metabolite sets containing at least two entries and all compounds in the selected library as a reference.

Certain compounds were not recognized by MetaboAnalyst. For these compounds, I clicked "View", then manually checked the box if the correct PubChem ID or KEGG ID was listed. If not, then that compound was not included in the analysis. The resulting name maps (input names v.s names used for enrichment analysis) from MetaboAnalyst were renamed according to module number and saved [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/metaboanalyst/name-maps).

The output tables (msea_ora_result.csv files from MetaboAnalyst) were renamed according to module number and saved [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/metaboanalyst/msea-compoundClass/enrichment-output). I then uploaded the enrichment output and created a table with all compound class enrichment information:

```
#Import dataframes from each module. Add a Module column and rename the first column.

module0_class <- read.csv("metaboanalyst/msea-compoundClass/enrichment-output/module0-msea_ora_result.csv") %>%
  mutate(., Module = "0") %>%
  dplyr::rename(., compoundClass = X)
module1_class <- read.csv("metaboanalyst/msea-compoundClass/enrichment-output/module1-msea_ora_result.csv") %>%
  mutate(., Module = "1") %>%
  dplyr::rename(., compoundClass = X)
module2_class <- read.csv("metaboanalyst/msea-compoundClass/enrichment-output/module2-msea_ora_result.csv") %>%
  mutate(., Module = "2") %>%
  dplyr::rename(., compoundClass = X)
module3_class <- read.csv("metaboanalyst/msea-compoundClass/enrichment-output/module3-msea_ora_result.csv") %>%
  mutate(., Module = "3") %>%
  dplyr::rename(., compoundClass = X)
module4_class <- read.csv("metaboanalyst/msea-compoundClass/enrichment-output/module4-msea_ora_result.csv") %>%
  mutate(., Module = "4") %>%
  dplyr::rename(., compoundClass = X)
module5_class <- read.csv("metaboanalyst/msea-compoundClass/enrichment-output/module5-msea_ora_result.csv") %>%
  mutate(., Module = "5") %>%
  dplyr::rename(., compoundClass = X)
module6_class <- read.csv("metaboanalyst/msea-compoundClass/enrichment-output/module6-msea_ora_result.csv") %>%
  mutate(., Module = "6") %>%
  dplyr::rename(., compoundClass = X)
module7_class <- read.csv("metaboanalyst/msea-compoundClass/enrichment-output/module7-msea_ora_result.csv") %>%
  mutate(., Module = "7") %>%
  dplyr::rename(., compoundClass = X)
module8_class <- read.csv("metaboanalyst/msea-compoundClass/enrichment-output/module8-msea_ora_result.csv") %>%
  mutate(., Module = "8") %>%
  dplyr::rename(., compoundClass = X)

compound_classes <- rbind(module0_class,
                          module1_class,
                          module2_class,
                          module3_class,
                          module4_class,
                          module5_class,
                          module6_class,
                          module7_class,
                          module8_class) #Create dataframe with compound classes
```

The output table with all compound class information for all modules can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/msea-compoundClass/all-module-compound-classes.csv), with enriched compound classes (FDR < 0.05) saved [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/msea-compoundClass/significant-compound-classes.csv). I then plotted the significant compound class information:

```
ggplot(sig_classes_comp, aes(x = Module, y = compoundClass)) +
  geom_point(aes(size = hits)) +
  scale_size(name = "Metabolites",
             range = c(4,15),
             limits = c(1,15)) +
  xlab("Module Eigengene") + ylab("Compound Class") +
  theme_classic(base_size = 15) #Plot significantly enriched compound classes for each module. Scale point size by the number of metabolite hits in each category. Define x- and y- axis labels. Change the angle of the x-axis text.
```

<img width="1337" alt="Screenshot 2023-12-29 at 3 23 29 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/085701ed-9e25-4971-bd47-02589d2d22e8">

**Figure 4**. Enriched compound classes in each module

It seems like every module had an enriched compound class, and I can use that information to understand what each module does in response to temperature and time! I still don't understand too much about compound classes, but Google can help with that.

#### KEGG Pathways

I then did a very similar thing with KEGG pathways! I used the same upload tool, but ran into an error trying to understand KEGG pathway enrichment for modules 6 and 8. MetaboAnalyst has their own help forum, so I posted an issue [here](https://omicsforum.ca/t/unknown-error-occurred-for-kegg-pathway-analysis/3439). I proceeded with enrichment for the remaining modules. The list of KEGG pathways in each module can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/msea-KEGG/all-module-KEGG-pathways.csv), and the significant pathways found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/msea-KEGG/significant-compound-classes.csv).

<img width="1337" alt="Screenshot 2023-12-29 at 3 23 39 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/ba8ccc88-99d0-4245-b07e-740635af191b">

**Figure 5**. Enriched KEGG pathways in each module

Interestingly, only module 1 had enriched KEGG pathways, both related to macronutrient processing and energy. However, module 1 was not significantly correlated with any treatment! This suggests to me that module 1 is related to general maintenance at each temperature, and crabs had no shortage of food. Since there aren't any other modules with significantly enriched KEGG pathways, I'll just use broader pathway information to understand what might be affected at each temperature.

### Going forward

1. Analyze lipidomics data using the same workflow
3. Make SICB talk
4. Update methods
5. Update results
6. Integrate metabolomics and lipidomics data

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
