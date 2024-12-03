---
layout: post
comments: true
title: Green Crab Experiment Part 41
tags: green-crab TTR lipidomics
---

## Supplementing the lipidomics analysis

Carolyn left a couple of other comments on my lipid analysis, so I'll address them in this notebook post.

### Do VIP lipids change across time?

The first question Carolyn had is if the abundance of VIP lipids change between days 3 and 22. Having some sort of supplemental figure would further demonstrate that there was no significant impact of time on lipid abundance. To do this, I decided to just make a some clustered heatmaps:

```
pheatmap((knownTransLipidData %>%
            filter(., treatment != "5") %>%
            dplyr::select(treatment_day, any_of(significantVIP_13v30$lipid)) %>%
            group_by(., treatment_day) %>%
            summarize_all(funs(mean(., na.rm = TRUE))) %>%
            column_to_rownames(var = "treatment_day") %>%
            t(.) %>% as.data.frame(.) %>%
            log(.) %>%
            mutate_if(is.numeric, list(~na_if(., -Inf))) %>%
            replace(is.na(.), 0) %>%
            as.matrix()), cluster_row = TRUE, fontsize_row = 4, show_rownames = TRUE,
         cluster_cols = TRUE, show_colnames = TRUE,
         color = heatmapGreyscale,
         name = "Abundance")
```
![Screenshot 2024-12-03 at 12 08 16 PM](https://github.com/user-attachments/assets/11d06404-fa9c-4aa9-86fd-68f43b1399ee)

![Screenshot 2024-12-03 at 12 08 08 PM](https://github.com/user-attachments/assets/180de232-8727-4581-993c-8215e6e185d8)

**Figures 1-2**. Heatmaps showing log abundance for 13 vs. 30 or 13 vs. 5 VIP lipids

As expected, there wasn't a large difference in lipid abundance between those two timepoints!

### Identifying VIP lipids between 5ºC and 30ºC

The second supplementary analysis suggested was to identify VIP between 5ºC and 30ºC. The purpose of this would be to see if there are any compounds that have different abundances at the two temperature extremes, and use that to understand changes in pathways. I first identified pairwise VIP:

```
list <- c("30", "5") #Assign treatments to examine

X <- knownTransLipidData %>%
  filter(., treatment %in% list) %>%
  droplevels() #Filter knownTransLipidData for desired contrasts
Y <- as.factor(X$treatment) #Treatments for Y
X <- X[, 5:419] #Retain data only

known.plsda_30v5 <- plsda(X, Y, ncomp = 3) #Run PLSDA for desired contrast
VIP_30v5 <- as.data.frame(PLSDA.VIP(known.plsda_30v5)[["tab"]]) %>%
  rownames_to_column(., var = "lipid") %>%
  filter(., VIP >= 1) #Extract VIP list and filter for VIP > 1
nrow(VIP_30v5)

clean_30v5 <- knownTransLipidData %>%
  dplyr::filter(., treatment %in% list) %>%
  droplevels() #Filter knownTransLipidData for desired contrasts.
VIP_select_30v5 <- cbind(clean_30v5[, 1:4],
                         (log(clean_30v5[, names(clean_30v5) %in% VIP_30v5$lipid]) %>%
                            mutate_if(is.numeric, list(~na_if(., -Inf))) %>%
                            replace(is.na(.), 0))) %>%
  pivot_longer(., cols = 5:153, values_to = "log_norm_counts", names_to = "lipid") %>%
  group_by(., lipid) #cbind metadata columns (1:4) and log-transformed "clean" data for VIP. Pivot data longer and group by lipids.
VIP_select_30v5 #Confirm changes

t.test_30v5 <-do(VIP_select_30v5, tidy(t.test(.$log_norm_counts ~ .$treatment,
                                              alternative = "two.sided",
                                              mu = 0,
                                              var.equal = FALSE,
                                              conf.level = 0.95
))) #Looped t-test for all lipids with VIP > 1
t.test_30v5$p.adj <- p.adjust(t.test_30v5$p.value, method = c("fdr"), n = length(VIP_30v5$lipid)) #Adjust p value for the number of comparisons
t.test_30v5
```

![Screenshot 2024-12-03 at 12 23 12 PM](https://github.com/user-attachments/assets/a55e58d3-cd08-484d-aa18-ca6ee7f1a166)

**Figure 3**. VIP lipids identified between 30ºC and 5ºC

I identified 135 VIP lipids, with 86 lipids having a higher abundance in 30ºC and 49 lipids having a higher abundance in 5ºC. I saved the output [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/06-lipidomics-analysis/PLSDA/known-lipids-30v5-VIP.csv). Since the purpose of this analysis was to determine if there were any VIP lipids not represented in the 13 vs. 30 or 13 vs. 5 contrasts, I pared down my list to unique VIP lipids identified only when comparing 30ºC and 5ºC:

```{r}
t.test_30v5 %>%
  ungroup(.) %>%
  filter(., p.adj < 0.05) %>%
  anti_join(x = ., y = (t.test_13v5 %>%
                          ungroup(.) %>%
                          filter(., p.adj < 0.05)),
            by = "lipid") %>%
  anti_join(x = ., y = (t.test_13v30 %>%
                          ungroup(.) %>%
                          filter(., p.adj < 0.05)),
            by = "lipid") %>%
  right_join(x = lipidIdentifiers, y = ., by = "lipid") %>%
  write.csv("PLSDA/unique-known-lipids-30v5-VIP.csv", quote = FALSE, row.names = FALSE) #Take the t-test results and filter out the significant entries. Use anti-join to identify lipids that are unique to the 30 vs. 5 comparison. Join with lipid identifier information and save as a .csv
```

The output is saved [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/06-lipidomics-analysis/PLSDA/unique-known-lipids-30v5-VIP.csv).

![Screenshot 2024-12-03 at 12 23 21 PM](https://github.com/user-attachments/assets/a14936bb-6a99-4e70-a263-935b9af334cc)

**Figure 4**. Unique VIP lipids identified between 30ºC and 5ºC.

Now that I had [unique VIP lipids](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/06-lipidomics-analysis/metaboanalyst/unique_30v5VIP_lipids.txt), I performed an enrichment analysis with LION/web to see if there were any lipid ontology terms that were unique to this set when compared to the [background](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/06-lipidomics-analysis/metaboanalyst/allKnown_lipids.txt).

The LION output can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/metaboanalyst/LION-enrichment-report-30v5-unique). I did not identify any significantly enriched LION terms with this set!

### Going forward

1. Address remaining comments on paper draft
2. Update methods and results
1. Interpret metabolomic and lipidomic WCNA correlations
2. Integrate physiology and -omics data with WCNA
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
