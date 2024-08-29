---
layout: post
comments: true
title: Green Crab Experiment Part 30
tags: green-crab metabolomics
---

## (Somewhat) finalized pathway analysis methods

After attending SEB and talking to Ariana, I had a much better idea of how to approach my metabolomics pathway analysis. The goal was to do enough programmatic analytical things so I could have a bit more guidance before digging into various metabolite functions. All analyses were conducted in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/05-metabolomics-analysis.Rmd).

### VIP in modules

My first instinct was to determine which VIP were present in the WCNA modules. I figured I already had metabolites sorted into modules by abundance, so perhaps VIP in the same module would be involved in similar pathways:

```
VIP_module0 <- rbind(t.test_13v30_day22 %>%
                       mutate(., comparison = "13v30_day22") %>%
                       filter(., p.adj < 0.05) %>%
                       filter(., metabolite %in% module0_metabolites$Metabolite),
                     t.test_13v5_day22 %>%
                       mutate(., comparison = "13v5_day22") %>%
                       filter(., p.adj < 0.05) %>%
                       filter(., metabolite %in% module0_metabolites$Metabolite),
                     t.test_5_day3v22 %>%
                       mutate(., comparison = "5_day3v22") %>%
                       filter(., p.adj < 0.05) %>%
                       filter(., metabolite %in% module0_metabolites$Metabolite),
                     t.test_30_day3v22 %>%
                       mutate(., comparison = "30_day3v22") %>%
                       filter(., p.adj < 0.05) %>%
                       filter(., metabolite %in% module0_metabolites$Metabolite)) %>%
  mutate(module = "module0") #Identify VIP present in module 0 across all pairwise comparisons
```

Initially, I thought I would identify VIP from all pairwise comparisons in each module. However, certain modules had significant correlations with various treatment x day conditions. I then decided to only include VIP from pairwise comparisons that reflected these significant correlations:

- Module 0: 5 and 30 at day 22
- Module 2: 5 at day 3 and 30 at day 22
- Module 3: 5 and 30 at day 22
- Module 4: 30 at a day 22
- Module 5: 5 at day 22, 30 at day 22, 30 at day 3
- Module 6: 13 at day 22
- Module 7: 13 and 5 at day 22

Since modules 1 and 8 were not significantly correlated with any treatment x day conditions, I decided not to check them for VIP. The list of VIP in each module can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/all-module-VIP.txt). My next step was to make heatmaps for the VIP in each module:

```
#pdf("metaboanalyst/figures/module0_VIP.pdf", height = 8.5, width = 11)
pheatmap((transMetabData %>%
            dplyr::select(., c(crab.ID, any_of(VIP_module0$metabolite))) %>%
            column_to_rownames(var = "crab.ID") %>%
            t(.) %>% as.data.frame(.) %>% log(.)), cluster_row = TRUE, show_rownames = TRUE, cluster_cols = FALSE, show_colnames = FALSE, color = heatmapColors, annotation_col = metabolomicsAnnotation, annotation_colors = metabolomicsColors, name = "Abundance")
#dev.off()
```

Uh.....so these were raelly messy. I then looked at Ariana's code from her coral metabolomics project and realized she created heatmaps with metabolite information aggregated by life stage. I decided to do something similar by averaging metabolite abundance across each treatment x day condition. Through this process, I learned how to use `any_of` within `dplyr::select` from [this link](https://stackoverflow.com/questions/68749491/dplyr-r-selecting-columns-whose-names-are-in-an-external-vector).

```
#pdf("metaboanalyst/figures/module0_VIP-average.pdf", height = 8.5, width = 11)
pheatmap((transMetabData %>%
            dplyr::select(treatment_day, any_of(VIP_module0$metabolite)) %>%
            group_by(., treatment_day) %>%
            summarize_all(funs(mean(., na.rm = TRUE))) %>%
            column_to_rownames(var = "treatment_day") %>%
            t(.) %>% log(.)), cluster_row = TRUE, show_rownames = TRUE, cluster_cols = TRUE, show_colnames = FALSE, scale = "row", color = heatmapColors, annotation_col = metabolomicsAvgAnnotation, annotation_colors = metabolomicsColors, name = "Abundance")
#dev.off()
```

This looked much better! I dug through Ariana's code more and saw that she plotted z-score in her heatmaps. To do something similar, I log transformed then scaled all values, using `across` with `mutate` (s/o to [this link](https://rebeccabarter.com/blog/2020-07-09-across)) to perform these operations on numerical columns:

```
#pdf("metaboanalyst/figures/module0_VIP-zscore.pdf", height = 8.5, width = 11)
pheatmap((transMetabData %>%
            dplyr::select(treatment_day, any_of(VIP_module0$metabolite)) %>%
            group_by(., treatment_day) %>%
            mutate(across(where(is.numeric), log)) %>%
            mutate(across(where(is.numeric), scale)) %>%
            group_by(., treatment_day) %>%
            summarize_all(funs(mean(., na.rm = TRUE))) %>%
            column_to_rownames(var = "treatment_day") %>%
            t(.)), cluster_row = TRUE, show_rownames = TRUE, cluster_cols = FALSE, show_colnames = FALSE, scale = "row", color = heatmapColors, annotation_col = metabolomicsAvgAnnotation, annotation_colors = metabolomicsColors, name = "Z-Score")
#dev.off()
```

All of the module-specific heatmaps can be found in [this folder](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/metaboanalyst). I think the z-score plots look the cleanest. Clustering columns doesn't make sense if I'm trying to compare heatmaps from different modules. I don't think the module heatmaps are very useful in the text, but having these lists will be nice for when I am writing.

### All VIP

When I met with Ariana, she mentioned that she had shifted to fully parsing through VIP instead of using any WCNA or MetaboAnalyst output. I think there's a happy medium to both approaches, and I set off to find it.

The first thing I did was make heatmaps with my VIP:

![Screenshot 2024-08-29 at 3 23 17 PM](https://github.com/user-attachments/assets/8968c652-ffb8-46a0-9102-d373656dd616)

**Figure 1**. Average metabolite abundance across all samples for VIP

![Screenshot 2024-08-29 at 3 22 35 PM](https://github.com/user-attachments/assets/e9a2324c-dace-4e1f-bd53-adeb36d8ba78)

**Figure 2**. Heatmap of z-scores for all VIP

Once again, I found the plot with z-scores averaged across treatment x day combination to be much easier to interpret. After making the figures, I decided to retry MetaboAnalyst. At SEB, everyone who was doing metabolomics analysis was using MetaboAnalyst. They liked the MetaboAnalyst workflow becuase the GUI was easy to navigate. None of them ran into issues with metabolite names not being recognized, but I wonder if this is because they were all doing targeted metabolomics. Additionally, I got a response to my [OmicsForum post](https://omicsforum.ca/t/unknown-error-occurred-for-kegg-pathway-analysis/3439/2) stating that I was attempting to use an enrichment analysis tool designed primarily for humans. I decided to dig a bit more into these methods to figure out if there was something useful I could get from MetaboAnalyst. I settled on the following workflow:

- Uploaded list of metabolite compound names for significant VIP in the Pathway Analysis module
- Matched to KEGG pathways from *Strongylocentrotus purpuratus*

Since the Pathway Analysis module integrates "enrichment analysis and pathway topology analysis," I did not conduct any other analyses. A few compounds did not have matching annotations (name map [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/allVIP_metaboanalyst-results/name_map.csv)). The following four pathways had significant FDR:

1. [Valine, leucine, and isoleucine biosynthesis](https://www.genome.jp/kegg-bin/show_pathway?spu00290)
2. [Arginine biosynthesis](https://www.genome.jp/kegg-bin/show_pathway?spu00220)
3. [Glutathione metabolism](https://www.genome.jp/kegg-bin/show_pathway?spu00480)
4. [Alaine, aspartate, and glutamate metabolism](https://www.genome.jp/kegg-bin/show_pathway?spu00250)

The statistical output is in [this table](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/allVIP_metaboanalyst-results/pathway_results.csv). Columns include [pathway impact](https://omicsforum.ca/t/what-is-pathway-topological-analysis-pathway-impact/162), which is the output of the pathway topology analysis, and p-values associated with enrichment. The pathway impact takes into account the position of metabolites in a given pathway (ie. node, bottleneck, or terminal metabolite). Maximum importance of a pathway can be 1. I manually calculated the [enrichment ratio](https://omicsforum.ca/t/how-to-interpret-enrichment-ratio-in-enrichment-analysis-result-table/291) as well. I could also download pathway diagrams for the significant pathways. In the diagrams (found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/metaboanalyst/allVIP_metaboanalyst-results)), the VIP are in red. I then set out to make some figures akin to what you would get from the Pathway and Enrichment Analysis modules.

![Screenshot 2024-08-29 at 3 24 44 PM](https://github.com/user-attachments/assets/ffa26324-36b7-401f-bb79-1fb549bf50ab)

**Figure 3**. Pathway impact vs. -log10 enrichment p-value

![Screenshot 2024-08-29 at 3 24 53 PM](https://github.com/user-attachments/assets/c9730a6d-475c-42d8-8cf0-c4b8e9e9c275)

![Screenshot 2024-08-29 at 3 25 48 PM](https://github.com/user-attachments/assets/1122e7aa-5828-478d-a757-7ca5a157e402)

**Figures 4-5**. Various figures showing enrichment ratio and FDR for the top 25 pathways

This version of the MetaboAnalyst analysis gives me a couple of pathways to consider when taking the whole dataset into account. I identified VIP in any of those pathways are present in WCNA modules and saved the output [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/all-module-VIP-pathways.txt).

TL;DR, I now liberally use `%in%` and I found the way MetaboAnalyst makes sense.

### Going forward

1. Update methods
2. Update results
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
