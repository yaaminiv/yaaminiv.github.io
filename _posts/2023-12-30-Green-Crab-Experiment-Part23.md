---
layout: post
comments: true
title: Green Crab Experiment Part 23
tags: green-crab lipidomics mixOmics WGCNA
---

## Preliminary lipidomics analysis

Time to analyze lipidomics data! I used the same workflow as I did for the metabolomics analysis ([multivariate analyses](https://yaaminiv.github.io/Green-Crab-Experiment-Part21/) and [network analyses](https://yaaminiv.github.io/Green-Crab-Experiment-Part22/)). All of my work was conducted in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/06-lipidomics-analysis.Rmd).

I won't go into too much detail, but I'll highlight some results and a few places where the methods weren't easily transferrable.

### Importing and formatting data

This was a MUCH bigger headache than anticipated! The [lipodomics data](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/data/raw_lipidomics_data.csv) is formatted similarly to the metabolomics data, with one main difference: there isn't a unique identifier for every single lipid. Some rows in the table are the same lipid, just captured at different polarities. Therefore, they have the same name or InChiKey. I went back and forth for a really long time trying to figure out if any of the metadata provided by the facility could work as a unique identifier. Instead, I created my own:

```
rawLipidomicsData <- read.csv("../../data/raw_lipidomics_data.csv", header = TRUE) %>%
  dplyr::rename_with(~stringr::str_replace_all(., "X", "")) %>%
  rownames_to_column(., var = "uniqueID") #Import data. Remove the "X" prefix for crab IDs. Convert rownames to a new column, uniqueID, since not every lipid has a unique identifier.
rawLipidomicsData[c(9:77)] <- rawLipidomicsData[c(9:77)] %>% mutate_if(is.character, as.numeric) #Convert data columns to numeric
```

When I kept going through the analysis, I ran into a secondary, similar issue: not every lipid compound with an InChiKey had a recognizable common name or PubChem ID! The common names provided by the facility weren't easily identifiable by MetaboAnalyst. So, I manually acquired common name and CID for InChiKeys using the [Chemical Translation Service](http://cts.fiehnlab.ucdavis.edu/batch) and [PubMed](https://pubchem.ncbi.nlm.nih.gov/). I used the batch service with CTS, but with PubMed I had to manually copy and paste every single InChiKey. It was a pain (both for my wrist and for my soul). Once I had these identifiers, I uploaded them into R and used them to annotate my various lipids and create the dataframe I needed for downstream analyses:

```
inChiKeyNamePubmed <- read.csv("../../data/cts-inchikey-name-pubmed.csv", header = TRUE, na.strings = c("", "NA")) %>%
  dplyr::rename(., INCHIKEY = InChIKey) %>%
  filter(., is.na(Chemical.Name) == FALSE) %>%
  unique(.) #Import table with translations from InChiKeys. Set "" or NA as NA, then remove rows without any chemical name. Remove potential duplicate entries.
nrow(inChiKeyNamePubmed) #391 unique lipids with chemical names and PubMed CID
```

```
transLipidData <- rawLipidomicsData %>%
  right_join(inChiKeyNamePubmed, ., by = "INCHIKEY") %>%
  arrange(., "PubChem.ID") %>%
  dplyr::select(., -c(1:3, 5:10)) %>%
  t(.) %>% as.data.frame(.) %>%
  purrr::set_names(as.character(dplyr::slice(., 1))) %>%
  dplyr::slice(., -1) %>%
  rownames_to_column(., var = "crab.ID") %>%
  left_join(lipidomicsMetadata, ., by = "crab.ID") #Join with inChiKey table and arrange by PubChem CID. This will ensure that any compounds without CID (aka unknowns) are at the bottom. The first 418 rows have PubChem CID (and chemical names). Remove name MS information and keep only the unique IDs (some chemical names and CID are repeated since positive and negative polarities are listed separately). Convert unique IDs to rownames, and transform dataframe. Convert rownames to crab.ID column.
transLipidData[, c(5:2410)] <- transLipidData[, c(5:2410)] %>% mutate_if(is.character, as.numeric) #Convert data columns to numeric
```

### PCA and PLSDA

The PCA shows that lipid profiles vary with temperature, but I don't see super clear separation by day.

<img width="1138" alt="Screenshot 2024-01-09 at 11 14 27 AM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/10f4a806-0330-4684-a63f-4376cd3ef66f">

**Figure 1**. PCA for temperature and day

When I ran the [perMANOVA](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/06-lipidomics-analysis/PCA/all-data-PERMANOVA-results.csv), I saw significant impacts of temperature and day, but not their interaction. Since both main effects were significant, I decided to included both in the PLSDA.

The PLSDA shows that lipid profiles at 30ºC are pretty distinct from the other two temperature treatments!

<img width="1141" alt="Screenshot 2024-01-09 at 11 15 15 AM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/5fae05ab-3498-45b5-af16-bdc09e7f5c79">

**Figure 2**. PLSDA for temperature and day

I did pairwise VIP comparisons and saved the output [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/PLSDA). There were a lot of molecules implicated in pairwise differences that would be worth exploring later on.

### WCNA

<img width="719" alt="Screenshot 2024-01-09 at 11 15 48 AM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/0b15af5e-d2d3-45db-a293-245dd94db40d">

**Figure 3**. WCNA heatmap

The WCNA heatmap is...interesting. While it makes sense that the 13ºC treatments cluster together, it's confusing that the day 3 5ºC and day 22 30ºC treatments cluster together while the day 22 5ºC and day 3 30ºC treatments cluster together. Since there aren't a lot of significant clusters to begin with, I think this is mainly due to increased or decreased patterns of lipid abundance. Decreased abundance of various lipid classes could signify initial slowing at 5ºC, with recovery at day 22, whereas decreased abundance day 22 at 30ºC could be related to poorer survival. The patterns from the WCNA heatmap are even clearer when looking at the boxplots and dotplots.

<img width="1139" alt="Screenshot 2024-01-09 at 11 16 34 AM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/8a720f83-7045-4eeb-b913-d1c0aad836ca">

<img width="1149" alt="Screenshot 2024-01-09 at 11 16 58 AM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/3a104b44-de0e-4b05-a90a-5c7ccc72424a">

**Figures 4-5**. Boxplot and dotplot for module eigengene abundance.

Since there wasn't an interaction effect between temperature and day, it may be worth running the PLSDA for temperature and day separately, then looking at those effects. Or at the very least, plotting the boxplots with temperature and day separately.

### Functional enrichment

So, this is where life REALLY sucked. When I uploaded the list of lipid names to MetaboAnalyst, over half of the names were consistently unidentifiable! Since I was pressed on time for my SICB talk, I went through modules 0, 3, and 6 (since those were ones significantly correlated with multiple conditions) and tried to match compounds without annotations to ones in the MetaboAnalyst database using PubChem IDs or alternative names. Still, the majority of compounds didn't have a matching annotation. I think there are some nuances in namings (ex. 18/1:18/0 vs. 36/1). I need to go through more closely and see if there's an easier way to match annotations as opposed to going through each name manually.

I was able to run the main class enrichment analysis for most modules, but not the KEGG enrichment. Subdirectories for name maps, enriched main classes in each module, and KEGG enrichment results can be found in [this folder](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/metaboanalyst).

It's also possible that instead of lipid pathways, there are a couple of lipid classes that are changing, which is why the global trends aren't as clear. Or, I may need to use a more strict definition of "named lipids". I used anything with an InChiKey, common name from various translation services, and a PubChem ID, but maybe I just need to use common names from the sequencer. I'll need to take a much closer look at this lipid analysis and search around for better methods.

### Going forward

1. Update methods
2. Update results
3. Repeat lipidomics WCNA with temperature and day separately
4. Look into MetaboAnalyst discussion comments
5. Determine how to get more common lipid names that match MetaboAnalyst formatting
6. Reach out to Shelly to see if she tried anything else for lipid analysis
7. Try RaMP enrichment instead of KEGG for lipid sets
6. Normalize for "captivity" effect
7. Look into individual lipid classes from earlier papers
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
