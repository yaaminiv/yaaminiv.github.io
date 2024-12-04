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

### What's going on with PC and PE lipids?

I did this a while back but never wrote a lab notebook post.....so here's the documentation! I decided to go down a bit of a rabbit hole and figure out how to visualize changes in lipid classes between treatments. The first thing I did was make some really chaotic stacked barplots. To do this, I needed to first pull lipid class information from [MS-DIAL nomenclature](https://systemsomicslab.github.io/compms/msdial/lipidnomenclature.html) and ensure that was reflected in my dataset. This was very annoying and sucked because of weird formatting inconsistencies and the need to Google lipid structures:

```
significantVIP_13v30_annot <- significantVIP_13v30 %>%
  mutate(comparison = "13 vs. 30") %>%
  mutate(., class = lipid) %>%
  separate(., col = class, into = c("class", "structure"), sep = " ") %>%
  mutate(class = gsub(pattern = "4-METHYL-2-OXO-PENTANOIC", replacement = "OxFA", x = class)) %>%
  mutate(class = gsub(pattern = "(eicosanoyl)", replacement = "", x = class)) %>%
  mutate(class = gsub(pattern = "()sphingosine", replacement = "", x = class)) %>%
  mutate(class = gsub(pattern = "N-\\(\\)", replacement = "Cer", x = class)) %>%
  mutate(class = gsub(pattern = "Phosphatidylethanolamine", replacement = "PE", x = class)) %>%
  mutate(targetClass = case_when(class == "TG" ~ class,
                                 class == "PE" ~ class,
                                 class == "PC" ~ class,
                                 class == "PI" ~ class,
                                 class == "PS" ~ class,
                                 class == "PA" ~ class,
                                 class != "TG" | class != "PE" | class != "PC" | class != "PI" | class != "PS" | class != "PA" ~ "Other")) %>%
  separate(., col = structure, into = c("carbons", "oxygen", "rest"), sep = ":") %>%
  separate(., col = oxygen, into = c("oxygen", "trash"), sep = "_") %>%
  dplyr::select(-trash) %>%
  separate(., col = oxygen, into = c("oxygen", "trash"), sep = ";") %>%
  dplyr::select(-trash) %>%
  mutate(., oxygen = gsub(pattern = "\\|", replacement = "", x = oxygen)) %>%
  mutate(., oxygen = gsub(pattern = "PC", replacement = "", x = oxygen)) %>%
  mutate(., oxygen = gsub(pattern = "PE", replacement = "", x = oxygen)) %>%
  mutate(., rest = case_when(is.na(rest) == TRUE ~ "0",
                             is.na(rest) == FALSE ~ rest)) %>%
  mutate(., rest = as.numeric(rest), oxygen = as.numeric(oxygen)) %>%
  mutate(., totDoubleBond = oxygen + rest) %>%
  mutate(., saturation = case_when(totDoubleBond == 0 ~ "Saturated",
                                   totDoubleBond == 1 ~ "Monounsaturated",
                                   totDoubleBond > 1 ~ "Polyunsaturated")) %>%
  dplyr::select(-c(carbons, oxygen, rest, totDoubleBond)) #Take significant VIP from 13 vs. 30 comparison and create a class column. Replace specific anomalies with correct lipid class. Add a column with comparison type. Add a columnn with targeted class information. Separate out structural information to determine if lipids are saturated or unsaturated.

#Replace specific values as needed
significantVIP_13v30_annot$saturation[1] <- "Saturated"
significantVIP_13v30_annot$saturation[28] <- "Monounsaturated"
significantVIP_13v30_annot$saturation[113] <- "Polyunsaturated"
```

```
significantVIP_13v5_annot <- significantVIP_13v5 %>%
  mutate(comparison = "13 vs. 5") %>%
  mutate(., class = lipid) %>%
  separate(., col = class, into = c("class", "structure"), sep = " ") %>%
  mutate(class = gsub(pattern = "Phosphatidylethanolamine", replacement = "PE", x = class)) %>%
  mutate(targetClass = case_when(class == "TG" ~ class,
                                 class == "PE" ~ class,
                                 class == "PC" ~ class,
                                 class == "PI" ~ class,
                                 class == "PS" ~ class,
                                 class == "PA" ~ class,
                                 class != "TG" | class != "PE" | class != "PC" | class != "PI" | class != "PS" | class != "PA" ~ "Other")) %>%
  separate(., col = structure, into = c("carbons", "oxygen", "rest"), sep = ":") %>%
  separate(., col = oxygen, into = c("oxygen", "trash"), sep = "\\+") %>%
  dplyr::select(-trash) %>%
  separate(., col = oxygen, into = c("oxygen", "trash"), sep = "_") %>%
  dplyr::select(-trash) %>%
  mutate(., oxygen = gsub(pattern = "\\|", replacement = "", x = oxygen)) %>%
  mutate(., oxygen = gsub(pattern = "PC", replacement = "", x = oxygen)) %>%
  mutate(., oxygen = gsub(pattern = "PE", replacement = "", x = oxygen)) %>%
  mutate(., rest = case_when(is.na(rest) == TRUE ~ "0",
                             is.na(rest) == FALSE ~ rest)) %>%
  mutate(., rest = as.numeric(rest), oxygen = as.numeric(oxygen)) %>%
  mutate(., totDoubleBond = oxygen + rest) %>%
  mutate(., saturation = case_when(totDoubleBond == 0 ~ "Saturated",
                                   totDoubleBond == 1 ~ "Monounsaturated",
                                   totDoubleBond > 1 ~ "Polyunsaturated")) %>%
  dplyr::select(-c(carbons, oxygen, rest, totDoubleBond)) #Take significant VIP from 13 vs. 5 comparison and create a class column. Replace specific anomalies with correct lipid class. Add a column with comparison type. Add a columnn with targeted class information. Separate out structural information to determine if lipids are saturated or unsaturated.

#Replace specific values as needed
significantVIP_13v5_annot$saturation[59] <- "Polyunsaturated"
```

<img width="1143" alt="Screenshot 2024-12-03 at 10 27 38 PM" src="https://github.com/user-attachments/assets/f908f379-fb7d-42c5-9c79-c2c833c9a8ea">

**Figure 5**. Stacked barplot for every lipid class found in this dataset.

That rainbow monstrosity was not helpful, so I made a different stacked barplot using only classes of interest.

<img width="1143" alt="Screenshot 2024-12-03 at 10 31 22 PM" src="https://github.com/user-attachments/assets/870f05be-3de4-40a6-97a7-ec85f526a4fb">

**Figure 6**. Stacked barplot for lipid classes of interest

Alright, now we were getting somewhere! The next thing I wanted to do was to overlay saturation information on lipid class information and abundance information. I decided to overlay this information onto the stacked barplots I already had:

```{r}
knownTransLipidData %>%
  filter(., treatment != "5") %>%
  dplyr::select(treatment, any_of(significantVIP_13v30$lipid)) %>%
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
  left_join(x = ., y = significantVIP_13v30_annot) %>%
  dplyr::select(-c(estimate:comparison)) %>%
  ggplot(., aes(x = logDiff, y = reorder(lipid, logDiff), fill = saturation)) +
  facet_wrap(~ class, scales = "free_y") +
  geom_bar(aes(fill = saturation, color = status), stat = 'identity') +
  scale_fill_manual(name = "Saturation",
                    values = c("grey40", "grey80", "black")) +
  scale_color_manual(values = c(plotColors[2], plotColors[1]),
                     guide = "none") +
  labs(x = "log(Difference)", y = "Pairwise VIP Lipid") +
  theme_classic(base_size = 15) + theme(axis.ticks.y = element_blank(),
                                        axis.text.y = element_blank()) #Take lipid abundance data and remove data from 5ºC treatment. Select VIP of interest. Calculate average abundance for each lipid and traspose dataframe. Calculate difference in abundance between treatments of interest and calculate log difference. A column for whether the lipid had higher abundance in 13 or 30. Join with annotated VIP data. Remove extra columns and keep only PC and PE data. Create bargraph and facet by lipid class. Fill by saturation state and color by treatment.
```

<img width="1137" alt="Screenshot 2024-12-04 at 1 30 23 PM" src="https://github.com/user-attachments/assets/66cfe65e-b335-4869-bd9d-c45098ffc280">

<img width="1137" alt="Screenshot 2024-12-04 at 1 30 39 PM" src="https://github.com/user-attachments/assets/e898a479-e9ba-43cf-b81b-892290a73141">

**Figures 7-8**. VIP lipid abundances by lipid class

Based on these figures, it seemed like the majority of the action for the 13ºC vs. 30ºC contrast was in the PC and PE lipids, so I made a bar chart with just those categories.

<img width="1137" alt="Screenshot 2024-12-04 at 1 31 31 PM" src="https://github.com/user-attachments/assets/e13555b8-b67f-4bb8-9a3b-ec49c2afd290">

**Figure 9**. VIP lipid abundances for PC and PE lipids

It looks like monounsaturated and saturated lipids are more abundant in 30ºC than 13ºC! This makes sense, since reducing the number of double bonds would add more rigidity to membrane structures. I then did something similar with the 13ºC vs. 5ºC VIP lipids.

<img width="1137" alt="Screenshot 2024-12-04 at 1 33 23 PM" src="https://github.com/user-attachments/assets/001eeecc-2cbf-4c29-8df6-bb4e0cf88c2d">

<img width="1137" alt="Screenshot 2024-12-04 at 1 33 33 PM" src="https://github.com/user-attachments/assets/976c3ca6-4b7a-4c0b-a2bf-c12005bd6ff6">

<img width="1137" alt="Screenshot 2024-12-04 at 1 33 43 PM" src="https://github.com/user-attachments/assets/7a84399c-74ff-4edb-8d4d-6752d9f666e3">

**Figures 10-12**. Abundances by class for 13ºC vs. 5ºC VIP lipids

The same saturation trend isn't prominent at 5ºC, which is interesting given that I saw differences in TTR and movement over the course of the experiment.

Anyways, time to incorporate all of this information as needed into the manuscript.

### Going forward

1. Interpret metabolomic and lipidomic WCNA correlations
2. Integrate physiology and -omics data with WCNA
3. Metabolomics and lipidomics DIABLO analysis
1. Update methods and results
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
