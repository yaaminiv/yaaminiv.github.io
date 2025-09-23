---
layout: post
comments: true
title: Green Crab Experiment Part 46
tags: green-crab metabolomics lipidomics WCNA
---

## Intergrating physiology data with WCNA

I want to tie average TTR and respirometry information with metabololite and lipid profiles. My plan is to find a way to add that information into the WCNA first, then see if I can do something similar with and ASCA or DIABLO to also tie the metabolomics and lipidomics data together. All my work was done in [this script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/07-integration-analysis.Rmd).

### Format input data

The first thing I wanted to do was format my TTR and respirometry data in a format similar to the metabolomics and lipidomics data used for WCNA! The big thing was ensuring that all of the treatment columns were similar:

In this process, I needed to grab the sampling information from the TTR metadata. Looking at this, I noticed two things:

```{r}
rawTTR <- read.csv("../../data/time-to-right.csv", header = TRUE) #Import raw data

modTTR <- rawTTR %>%
  mutate(., day = case_when(date == "7/8/2022" ~ 4,
                            date == "7/12/2022" ~ 8,
                            date == "7/15/2022" ~ 11,
                            date == "7/19/2022" ~ 15,
                            date == "7/22/2022" ~ 18,
                            date == "7/26/2022" ~ 22)) %>%
  mutate(., treatment = case_when(treatment.tank == "1" | treatment.tank == "4" ~ "13C",
                                  treatment.tank == "2" | treatment.tank == "5" ~ "5C",
                                  treatment.tank == "3" | treatment.tank == "6" ~ "30C")) %>%
  mutate(., missing.legs = case_when(number.legs < 10 ~ "Y",
                                     number.legs == 10 ~ "N")) %>%
  mutate(., integument.cont = recode(integument.color,
                                     "B" = 0,
                                     "BG" = 0.5,
                                     "G" = 1, 
                                     "YG" = 1.5, 
                                     "Y" = 2, 
                                     "YO" = 2.5 , 
                                     "O" = 3, 
                                     "RO" = 3.5)) %>%
  mutate(., trial.1 = na_if(trial.1, 91.00)) %>%
  mutate(., trial.2 = na_if(trial.2, 91.00)) %>%
  mutate(., trial.3 = na_if(trial.3, 91.00)) %>%
  rowwise(.) %>% 
  mutate(., TTRavg = mean(c(trial.1, trial.2, trial.3), na.rm = TRUE)) %>%
  filter(., is.na(TTRavg) == FALSE) %>%
  mutate(., TTRSE = std.error(c(trial.1, trial.2, trial.3), na.rm = TRUE)) %>%
  filter(., is.na(TTRSE) == FALSE) %>%
  group_by(., day, treatment) %>%
  mutate(., TTRavgFull = mean(TTRavg)) %>%
  mutate(., TTRSEFull = std.error(TTRavg)) %>%
  mutate(., TTRavgFullLow = TTRavgFull - TTRSEFull) %>%
  mutate(., TTRavgFullHigh = TTRavgFull + TTRSEFull) %>%
  ungroup(.) #Remove sampling ID and notes columns. Add new column with day information. Add new column with treatment information. Create a a new column that with an index for width/length. Create new column as a binary for whether or not crabs are missing legs. Change all 91 to NA, calculate average and SE using data from the three TTR trials using rowwise operations, and remove rows where TTRavg | TTRSE = NA.  Calculate average and SE for all samples in a treatment for each day. Add/subtract TTRSEFull to/from TTRavgFull to get bounds. Ungroup.

modTTRSampled <- modTTR %>%
  filter(is.na(sampling.ID) == FALSE) %>% 
  dplyr::select(-c(date, trial.1:trial.3, carapace.length, notes, TTRavgFull:TTRavgFullHigh)) %>% 
  rename(crab.ID = sampling.ID) %>%
  rename(tank = treatment.tank) %>%
  rename(probe_num = probe.number) %>% 
  mutate(treatment = gsub(pattern = "C", replacement = "", x = treatment) %>% as.factor(.)) %>%
  mutate(day = as.factor(day)) %>% 
  mutate(., unite(., "treatment_day", treatment:day, sep = "_")) %>%
  mutate(., treatment_day = gsub(x = treatment_day, pattern = "_  3", replacement = "_3")) %>%
  mutate(., treatment_day = gsub(x = treatment_day, pattern = "_ 22", replacement = "_22")) %>%
  mutate(., treatment_day = gsub(x = treatment_day, pattern = " 13_", replacement = "13_")) %>%
  mutate(., treatment_day = gsub(x = treatment_day, pattern = " 30_", replacement = "30_")) %>%
  mutate(., treatment_day = gsub(x = treatment_day, pattern = "  5_", replacement = "5_")) #Retain only crabs that were sampled and remove any superfluous columns. Rename specific columns to match respirometry data. Change certain columns to characters in order to match with metabolomics/lipidomics data.
```

```
attach("../04-respirometry-analysis/04-respirometry-analysis.RData") #Attach respirometry RData
all_MO2_slope_results_demTTRdata <- all_MO2_slope_results_demTTRdata #Data used for respirometry GLM
detach("file:../04-respirometry-analysis/04-respirometry-analysis.RData") #Detach respirometry RData

all_MO2_slope_results_demTTRdata %>%
  mutate(treatment = gsub(pattern = "C", replacement = "", x = treatment) %>% as.factor(.)) %>%
  mutate(., day = case_when(day == "04" ~ "4",
                            day != "04" ~ day) %>%
           as.factor(.)) %>%
  mutate(., unite(., "treatment_day", treatment:day, sep = "_")) %>%
  mutate(., treatment_day = gsub(x = treatment_day, pattern = "_  3", replacement = "_3")) %>%
  mutate(., treatment_day = gsub(x = treatment_day, pattern = "_ 22", replacement = "_22")) %>%
  mutate(., treatment_day = gsub(x = treatment_day, pattern = " 13_", replacement = "13_")) %>%
  mutate(., treatment_day = gsub(x = treatment_day, pattern = " 30_", replacement = "30_")) %>%
  mutate(., treatment_day = gsub(x = treatment_day, pattern = "  5_", replacement = "5_")) %>%
  filter(., day == "4" | day == "22") %>%
  left_join(., modTTRSampled, by = c("treatment", "day", "treatment_day", "tank", "probe_num", "sex", "integument.cont", "weight", "carapace.width", "missing.legs")) %>%
  dplyr::select(-c(TTRavg:TTRSE)) #Change treatment, day, and treatment_day columns to match metabolomics/lipidomics datasets. Retain only respirometry data on the days that sampling was done. Join with modTTRSampled by all columns in common to get crab.ID. Remove superfluous columns. 
```

1. My 5ºC samples weren't necessarily placed in the correct tubes. But I remember correcting the tube labels to actually match the samples when sending them in, so that's set.
2. I TOOK MY SAMPLES ON DAY 4, NOT DAY 3!!

Noticing this error, I also went back to the experimental design and PLSDA figures and made necessary changes. This involved changing the InDesign documents, not any R code or output figure because I was thinking about efficiency.

When trying to create trait matrices to correlate with WCNA, I also noticed that there were some discrepancies. At one point, I had multiple crabs labelled 302 and 303. I ended up fixing this error in the TTR datasheet pretty easily, since 301-303 and 304-onwards were sampled on different days! When merging dataframes, I noticed that crabs 304, 308, and 506 were all discarded from the TTR analysis since they failed to right. However, 304 had respirometry information, so I had to go in manually to ensure that label still existed in the final trait matrix.

### Get correlations between WCNA modules and with physiology

I wanted to correlate metabolite and lipid profiles with average TTR and respirometry data on the days the crabs were dissected in three steps:

1. Metabolomics and lipidomics WCNAs separately, correlate with physiology data only
2. Metabolomics and lipidomics WCNAs separately, correlate with physiology and treatment data
3. Combined metabolomics and lipidomics WCNAs

The main difference between the first two points were purely visualization. My visualizations are below:

**Figures 1-2**. Correlations between metabolomics modules and physiology only, or metabolomics modules and physiology and treatment information

Comparing Figure 2 with the existing manuscript figures, I noticed that the correlation and P-values were different for the treatment x module correlations! However, the physiology correlations were consistent, which was to be expect. After breaking my brain, my guess was because I was using an updated R version and updated package versions in this round. I tried combining the correlations from physiology alone (derived today) and with the already saved information with the existing temperature WCNA output. I could then combine those two correlation matrices in a heatmap:

**Figure 3**. Existing module-trait correlations with new physiology data appended for metabolomics

I'm going to stick with this figure for now since it will allow me to maintain consistency between analyses I've done previously with the new analysis!

I did all the same things for the lipidomics data:

**Figures 4-6**. Correlations between lipidomics modules and physiology only, physiology and temperature from today, and physiology appended onto previous temperature correlations.

### Intepret output

To aid in interpretation, I did a few things:

1. Redo my correlations! I realized I did the original correlations with slope instead of negative slope. Using slope meant that a more negative number was increased oxygen consumption, and this is annoying to interpret. I redid the correlations with negative slope (which just changed the sign of my correlations, not the significance) so that a larger oxygen consumption number meant increased consumption.
2. Plug metabolites from significant physiology modules into MetaboAnalyst to get pathway information
3. Plug lipid from signifficant lipidomics modules into LION/web to get pathway information

Then came the annoying (metabolomics) and extremely annoying (lipidomics) parts. For each module, I wanted to combine information for temperature-specific VIP, a-priori pathways or classifications, enrichment information for temperature, and enrichment information for physiology modules. Of course, this simple task took eons (multiple days for lipidomics because of more difficult formatting puzzles). Here are some things that proved helpful for the lipidomics formatting:

- [Removing whitespace with `str_trim`](https://www.gastonsanchez.com/r4strings/stringr-basics.html#trimming-with-str_trim)
- [Removing rownames with `remove_rownames`](https://tibble.tidyverse.org/reference/rownames.html)
- [`summarise(across)` to collapse rows with the same ID columns but with data across multiple rows](https://stackoverflow.com/questions/72853248/r-combine-rows-with-same-id)
- [`rename_with(across)` to add a prefix or suffix to several columns](https://stackoverflow.com/questions/71303026/adding-a-suffix-to-a-selection-of-column-names-in-tidyverse)

In particular, the hardest part for the lipidomics was assigning LION terms to lipids within a module. Here's how I finally cracked it:

```
module15Lipid_enrichment <- read.csv("WCNA/lipidomics/LION-enrichment-report-module15phys/LION-enrichment-results.csv") %>%
  filter(., FDR.q.value < 0.05)
module15Lipid_enrichment #5 enriched terms in module
```

```
LIONtoTermmodule15 <- read.csv("WCNA/lipidomics/LION-enrichment-report-module15phys/LION-term-associations.csv") %>%
  dplyr::select(LION.term, LION.name) %>%
  slice(-1) %>%
  filter(LION.term %in% module15Lipid_enrichment$Term.ID) %>% 
  mutate(LION.term = gsub(x = LION.term, pattern = ":", replacement = "."))
#Import dataframe with lion terms and term names. Retain only significantly enriched terms
head(LIONtoTermmodule15) #Confirm formatting
```

```
module15Lipids_enrichedPhys <- read.csv("WCNA/lipidomics/LION-enrichment-report-module15phys/LION-lipid-associations.csv", strip.white = TRUE) %>%
  slice(-1) %>%
  mutate(LION.term = substr(x = LION.term, start = 9, stop = 1000000L)) %>%
  rename(lipid = LION.term) %>%
  mutate(lipid = str_trim(lipid, side = "both")) %>%
  filter(., lipid %in% (module15_lipids$lipid %>%
                          str_trim(side = "both"))) %>%
  unique(.) %>%
  remove_rownames() %>%
  column_to_rownames(., var = "lipid") %>%
  dplyr::select_if(colnames(.) %in% LIONtoTermmodule15$LION.term) %>%
  rename_with(~ paste0(.x, ".PhysModule15")) %>%
  rownames_to_column(., var = "lipid")
#Import table with lion terms as columns and lipids as rows. Remove first row, which has lion term names. Remove leading characters from lipids, rename column, and remove white space on both sides of the string. Filter for lipids in module, modified to remove white space. Remove repetitive rows and remove rownames. Assign new rownames using unique lipid names. Select columns if they match with lion terms in LIONtoTerm13v30. Convert rownames back to a column. Add suffix to all columns to delineate these are enriched phys pathways, not associated with temperature VIP.
module15Lipids_enrichedPhys #Confirm dataframe creation
```

I ended up dataframes for metabolomics and lipidomics data that highlighted enrichment for significant physiology modules, as well as overlaps between physiology and temperature information. Many of my physiology modules were also correlated with temperature, so it's good to have all of this information in one place! The goal is to look at one of these spreadsheets to guide any Google Scholar searches and discussion writing.

- [module 0 metabolomics output](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/07-integration-analysis/WCNA/metabolomics/module0_annotations.csv)
- [module 5 metabolomics output](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/07-integration-analysis/WCNA/metabolomics/module5_annotations.csv)
- [lipid output](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/07-integration-analysis/WCNA/lipidomics/all-phys-module-lipid-annotations.csv)

### Some interesting trends

For the metabolomics, I noticed that valine biosynthesis, valine metabolism, phenylalanine biosynthesis, and phenylalanine metabolism were all important for oxygen consumption! This is interesting because valine biosynthesis and phenylalanine metabolism were also important for distinguishing the 30ºC from either ther 13º or 5ºC treatments. I'll have to dig into how these pathways are related to oxygen consumption a bit further to clarify what's going on within the animal.

The lipidomics information, as expected, seemed primarily related to glycerophospholipids and bilayer thickness. There was also some interesting stuff with ceramindes that I'll need to learn more about.

In any case, it's nice to see this information coming together at a whole-animal system level!

### Going forward

1. Integrate physiology and -omics data with WCNA
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
