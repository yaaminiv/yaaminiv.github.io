---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 30
tags: killifish RRBS
---

## Fixing DMR figures

When working on [green crab metabolomics analysis](https://yaaminiv.github.io/Green-Crab-Experiment-Part30/), I realized that I used `summarize_all` incorrectly! Instead of averaging methylation for CpGs in a DMR, I accidentally added all methylation. That could explain the weird heatmaps. I went back into [this code](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/08-annotate-DMR.Rmd) to correct my mistake.

### Heatmaps

I revised my code to actually average methylation then made heatmaps.

```
NBH20vOCavgMeth <- NBH20vOCpositions %>%
  group_by(., DMR) %>%
  dplyr::select(., -c(chr, pos)) %>%
  summarize_all(funs(mean(., na.rm = TRUE))) %>%
  slice(., 1, 3:10, 2) %>%
  column_to_rownames(var = "DMR") #Group by DMR. Remove chromosome and position columns. Average methylation level across all CpG positions in each DMR for each sample. Reorganize manually with slice. Change DMR column to rownames
```

In addition to using the correct data, I installed `ComplexHeatmap` as a wrapper to `pheatmap`. This gave me some flexibility in plotting. I also added rownames (DMR IDs), plotted percent methylation, and updated the legend name:

```
pdf("figures/20_OC_N-DMR-heatmap.pdf", width = 11, height = 8.5)

pheatmap(as.matrix(NBH20vOCavgMeth*100), cluster_row = TRUE, show_rownames = TRUE, cluster_cols = FALSE, show_colnames = FALSE, color = heatmapColors, annotation_col = NBH20vOCoxygenTreatment, annotation_colors = NBH20vOCoxygenTreatmentColors, annotation_names_col = FALSE, name = "Methylation (%)")

dev.off()
```

![Screenshot 2024-09-03 at 11 06 02 AM](https://github.com/user-attachments/assets/2f3d500e-5871-4b97-82d9-3f7468db175c)

![Screenshot 2024-09-03 at 11 05 47 AM](https://github.com/user-attachments/assets/cd0574f7-77f6-4135-83ff-e5647d3e9710)

**Figures 1-2**. Heatmaps of average methylation across samples for normoxia vs. outside control DMR and hypoxia vs. outside control DMR.

Looking at these heatmaps, I still see inter-individual variation. However, there are more DMR with clearer differences between treatments when comparing the control and severe hypoxia treatments. 

### `BAT_correlating` plots

I went through the rest of my code to see if there was any downstream analysis that used the dataframe of averaged methylation. Turns out, my correlation plots did! I reran the chunk of code that performed several `left_join` commands to get the dataframe for the correlation plot, then remade figures.

```
NBH20vOCcorrOnly <- NBH20vOCBATcorrelating %>%
  filter(., is.na(rho.p.val) == FALSE) %>%
  left_join(., (NBH20vOCavgMeth %>%
                  rename_with(., function(x) paste0(x, sep = "_meth")) %>%
                  rownames_to_column(., var = "DMR")), by = "DMR") %>%
  left_join(., NBH20geneExp, by = "gene.name") %>%
  left_join(., NBHOCgeneExp, by = "gene.name") %>%
  dplyr::select(-c(chr, DMR.start, DMR.end, direction, gene.start, gene.end, strands, "OC-N3_exp")) %>%
  pivot_longer(cols = "NO_20.N4_meth":"OC_OC.N4_meth", names_to = "sample_meth", values_to = "meth") %>%
  pivot_longer(cols = "20-N1_exp":"OC-N5_exp", names_to = "sample_exp", values_to = "exp") %>%
  mutate(sample_meth = gsub(x = sample_meth, pattern = "_meth", replacement = "")) %>%
  mutate(sample_meth = gsub(x = sample_meth, pattern = "NO_20.", replacement = "20-")) %>%
  mutate(sample_meth = gsub(x = sample_meth, pattern = "OC_OC.", replacement = "OC-")) %>%
  mutate(sample_exp = gsub(x = sample_exp, pattern = "_exp", replacement = "")) %>%
  filter(., sample_meth == sample_exp) %>%
  mutate(sample = sample_meth) %>%
  separate(., col = sample_meth, into = c("treatment", "extra"), sep = "-") %>%
  dplyr::select(-c(sample_exp, extra))
#Take BAT_correlating output and retain rows that have a correlation p-value. Join with average methylation data where the columns are renamed to indicate methylation values. Join with gene expression data. Remove unecessary columns. Pivot longer for methylation and expression data separately. Modify column contents and retain rows where the sample ID for methylation and expression match. Create a new sample ID column. Create a new column for treatment specifications by separating defunct sample_meth column. Remove extra columns
```

![Screenshot 2024-09-03 at 11 05 53 AM](https://github.com/user-attachments/assets/40304d69-c11f-44c4-8ae4-0020b5e3192e)

**Figure 3**. Correlation plot for normoxia vs. outside control DMR

![Screenshot 2024-09-03 at 11 05 37 AM](https://github.com/user-attachments/assets/138f3e58-456c-4bbc-8660-07652d8a9a22)

![Screenshot 2024-09-03 at 11 05 21 AM](https://github.com/user-attachments/assets/903a8860-1d61-4063-bdb2-432c662995a6)

**Figures 4-5**. Hypoxia vs. outside control DMR plots for all correlations and only significant correlations

### Going forward

1. Update methods and results
5. Identify known SNP/DMR overlaps
6. Update OSF repository for all intermediate files

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
