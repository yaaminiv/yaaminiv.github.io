---
layout: post
comments: true
title: Green Crab Experiment Part 49
tags: green-crab metabolomics lipidomics ASCA
---

## Retooling individual analyses with ASCA

I originally set out to use an ASCA to understand metabolie-lipid interactions over temperatures. But after talking with Ariana a bit more, I came to the realization that the ASCA is actually a better tool for me to understand my individual compound data, since it is a more quantiative version of a WCNA. I sketched out this quick mind map for how my various analyses would fit together:

![Image](https://github.com/user-attachments/assets/40298fd0-beab-4c9e-ba8f-cdaa2d98d53c)

**Figure 1**. Analytical map

I then set out to use an ASCA to classify groups of compounds by temperature and time (metabolomics) or just temperature (lipidomics). Helpful resources include:

- [ALASCA tutorial](https://andjar.github.io/ALASCA/articles/ALASCA.html)
- [Ariana's lab notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Analysis-of-Variance-Simultaneous-Component-Analyses-ASCA-for-CEABIGR-project/)
- [Ariana's code from CEABiGR](https://github.com/sr320/ceabigr/blob/main/code/00-77-asca-exon.Rmd)

All analyses were performed in [the metabolomics script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/05-metabolomics-analysis.Rmd) and [lipidomics script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/06-lipidomics-analysis.Rmd).

### General methods

An ASCA combines the power of a PCA with an ANOVA to classify variables (in my case, metabolites or lipids) into groups according to conditions listed in a regression model. First, I needed to have data in a long format by condition and variable. This is when I learned that `ALASCA` gets mad if your variable column is not explicitly called "variable" (I originally named it "compound" and got a bunch of error messages about not being able to find "variable", nor was there a "variable" argument for you to specify which column was the variable of interest):

```
metabolomics.ASCA <- rowwiseMetabData %>%
  log(.) %>%
  rownames_to_column(., var = "crab.ID") %>%
  pivot_longer(., cols = !crab.ID, names_to = "variable", values_to = "value") %>%
  left_join(metabolomicsMetadata, ., by = "crab.ID") %>%
  dplyr::select(-treatment_day) %>%
  mutate(., day = str_trim(day, side = "both")) %>%
  mutate(day = case_when(day == "3" ~ "04",
                         day != "3" ~ day)) %>%
  mutate(day = as.factor(day)) #Log-transform data. Pivot data longer for ASCA. Compound information should be "variable." Add metadata to dataframe with all compound information. Update metadata for day and reconvert day to a factor
```

Running the model is very simple: you just need to have a regression formula for your comparison of interest:

```
metabolomics.ASCAmodel <- ALASCA(df = metabolomics.ASCA,
                                 formula = value ~ day*treatment + (1|crab.ID),
                                 n_validation_runs = 100,
                                 validate = TRUE,
                                 reduce_dimensions = TRUE) #Run ASCA to identify day and temperature effects on compound abundance
```

I specified that 100 permutations should be run, and the model should be validated and the number of components should be reduced. More information on those arguments can be found [here](https://andjar.github.io/ALASCA/reference/ALASCA.html). The model ran pretty quickly (~10 seconds). `ALASCA` creates an output folder with run information and tracks all commmands used in a log file.

### Metabolomics results

The `ALASCA` output folder can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/ALASCA).

To interpret the output, I first plotted a scree plot to see the number of components that explained any variance at all. Components 1-5 explained some variance, while component 6 explained 0% variance. For each component, I then plotted the effect plot, which showed me the general trend of compounds that were correlated with the PC, and got a list of the top 5 positively loaded and top 5 negatively loaded compounds. Going through and trying to figure out what the trends were for each component was...confusing. I focused on making comparisons between the control and either the 5ºC or 30ºC treatments. I found it easier to understand these trends when looking at the prediction plots, which showed the abundance of molecules strongly correlated (positively or negatively) with the PC:

<img width="958" height="1248" alt="Image" src="https://github.com/user-attachments/assets/8dca9e7b-673b-4fc4-88b9-03605f7301d8" />

**Figure 2**. Prediction plot for PC1

When I was going through this process, I noticed that component 5 mainly showed trends for compounds that increased in abundance over time for the control, with limited changes at 5ºC or 30ºC! This component also explained less than 10% of the total variance. I decided that I would establish a 10% variance cutoff for components, which would allow me to focus on PCs that explained more variance in the 5ºC and 30ºC temperature treatments.

For each PC, I also got the compound names of the top 20 compounds correlated with the PC and annotated them:

```
loadingsPC1.annot <- loadingsPC1[[1]] %>%
  rename(metabolite = "covars") %>%
  mutate(metabolite = str_trim(metabolite, "both")) %>%
  dplyr::select(PC:metabolite) %>%
  left_join(., 
            y = rbind(t.test_30v5_day3 %>%
                        dplyr::select(metabolite, statistic, p.adj) %>%
                        filter(., p.adj < 0.05) %>%
                        mutate(metabolite = str_trim(metabolite, "both"),
                               comparison = "30v5_day3"),
                      t.test_13v30_day22 %>%
                        dplyr::select(metabolite, statistic, p.adj) %>%
                        filter(., p.adj < 0.05) %>%
                        mutate(metabolite = str_trim(metabolite, "both"),
                               comparison = "13v30_day22"),
                      t.test_13v5_day22 %>%
                        dplyr::select(metabolite, statistic, p.adj) %>%
                        filter(., p.adj < 0.05) %>%
                        mutate(metabolite = str_trim(metabolite, "both"),
                               comparison = "13v5_day22"),
                      t.test_30v5_day22 %>%
                        dplyr::select(metabolite, statistic, p.adj) %>%
                        filter(., p.adj < 0.05) %>%
                        mutate(metabolite = str_trim(metabolite, "both"),
                               comparison = "30v5_day22"),
                      t.test_5_day3v22 %>%
                        dplyr::select(metabolite, statistic, p.adj) %>%
                        filter(., p.adj < 0.05) %>%
                        mutate(metabolite = str_trim(metabolite, "both"),
                               comparison = "5_day3v22")) %>%
              arrange(metabolite), 
            by = "metabolite") %>%
  mutate(., respirationMetabolites = case_when(metabolite %in% respirationMetabolites %>% str_trim(., side = "both") == TRUE ~ "Y",
                                               metabolite %in% respirationMetabolites %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  mutate(., calciumSignalingMetabolites = case_when(metabolite %in% calciumSignalingMetabolites %>% str_trim(., side = "both") == TRUE ~ "Y",
                                                    metabolite %in% calciumSignalingMetabolites %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  mutate(., coldProtectionMetabolites = case_when(metabolite %in% coldProtectionMetabolites %>% str_trim(., side = "both") == TRUE ~ "Y",
                                                  metabolite %in% coldProtectionMetabolites %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  mutate(., valineBiosynthesis = case_when(metabolite %in% valineBiosynthesis %>% str_trim(., side = "both") == TRUE ~ "Y",
                                           metabolite %in% valineBiosynthesis %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  mutate(., arginineBiosynthesis = case_when(metabolite %in% arginineBiosynthesis %>% str_trim(., side = "both") == TRUE ~ "Y",
                                             metabolite %in% arginineBiosynthesis %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  mutate(., glutathioneMetabolism = case_when(metabolite %in% glutathioneMetabolism %>% str_trim(., side = "both") == TRUE ~ "Y",
                                              metabolite %in% glutathioneMetabolism %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  mutate(., alanineMetabolism = case_when(metabolite %in% alanineMetabolism %>% str_trim(., side = "both") == TRUE ~ "Y",
                                          metabolite %in% alanineMetabolism %>% str_trim(., side = "both") == FALSE ~ "N")) #Obtain loadings dataframe from list and modify columns. Join with VIP information, a priori pathway information, and enrichment information.
```

The annotated lists can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/ASCA).

Finally, I plotted effect plots all significant components (PCs 1-4). This figure (with modifications) would be necessary to include in the main text:

<img width="690" height="894" alt="Image" src="https://github.com/user-attachments/assets/2cbc1815-a82e-4c73-b3b8-f771f6db7155" />

**Figure 3**. Metabolomics ASCA results

- PC1: This component shows metabolites with consistently higher or lower abundance than 5ºC or 13ºC that change abundance (generally increasing) at 30ºC over time.
- PC2: This component highlights compounds in 5ºC that have similar abundance to 13ºC in day 4, but different in day 22.
- PC3: Absolute abundance in 5ºC changes over time but 30ºC remains stable
- PC4: Absolute change in abundance over time in 30ºC is more extreme than at 5ºC.

### Lipidomics results

I followed the same procedure for the lipidomics data [in this script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/06-lipidomics-analysis.Rmd). I ended up with two PCs above the 10% threshold. Since I was looking at differences in temperature without the time variable, it was a bit easier to interpret the different patterns. The `ALASCA` output can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/ALASCA/2025-10-03T122355).

<img width="966" height="1213" alt="Image" src="https://github.com/user-attachments/assets/909c7533-1883-450b-a5d7-d21fc7b14e37" />

**Figure 4**. Lipidomics ASCA results

- PC1: This component shows lipids with lower absolute abundance at 5ºC and much lower absolute abundance at 30ºC
- PC2: This component shows lipids with lower absolute abundance at 30ºC and much lower absolute abundance at 5ºC

As expected, annotating the lipids was annoying but way quicker for me to do since [I already cracked how to do it](https://yaaminiv.github.io/Green-Crab-Experiment-Part46/):

```
loadingsPC2.annot <- loadingsPC2[[1]] %>%
  rename(lipid = "covars") %>%
  mutate(lipid = str_trim(lipid, "both")) %>%
  dplyr::select(PC:lipid) %>%
  left_join(., 
            y = rbind(t.test_13v30 %>%
                        dplyr::select(lipid, statistic, p.adj) %>%
                        filter(., p.adj < 0.05) %>%
                        mutate(lipid = str_trim(lipid, "both"),
                               comparison = "13v30"),
                      t.test_13v5 %>%
                        dplyr::select(lipid, statistic, p.adj) %>%
                        filter(., p.adj < 0.05) %>%
                        mutate(lipid = str_trim(lipid, "both"),
                               comparison = "3v5"),
                      t.test_30v5 %>%
                        dplyr::select(lipid, statistic, p.adj) %>%
                        filter(., p.adj < 0.05) %>%
                        mutate(lipid = str_trim(lipid, "both"),
                               comparison = "30v5")) %>%
              arrange(lipid), 
            by = "lipid") %>%
  left_join(.,
            y = rbind(significantVIP_13v30_annot, significantVIP_13v5_annot) %>%
              dplyr::select(lipid, class, targetClass, saturation) %>%
              mutate(lipid = str_trim(lipid, "both")) %>%
              unique(.),
            by = "lipid") %>%
  left_join(x = .,
            y = read.csv("../06-lipidomics-analysis/metaboanalyst/LION-enrichment-report-13v30/LION-lipid-associations-13v30.csv", strip.white = TRUE) %>%
              slice(-1) %>%
              mutate(LION.term = substr(x = LION.term, start = 9, stop = 1000000L)) %>%
              rename(lipid = LION.term) %>%
              mutate(lipid = str_trim(lipid, side = "both")) %>%
              filter(., lipid %in% (loadingsPC2[[1]]$covars %>%
                                      str_trim(side = "both"))) %>%
              unique(.) %>%
              remove_rownames() %>%
              column_to_rownames(., var = "lipid") %>%
              dplyr::select_if(colnames(.) %in% LIONtoTerm13v30$LION.term) %>%
              rownames_to_column(., var = "lipid") %>%
              left_join(x = .,
                        y = read.csv("../06-lipidomics-analysis/metaboanalyst/LION-enrichment-report-13v5/LION-lipid-associations-13v5.csv", strip.white = TRUE) %>%
                          slice(-1) %>%
                          mutate(LION.term = substr(x = LION.term, start = 9, stop = 1000000L)) %>%
                          rename(lipid = LION.term) %>%
                          mutate(lipid = str_trim(lipid, side = "both")) %>%
                          filter(., lipid %in% (loadingsPC2[[1]]$covars %>%
                                                  str_trim(side = "both"))) %>%
                          unique(.) %>%
                          remove_rownames() %>%
                          column_to_rownames(., var = "lipid") %>%
                          dplyr::select_if(colnames(.) %in% LIONtoTerm13v5$LION.term) %>%
                          rownames_to_column(., var = "lipid"),
                        by = c("lipid", "LION.0000003", "LION.0012080")) %>%
              mutate(., LION.0000003 = case_when(LION.0000003 == "x" ~ "both",
                                                 LION.0000003 != "x" ~ NA)) %>%
              mutate(., LION.0000010 = case_when(LION.0000010 == "x" ~ "13v30",
                                                 LION.0000010 != "x" ~ NA)) %>% 
              mutate(., LION.0000031 = case_when(LION.0000031 == "x" ~ "13v30",
                                                 LION.0000031 != "x" ~ NA)) %>% 
              mutate(., LION.0000095 = case_when(LION.0000095 == "x" ~ "13v30",
                                                 LION.0000095 != "x" ~ NA)) %>%   
              mutate(., LION.0000465 = case_when(LION.0000465 == "x" ~ "13v30",
                                                 LION.0000465 != "x" ~ NA)) %>%    
              mutate(., LION.0012010 = case_when(LION.0012010 == "x" ~ "13v30",
                                                 LION.0012010 != "x" ~ NA)) %>%
              mutate(., LION.0012080 = case_when(LION.0012080 == "x" ~ "both",
                                                 LION.0012080 != "x" ~ NA)) %>%
              mutate(., LION.0000011 = case_when(LION.0000011 == "x" ~ "13v5",
                                                 LION.0000011 != "x" ~ NA)) %>%
              mutate(., LION.0012081 = case_when(LION.0012081 == "x" ~ "13v5",
                                                 LION.0012081 != "x" ~ NA)),
            by = "lipid") #Obtain loadings dataframe from list and modify columns. Join with VIP information, a priori pathway information, and enrichment information. For enrichment information, import table with lion terms as columns and lipids as rows. Remove first row, which has lion term names. Remove leading characters from lipids, rename column, and remove white space on both sides of the string. Filter for VIP lipids in module, modified to remove white space. Remove repetitive rows and remove rownames. Assign new rownames using unique lipid names. Select columns if they match with lion terms in LIONtoTerm13v30. Convert rownames back to a column. Repeat process with 13v5 LION terms and join with dataframe based on common LION terms and lipids. Use case_when to pinpoint which LION terms ar related to specific enrichment categories
```

The lipid annotations for each PC can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/ASCA).

When _very annoying_ thing is that the `ALASCA` plots are a pain to customize! There's no real way to customize the plot (remove a legend, change colors of speciifc points) beyond what is provided [in the visualization guide](https://andjar.github.io/ALASCA/articles/plot.html). I decided to shunt that problem to next week when I update methods, results, and figures. I did, however, manage to play around a bit with some plotting customization. I found that you could save the individual effect plot (without any variable information) as an object, then customize it further using `ggplot` since all `ALASCA` plots are `ggplot` objects.

### Going forward

1. Update methods
3. Update results
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
