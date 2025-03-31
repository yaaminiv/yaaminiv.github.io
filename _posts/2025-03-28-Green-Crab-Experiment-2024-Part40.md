---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 40
tags: green-crab-cold mixed-effects-models glm
---

## Population comparison for MA and WA crabs

When going through Carolyn's feedback, she was wondering if I did any population comparisons. Aside from calculating Q10, I didn't do any formal comparison! When I looked at the Q10 data, I didn't think it really had any compelling information/added to the story. Instead, I decided to use a modeling approach to see if population had a significant impact on average TTR or failure to right. I conducted all of my analysis in [this script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/03-TTR-analysis-WA.Rmd).

### Average TTR

The first thing I did was import and format all the data. I initially thought I would subset timepoints 0, 2, and 5, but then decided to keep all timepoints because why not. I made sure there was a new population column so I could test this information in a model:

```
modTTRWA <- modTTR #Save dataframe with WA-specific tag.
modTTRWA$population <- "WA" #Add column with population information
```

```
rawTTRMA <- read.csv("../../../data/MA/time-to-right-MA-clean.csv", header = TRUE, na.strings = c("N/A", "NA")) #Import raw data

crabMetadataMA <- read.csv("../../../data/MA/crab-metadata-MA.csv", header = TRUE) %>%
  dplyr::select(., c("number", "tank", "carapace.width", "carapace.length")) %>%
  dplyr::rename(crab.ID = "number") #Import metadata. Select ID, treatment, width and length columns. Rename "ID" as "crab.ID"

genotypeDataMA <- read.csv("../../../data/MA/2024-crab-genotype-MA.csv", header = TRUE, na.strings = "") %>%
  select(., -c(extraction.date:gel.date, notes:QC.gen)) %>%
  mutate(., crab.ID = gsub(pattern = "MA-", replacement = "", x = ID)) %>%
  mutate(., crab.ID = as.numeric(crab.ID)) %>%
  mutate(., presenceC = case_when(genotype == "CC" ~ "Y",
                                  genotype == "CT" ~ "Y",
                                  genotype == "TT" ~ "N")) %>%
  mutate(., presenceT = case_when(genotype == "CC" ~ "N",
                                  genotype == "CT" ~ "Y",
                                  genotype == "TT" ~ "Y")) #Import genotype data. Remove unnecessary columns. Create a crab.ID column to match with other data and ensure it is numeric. Add columns for precense of C and T

modTTRMA <- rawTTRMA %>%
  dplyr::select(., -notes) %>%
  left_join(., crabMetadataMA, by = c("crab.ID", "tank", "carapace.width")) %>%
  filter(., is.na(trial.1) == FALSE) %>%
  mutate(., integument.cont = case_when(integument.color == "B/G" ~ 0.5,
                                        integument.color == "G" ~ 1.0,
                                        integument.color == "Y/G" ~ 1.5,
                                        integument.color == "Y" ~ 2.0,
                                        integument.color == "Y/O" ~ 2.5,
                                        integument.color == "O" ~ 3,
                                        integument.color == "R/O" ~ 3.5,
                                        integument.color == "R" ~ 4)) %>%
  mutate(., treatment = case_when(tank == "T01" ~ "0C",
                                  tank == "T02" ~ "0C",
                                  tank == "T03" ~ "0C",
                                  tank == "T05" ~ "5C",
                                  tank == "T06" ~ "5C",
                                  tank == "T07" ~ "5C")) %>%
  mutate(., missing.swimmer = case_when(missing.swimmer == "Y" ~ missing.swimmer,
                                        missing.swimmer == "N" ~ missing.swimmer,
                                        missing.swimmer == "Y " ~ "Y")) %>%
  mutate(., trial.1 = na_if(trial.1, 91.00)) %>%
  mutate(., trial.2 = na_if(trial.2, 91.00)) %>%
  mutate(., trial.3 = na_if(trial.3, 91.00)) %>%
  rowwise(.) %>%
  mutate(., TTRavg = mean(c(trial.1, trial.2, trial.3), na.rm = TRUE)) %>%
  mutate(., TTRSE = std.error(c(trial.1, trial.2, trial.3), na.rm = TRUE)) %>%
  left_join(x = ., y = (genotypeDataMA %>% dplyr::select(genotype:presenceT)), by = "crab.ID") %>%
  ungroup(.) %>%
  mutate(., TTRthresh = case_when(!(is.na(trial.1 & trial.2 & trial.3) == FALSE) ~ 0,
                                  is.na(trial.1 & trial.2 & trial.3) == FALSE ~ 1))
#Remove sampling ID and notes columns. Use case_when to create a column of continuous integument color metrics. Change all 91 to NA, calculate average and SE using data from the three TTR trials using rowwise operations. Ungroup. Join with genotype data. Create a new column where 1 = righted within 90 seconds in all trials and 0 = failure to right within that threshold. Subset data from hours 0, 22, and 94.
modTTRMA$population <- "MA" #Add column with population information
```

```
modTTRCombined <- rbind(modTTRMA, modTTRWA) %>%
  dplyr::select(., treatment, timepoint, crab.ID,
                TTRavg, TTRthresh,
                population,
                sex, integument.cont, carapace.width, weight, missing.swimmer, genotype, presenceC, presenceT) #Rbind data from both populations, and select columns of interest to use in the model
```

Then I created a linear mixed effects model to understand the impact of population on average TTR:

```
TTRCombinedNull <- lmer(log(TTRavg) ~ treatment + timepoint + treatment:timepoint +
                          as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                          (1|crab.ID),
                        data = (modTTRCombined %>% filter(., is.na(TTRavg) == FALSE)), REML = FALSE) #Null model without population
summary(TTRCombinedNull) #Crab ID doesn't explain any variation!
```

```
TTRPopulation <- lmer(log(TTRavg) ~ treatment + timepoint + treatment:timepoint +
                        as.factor(population) +
                        as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                        (1|crab.ID),
                      data = (modTTRCombined %>% filter(., is.na(TTRavg) == FALSE)), REML = FALSE) #Model with population
anova(TTRCombinedNull, TTRPopulation) #Population is not a significant predictor of avg TTR responses! Chisq = 0.466, p = 0.4948
```

Population was not a significant predictor of average TTR responses! This makes sense to me, since the responses to the cold and colder treatments in both populations weren't that different when the crab was able to right.

### Failure to right

I then wanted to do a similar thing with failure to right:

```
TTRthreshModelCombined <- glmer(I(TTRthresh == 1) ~ treatment + timepoint +
                                  as.factor(population) +
                                  as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                                  (1|crab.ID),
                                family = binomial(),
                                data = (modTTRCombined %>% filter(., is.na(genotype) == FALSE))) #Create a binomial model, where 1 = success of righting wihtin the defined threshold

TTRthreshNullPopulation <- glmer(I(TTRthresh == 1) ~ treatment + timepoint +
                                   as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) +
                                   (1|crab.ID),
                                 family = binomial(),
                                 data = (modTTRCombined %>% filter(., is.na(genotype) == FALSE))) #Create null model without treatment
anova(TTRthreshNullPopulation, TTRthreshModelCombined) #Population is not a factor in failure to right! Chisq = 0, p = 1
```

Well...this was fishy. There were many more crabs in MA that failed to right in the colder treatment in comparison to WA. I wasn't sure if having the full data set for the analysis was maybe masking some smaller effects. Since temperature and time significantly impacted failure to right in both populations, I decided to investigate the impact of population on failure to right at each timepoint in only the colder treatment. Since there weren't any repeated measurements in these models, I used GLM instead of mixed effects models.

```
TTRthreshModelCombined0 <- glm(I(TTRthresh == 1) ~ as.factor(population) +
                                 as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer),
                               family = binomial(),
                               data = (modTTRCombinedColder %>% filter(., timepoint == 0))) #Create a binomial model, where 1 = success of righting wihtin the defined threshold

TTRthreshModelCombined1 <- glm(I(TTRthresh == 1) ~ as.factor(population) +
                                 as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer),
                               family = binomial(),
                               data = (modTTRCombinedColder %>% filter(., timepoint == 1))) #Create a binomial model, where 1 = success of righting wihtin the defined threshold

TTRthreshModelCombined2 <- glm(I(TTRthresh == 1) ~ as.factor(population) +
                                 as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer),
                               family = binomial(),
                               data = (modTTRCombinedColder %>% filter(., timepoint == 2))) #Create a binomial model, where 1 = success of righting wihtin the defined threshold

TTRthreshModelCombined3 <- glm(I(TTRthresh == 1) ~ as.factor(population) +
                                 as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer),
                               family = binomial(),
                               data = (modTTRCombinedColder %>% filter(., timepoint == 3))) #Create a binomial model, where 1 = success of righting wihtin the defined threshold

TTRthreshModelCombined4 <- glm(I(TTRthresh == 1) ~ as.factor(population) +
                                 as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer),
                               family = binomial(),
                               data = (modTTRCombinedColder %>% filter(., timepoint == 4))) #Create a binomial model, where 1 = success of righting wihtin the defined threshold

TTRthreshModelCombined5 <- glm(I(TTRthresh == 1) ~ as.factor(population) +
                                 as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer),
                               family = binomial(),
                               data = (modTTRCombinedColder %>% filter(., timepoint == 5))) #Create a binomial model, where 1 = success of righting wihtin the defined threshold

rbind(broom::tidy(TTRthreshModelCombined0) %>%
        mutate(timepoint = 0),
      broom::tidy(TTRthreshModelCombined1) %>%
        mutate(timepoint = 1),
      broom::tidy(TTRthreshModelCombined2) %>%
        mutate(timepoint = 2),
      broom::tidy(TTRthreshModelCombined3) %>%
        mutate(timepoint = 3),
      broom::tidy(TTRthreshModelCombined4) %>%
        mutate(timepoint = 4),
      broom::tidy(TTRthreshModelCombined5) %>%
        mutate(timepoint = 5)) %>%
  filter(., term == "as.factor(population)WA") %>%
  mutate(., p.adj = p.adjust(p.value, method = "bonferroni")) %>%
  write.csv("population-comparison-TTRthresh-stats.csv", quote = FALSE, row.names = FALSE) #Combine tidy output from GLMs. Filter out population comparison statistical information and correct for multiple comparisons. Save output as a .csv
```

The output for these GLM are saved [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/population-comparison-TTRthresh-stats.csv). Prior to adjustment, population had a significant effect on failure to right at timepoints 2-4, and a marginal impact at the final timepoint. After p-value correction, population only had a marginal effect on failure to right at timepoint 2.

I'll update the paper with this information! I like this approach more than the [Q10 approach](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part39/).

### Going forward

1. Re-analyze Catlin's TTR data
5. Analyze Catlin's HOBO data
1. Add Catlin's methods and results to manuscript
6. Refine temperature analysis and revise methods and results
2. Outline introduction
3. Outline discussion
4. Write introduction
5. Write discussion
3. Troubleshoot lipid assay protocol
5. Conduct lipid assay for crabs of interest

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
