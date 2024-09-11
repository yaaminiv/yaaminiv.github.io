---
layout: post
comments: true
title: Green Crab Experiment Part 35
tags: green-crab lipidomics
---

## Pairwise lipidomics enrichment

After performing an [enrichment anlaysis with overall VIP](https://yaaminiv.github.io/Green-Crab-Experiment-Part34/), I spoke to Ariana and realized a pairwise enrichment may be the best option. A pairwise VIP enrichment will allow me to determine which specific processes are impacted in acclimation to cold or warm temperatures.

### LION enrichment

I returned to my [R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/06-lipidomics-analysis.Rmd) to format inputs for LION. I used the same background for these tests as I did the overall VIP test, which was all known lipids. For the target lists, I exported lists of pairwise VIP lipids along with their identifiers. The lists can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/metaboanalyst). Within LION, I performed a target enrichment. The results can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/metaboanalyst/LION-enrichment-report-13v30) for 13 vs. 30 and [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/metaboanalyst/LION-enrichment-report-13v5) for 13 vs. 5.

<img width="1137" alt="Screenshot 2024-09-11 at 1 01 50 PM" src="https://github.com/user-attachments/assets/d63150ef-d72b-4f33-9515-e46a87fc2a09">

<img width="1137" alt="Screenshot 2024-09-11 at 1 02 04 PM" src="https://github.com/user-attachments/assets/7cef72e4-044e-4bac-9686-06984317b60c">

**Figures 1-2**. Enriched LION terms for 13 vs. 30 or 13 vs. 5

<img width="859" alt="LION-network-13v30" src="https://github.com/user-attachments/assets/e9fb811e-6b13-45f0-8870-833ce9b87fd3">

<img width="859" alt="LION-network-13v5" src="https://github.com/user-attachments/assets/d91069ff-5248-4ebe-b83d-c17122192ade">

**Figure 3-4**. Network diagrams for 13 vs. 30 or 13 vs. 5

I'm seeing glycerophospholipid or membrane-related terms being significantly enriched when I do pairwise enrichment tests! This is really interesting to see, and makes sense biologically that we would see enrichment of these processes. I'll have to dig into with the endoplasmic reticulum is also popping up. Does the ER have a membrane bilayer similar to the cell membrane? The network diagrams are neat visually, but I don't get much information out of them.

### Manual pathway analysis

I then returned to the manual pathway analysis. Since I'm basing my enrichment off of pairwise VIP, I decided to only visualize pairwise VIP in the various categories of interest.

First, I created lists of the significant pairwise VIP, then pulled out pairwise VIP in the category of interest.

```
PCLipids <- rawLipidomicsData %>%
  slice(., 151:390) %>%
  dplyr::select(lipid) #Create a new object with only PC lipid names
nrow(PCLipids) #There are 240 PC lipids, but not all of them may have accompanying PubChem IDs (not all known)

PCDataVIP13v30 <- knownTransLipidData %>%
  dplyr::select(1:4, any_of(PCLipids$lipid)) %>%
  dplyr::select(1:4, any_of(significantVIP_13v30$lipid)) %>%
  column_to_rownames(var = "crab.ID") #Take known lipid data and select metadata columns and columns with names that match PC lipids. Convert crab ID to column name
ncol(PCDataVIP13v30) - 3 #There are 63 known pairwise PC lipids
```

I then made heatmaps and barplots to look at differences in abundance. Since these were pairwise VIP, I only looked at differences between 13ºC and the temperature treatment of interest:

```
#pdf("metaboanalyst/figures/pairwise-VIP-13v30-PC-heatmap.pdf", width = 11, height = 8.5)
pheatmap((PCDataVIP13v30 %>%
            dplyr::select(., -c(day, treatment_day)) %>%
            filter(., treatment != "5") %>%
            group_by(treatment) %>%
            mutate(across(where(is.numeric), log)) %>%
            mutate_if(is.numeric, list(~na_if(., -Inf))) %>%
            replace(is.na(.), 0) %>%
            # mutate(across(where(is.numeric), scale)) %>%
            summarize_all(funs(mean(., na.rm = TRUE))) %>%
            column_to_rownames(var = "treatment") %>%
            t(.)), cluster_row = TRUE, show_rownames = TRUE, fontsize_row = 5,
         cluster_cols = FALSE, show_colnames = FALSE,
         # scale = "row",
         color = heatmapColors,
         annotation_col = lipidomicsAvgAnnotation %>% filter(., Treatment != "5ºC"),
         annotation_colors = lipidomicsColors,
         name = "Scaled Abundance") #Take PC data and remove unnecessary metadata columns. Group by treatment and log-transform numeric columns. Convert -Inf to NA, then NA to 0. Scale all values to obtain z-scores. Average lipid z-scores across treatment. Convert treatment column to rownames. Transpose to obtain matrix. Create heatmap using color schemes and annotations created above.
#dev.off()
```

```{r}
PCDataVIP13v30 %>%
  dplyr::select(., -c(day, treatment_day)) %>%
  filter(., treatment != "5") %>%
  group_by(treatment) %>%
  summarize_all(funs(mean(., na.rm = TRUE))) %>%
  column_to_rownames(var = "treatment") %>%
  t(.) %>% as.data.frame(.) %>%
  rownames_to_column(var = "lipid") %>%
  mutate(., diff = `13` - `30`) %>%
  mutate(., logDiff = log(abs(diff))) %>%
  mutate(., logDiff = case_when(diff < 0 ~ -logDiff,
                                diff > 0 ~ logDiff)) %>%
  mutate(., status = case_when(logDiff > 0 ~ "13",
                               logDiff < 0 ~ "30")) %>%
  ggplot(., aes(x = logDiff, y = lipid, fill = status)) +
  geom_bar(aes(fill = status), stat = 'identity') +
  labs(x = "log(Difference)", y = "") +
  scale_fill_manual(values = c(plotColors[2], plotColors[1]),
                    guide = "none") +
  theme_classic(base_size = 15) + theme(axis.text.y = element_text(size = 6)) #Average treatments of interest for data and transpose. Calculate log difference, negative if higher in the treatment. Create a barplot
ggsave("metaboanalyst/figures/pairwise-VIP-13v30-PC-bargraph.pdf", height = 8.5, width = 11)
```

<img width="1147" alt="Screenshot 2024-09-11 at 1 13 42 PM" src="https://github.com/user-attachments/assets/906ef970-4c94-479c-a574-f0d951656198">

<img width="1147" alt="Screenshot 2024-09-11 at 1 13 53 PM" src="https://github.com/user-attachments/assets/158f9ce6-5b99-42ad-9c88-0d638eef38e7">

**Figures 5-6**. Bar plots for PC lipids

I liked the bar plots a lot! They match with the previous VIP graphs, and they more clearly show which pairwise VIP have higher abundance in 13ºC vs. the temperature of interest. The heatmaps don't show this difference as nicely. I made bar plots for the other categories of interest.

<img width="1147" alt="Screenshot 2024-09-11 at 1 17 28 PM" src="https://github.com/user-attachments/assets/516ce889-9f7a-4013-968d-30b9eccf276c">

<img width="1147" alt="Screenshot 2024-09-11 at 1 17 02 PM" src="https://github.com/user-attachments/assets/0095479e-385d-42cb-a026-85c153e0d77d">

**Figures 7-8**. Bar plots for PE lipids

There weren't any pairwise VIP that were PS lipids, and only one pairwise VIP was a PI lipid for the 13 vs. 30 comparison. This aligns kind of nicely with the Chapelle data. He only looked at PC and PE lipids, which seem to be the most important for temperature changes.

In looking at the PC and PE pairwise VIP, overall it seems like the direction of change is associated with number of double bonds. For the 13 vs. 5 comparisons, lipids with more double bonds were less abundant in 5ºC, while the opposite was true for 13 vs. 30. I'll need to dig into this further.

### Going forward

1. Pairwise VIP enrichment for metabolomics
1. Update methods
2. Update results
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
