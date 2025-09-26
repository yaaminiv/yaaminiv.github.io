---
layout: post
comments: true
title: Green Crab Experiment Part 47
tags: green-crab metabolomics lipidomics WCNA
---

## Combined metabolomics-lipidomics WCNA

The next step in my endeavor to integrate my datasets is to throw all the metabolites and lipids into a single WCNA, then correlate the module eigengenes with temperature and physiological traits. That way, I'll have a better understanding of how metabolite-lipid interactions influence whole-organism physiology. All my work was done in [this script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/07-integration-analysis.Rmd).

Side note: R Studio on my laptop has a weird font issue. [This Github issue](https://github.com/rstudio/rstudio/issues/16237) helps.

### Run WCNA

The WCNA was pretty staight-forward. I smashed together the formatted dataframes from the metabolomics and lipidomcs WCNAs, identified a soft thresholding power of 6, and ran my analysis. Once I had my modules, I correlated them with physiological traits and temperature separately to mimic what I did with the single-omics WCNAs. I only used temperature since day was not a significant factor in the lipid PCA.

<img width="1145" height="1067" alt="Image" src="https://github.com/user-attachments/assets/bf4e0ba6-7f74-461b-9617-1a28cfefadee" />

**Figure 1**. Combined WCNA correlated with temperature and physiological traits

### Interpretation plan

Honestly I'm not sure how I want to interpret this out. My gut feeling is to focus on significant modules that were correlated with a physiological trait. Whichever method I choose, I'll need lists of the compounds in each module. Of course, getting this information took me longer than I'd like, but I'm happy with how quickly I was able to identify the issues and find solutions! Here's what helped:

- [using `summarise_if` to perform actions across numerical columns](https://dplyr.tidyverse.org/reference/summarise_all.html#ref-examples)
- [creating a list of dataframes using a for loop](https://stackoverflow.com/questions/34832171/using-a-loop-to-create-multiple-data-frames-in-r)
- [naming objects inside a list](https://stackoverflow.com/questions/59620113/how-to-name-dataframes-inside-a-list)
- [outputting dataframes to csvs from a list](https://www.reddit.com/r/rstats/comments/1gsum98/outputting_multiple_dataframes_to_csv_files/)

```
module_alldfs <- list() #Create a list to save dataframes

for (i in 0:19) {
  module_alldfs[[i+1]] <- rbind(module_df %>%
          dplyr::rename(., Module = colors,
                        Compound = Metabolite) %>%
          mutate(Compound = str_trim(Compound, side = "both")) %>%
          filter(., Module == i) %>%
          inner_join(., (rawMetabolomicsData %>%
                           dplyr::rename(Compound = BinBase.name) %>%
                           mutate(Compound = str_trim(Compound, side = "both"))),
                     by = "Compound") %>%
          dplyr::select(., -c(ret.index:mass.spec, InChI.Key)) %>%
          mutate(class = "metabolite"), #Take metabolite information from the module of interest. Trim white space on each side of the compound name. Join with raw metabolomics data and retain columns of interest
        module_df %>%
          dplyr::rename(., Module = colors,
                        Compound = Metabolite) %>%
          mutate(Compound = str_trim(Compound, side = "both")) %>%
          filter(., Module == i) %>%
          inner_join(., (rawLipidomicsData %>%
                           dplyr::rename(Compound = lipid) %>%
                           mutate(Compound = str_trim(Compound, side = "both"))),
                     by = c("Compound")) %>%
          dplyr::select(., -c(identifier:ESI.mode)) %>%
          unique(.) %>%
          group_by(Compound) %>%
          summarise_if(is.numeric, ~sum(., na.rm = TRUE)) %>%
          ungroup(.) %>%
          mutate(., PubChem = NA,
                 KEGG = NA,
                 Module = i,
                 class = "lipid") #Take lipid information from the module of interest. Trim white space on each side of the compound name. Join with raw lipid data and retain columns of interest. Concatenate any duplicate rows for lipids with the same name.
  )
} #Use a for loop to create a list of module dataframes
```

```
module_allInformation <- data.frame("Module" = 0:19,
                                    "Metabolites" = rep(0, times = 20),
                                    "Lipids" = rep(0, times = 20)) #Create empty dataframe to store information
for (i in 0:19) {
  module_allInformation$Metabolites[i+1] <- nrow(module_alldfs[[i+1]] %>% filter(., class == "metabolite")) #Count the number of metabolites in specified list
  module_allInformation$Lipids[i+1] <- nrow(module_alldfs[[i+1]] %>% filter(., class == "lipid")) #Count the number of lipids in the specified list
}
head(module_allInformation) #Confirm dataframe modification
write.csv(module_allInformation, "combined-WCNA/module-information.csv", quote = FALSE, row.names = FALSE)
```

```
for (i in 0:19) {
write_delim(module_alldfs[[i+1]], paste0("combined-WCNA/module", i, "_compounds.tsv"), delim = "\t", col_names = TRUE)
} #Save each module dataframe from list as a tab-delimted file
```

All lists can be found in [this folder](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/07-integration-analysis/combined-WCNA), and the counts of metabolites and lipids in each module can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/07-integration-analysis/combined-WCNA/module-information.csv).

### Interpret output

I decided to move forward with my plan to only look at the modules that had significant correlations with temperature and a physiological trait:

- ME3: Negatively correlated with 5ºC, negatively correlated with righting response, positively correlated with oxygen consumption
- ME6: Negatively correlated with 5ºC, negatively correlated with TTR
- ME13: Negatively correlated with 5ºC, positively correlated with 30ºC, negatively correlated with TTR
- ME11: Negatively correlated with 30ºC, negatively correlated with oxygen consumption

When I looked at the number of metabolites and lipids in each module, almost all of them were single-compound modules! That in itself suggests that metabolites and lipids have unique roles in regulating the physiological endpoints we are interested in. My next step was annotate the compounds in these modules. This was MUCH easier than annotating last week since I already figured out the nuts and bolts of how to do it. I annotated metabolites and lipids separately. I was interested in:

- whether any of the compounds were VIP for temperature
- whether any of the compounds were in a priori pathways or had lipid class information
- whether any of the compounds were in enriched pathways for temperature

```
module3_allMetabAnnot <- module_alldfs[[4]] %>%
  dplyr::select(Compound:KEGG, class) %>%
  filter(., class == "metabolite") %>%
  mutate(., respirationMetabolites = case_when(Compound %in% respirationMetabolites %>% str_trim(., side = "both") == TRUE ~ "Y",
                                               Compound %in% respirationMetabolites %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  mutate(., calciumSignalingMetabolites = case_when(Compound %in% calciumSignalingMetabolites %>% str_trim(., side = "both") == TRUE ~ "Y",
                                                    Compound %in% calciumSignalingMetabolites %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  mutate(., coldProtectionMetabolites = case_when(Compound %in% coldProtectionMetabolites %>% str_trim(., side = "both") == TRUE ~ "Y",
                                                  Compound %in% coldProtectionMetabolites %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  left_join(x = .,
            y = read.delim("../05-metabolomics-analysis/metaboanalyst/all-module-VIP.txt", sep = "\t") %>%
              dplyr::select(metabolite, comparison) %>%
              mutate(metabolite = str_trim(metabolite, side = "both")) %>%
              pivot_wider(., id_cols = metabolite, names_from = "comparison", names_prefix = "VIP", values_from = "comparison") %>%
              mutate(., VIP13v30_day22 = case_when(is.na(VIP13v30_day22) == FALSE ~ "Y",
                                                   is.na(VIP13v30_day22) == TRUE ~ "N")) %>%
              mutate(., VIP13v5_day22 = case_when(is.na(VIP13v5_day22) == FALSE ~ "Y",
                                                  is.na(VIP13v5_day22) == TRUE ~ "N")) %>%
              mutate(., VIP5_day3v22 = case_when(is.na(VIP5_day3v22) == FALSE ~ "Y",
                                                 is.na(VIP5_day3v22) == TRUE ~ "N")) %>%
              dplyr::rename(., Compound = metabolite),
            by = "Compound") %>%
  mutate(., VIP13v30_day22 = case_when(is.na(VIP13v30_day22) == TRUE ~ "N",
                                       is.na(VIP13v30_day22) == FALSE ~ VIP13v30_day22)) %>%
  mutate(., VIP13v5_day22 = case_when(is.na(VIP13v5_day22) == TRUE ~ "N",
                                      is.na(VIP13v5_day22) == FALSE ~ VIP13v5_day22)) %>%
  mutate(., VIP5_day3v22 = case_when(is.na(VIP5_day3v22) == TRUE ~ "N",
                                     is.na(VIP5_day3v22) == FALSE ~ VIP5_day3v22)) %>%
  mutate(., valineBiosynthesis = case_when(Compound %in% valineBiosynthesis %>% str_trim(., side = "both") == TRUE ~ "Y",
                                           Compound %in% valineBiosynthesis %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  mutate(., arginineBiosynthesis = case_when(Compound %in% arginineBiosynthesis %>% str_trim(., side = "both") == TRUE ~ "Y",
                                             Compound %in% arginineBiosynthesis %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  mutate(., glutathioneMetabolism = case_when(Compound %in% glutathioneMetabolism %>% str_trim(., side = "both") == TRUE ~ "Y",
                                              Compound %in% glutathioneMetabolism %>% str_trim(., side = "both") == FALSE ~ "N")) %>%
  mutate(., alanineMetabolism = case_when(Compound %in% alanineMetabolism %>% str_trim(., side = "both") == TRUE ~ "Y",
                                          Compound %in% alanineMetabolism %>% str_trim(., side = "both") == FALSE ~ "N"))


#Take module information and retain columns of interest. Subset for metabolite information. Add annotation information for a priori pathways, VIP for temperature treatments, and enrichment for temperature treatment.
```

```
module3_allLipidAnnot <- module_alldfs[[4]] %>%
  dplyr::select(Compound:KEGG, class) %>%
  filter(., class == "lipid") %>%
  left_join(x = .,
            y = read.delim("../06-lipidomics-analysis/metaboanalyst/all-module-VIP.txt", sep = "\t") %>%
              dplyr::select(lipid, comparison) %>%
              mutate(lipid = str_trim(lipid, side = "both")) %>%
              dplyr::rename(Compound = lipid) %>%
              unique(.) %>%
              pivot_wider(., id_cols = Compound, names_from = "comparison", names_prefix = "VIP", values_from = "comparison") %>%
              mutate(., VIP13v30 = case_when(is.na(VIP13v30) == FALSE ~ "Y",
                                             is.na(VIP13v30) == TRUE ~ "N")) %>%
              mutate(., VIP13v5 = case_when(is.na(VIP13v5) == FALSE ~ "Y",
                                            is.na(VIP13v5) == TRUE ~ "N")) ,
            by = "Compound") %>%
  mutate(., VIP13v30 = case_when(is.na(VIP13v30) == TRUE ~ "N",
                                 is.na(VIP13v30) == FALSE ~ VIP13v30)) %>%
  mutate(., VIP13v5 = case_when(is.na(VIP13v5) == TRUE ~ "N",
                                is.na(VIP13v5) == FALSE ~ VIP13v5)) %>%
  left_join(x = .,
            y = significantVIP_13v30_annot %>%
              rbind(significantVIP_13v5_annot) %>%
              mutate(lipid = str_trim(lipid, side = "both")) %>%
              dplyr::select(lipid, class:saturation) %>%
              unique(.) %>%
              dplyr::rename(lipidClass = class,
                            Compound = lipid),
            by = "Compound") %>%
  left_join(x = .,
            y = read.csv("../06-lipidomics-analysis/metaboanalyst/LION-enrichment-report-13v30/LION-lipid-associations-13v30.csv", strip.white = TRUE) %>%
              slice(-1) %>%
              mutate(LION.term = substr(x = LION.term, start = 9, stop = 1000000L)) %>%
              rename(lipid = LION.term) %>%
              mutate(lipid = str_trim(lipid, side = "both")) %>%
              unique(.) %>%
              remove_rownames() %>%
              column_to_rownames(., var = "lipid") %>%
              dplyr::select_if(colnames(.) %in% LIONtoTerm13v30$LION.term) %>%
              rownames_to_column(., var = "Compound"),
            by = "Compound") %>%
  left_join(x = ., 
            y = read.csv("../06-lipidomics-analysis/metaboanalyst/LION-enrichment-report-13v5/LION-lipid-associations-13v5.csv", strip.white = TRUE) %>%
              slice(-1) %>%
              mutate(LION.term = substr(x = LION.term, start = 9, stop = 1000000L)) %>%
              rename(lipid = LION.term) %>%
              mutate(lipid = str_trim(lipid, side = "both")) %>%
              unique(.) %>%
              remove_rownames() %>%
              column_to_rownames(., var = "lipid") %>%
              dplyr::select_if(colnames(.) %in% LIONtoTerm13v5$LION.term) %>%
              rownames_to_column(., var = "Compound")) %>%
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
                                     LION.0012081 != "x" ~ NA))
#Take module information and retain columns of interest. Subset for lipid information. Add annotation information for VIP for temperature treatments, class and saturation state for VIP, and enrichment for temperature treatment.
```

All of the output can be found [in this folder](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/07-integration-analysis/combined-WCNA).

Since there were so few compounds in each module, I opted not to run any enrichment with MetaboAnalyst or LION/web. I may pull that information at a later time if I'm having trouble writing the discussion.

### Going forward

1. ASCA analysis
2. Metabolomics and lipidomics DIABLO analysis
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
