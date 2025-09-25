---
layout: post
comments: true
title: Green Crab Experiment Part 47
tags: green-crab metabolomics lipidomics WCNA
---

## Combined metabolomics-lipidomics WCNA

The next step in my endeavor to integrate my datasets is to throw all the metabolites and lipids into a single WCNA, then correlate the module eigengenes with temperature and physiological traits. That way, I'll have a better understanding of how metabolite-lipid interactions influence whole-organism physiology. All my work was done in [this script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/07-integration-analysis.Rmd).

### Run WCNA

The WCNA was pretty staight-forward. I smashed together the formatted dataframes from the metabolomics and lipidomcs WCNAs, identified a soft thresholding power of 6, and ran my analysis. Once I had my modules, I correlated them with physiological traits and temperature separately to mimic what I did with the single-omics WCNAs. I only used temperature since day was not a significant factor in the lipid PCA.

<img width="1145" height="1067" alt="Image" src="https://github.com/user-attachments/assets/bf4e0ba6-7f74-461b-9617-1a28cfefadee" />

**Figure 1**. Combined WCNA correlated with temperature and physiological traits

### Interpret output

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

### Going forward

1. Intepret combinede WCNA
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
