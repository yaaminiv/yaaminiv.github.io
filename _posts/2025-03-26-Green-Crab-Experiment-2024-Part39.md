---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 39
tags: green-crab-cold TTR mixed-effects-models
---

## Revising modeling for 2024 and Julia experiments

I'm addressing feedback from Carolyn on the paper! She asked for some more detail in the methods about the modeling approach, so I revisited [this tutorial](https://jontalle.web.engr.illinois.edu/MISC/lme4/bw_LME_tutorial.pdf) and [code from my JSR paper](https://github.com/RobertsLab/paper-gigas-early-gametogenic-exposure/blob/master/analyses/Larval-Abundance/2018-02-14-Larval-Abundance.R). I realized I needed to revist my scripts to 1) calculate the variance explained by crab ID; 2) correct the ANOVA output for multiple comparisons; and 3) calculate average weight and carapace widths for the populations. For the MA and WA data, I'm also going to clean up some of the Q10 analysis.

### Julia's experiment

I conducted all of my analysis in [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/repeat-heat/03-TTR-analysis-julia.Rmd). I first calculated average width and weight for the crabs. I tried to do this using `summarize` but for some reason the standard deviation for weight wouldn't calculate! All of the values were numerical (mean was calculated just fine) and there were no missing values (I even included `na.rm = TRUE` just in case). Decided to cut my losses and just calculate things in separate steps:

```
modTTR[1:24]$carapace.width %>% mean(., na.rm = TRUE) #46.83333
modTTR[1:24]$carapace.width %>% sd(., na.rm = TRUE) #5.505439

modTTR[1:24]$weight %>% mean(., na.rm = TRUE) #24.17931
modTTR[1:24]$weight %>% sd(., na.rm = TRUE) #8.280845
```

Next I set out to re-run the models. I used the following code to calculate how much variance individual crab explained (s/o to my old paper code for helping me figure this one out!):

```
var.lmer <- c(0.02054, 0.22874) #Save variances of random effects as a new vector (crab ID, residuals)
percentvar.lmer <- (100*var.lmer)/sum(var.lmer) #Calculate percent variances
percentvar.lmer[1] #Crab ID accounts for 8.23973% of variance
```

Then, I performed a Bonferroni calculation on the ANOVA p-values:

```
TTRModelComparisons %>%
  mutate(., p.adj = p.adjust(p.value, method = "bonferroni")) #Perform bonferroni adjustment for multiple comparisons
```

The table can be found [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/repeat-heat/03-TTR-analysis/ttr-model-comparison-stat-output.csv). My marginally significant effect of the T allele on average righting response is no longer significant. I think I'll still include the genotype panel in the figure because there is an interesting trend.

### MA and WA

I performed similar steps in [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/MA/03-TTR-analysis-MA.Rmd) for MA and [this script for WA](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/03-TTR-analysis-WA.Rmd). These scripts were able to use `summarize` to calculate mean and SD...so who knows what was happening with Julia's data. I noticed that my binomial models were not converging so there was no variance information for residuals. I'll see if I can troubleshoot this more once I have a full working draft of the paper. Something about rescaling I don't understand.

The revised data tables can be found at the following links:

- [MA average TTR](https://github.com/yaaminiv/cold-green-crab/blob/main/output/MA/03-TTR-analysis/ttr-model-comparison-stat-output.csv)
- [MA TTR threshold](https://github.com/yaaminiv/cold-green-crab/blob/main/output/MA/03-TTR-analysis/ttr-binomial-model-comparison-output.csv)
- [WA average TTR](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/ttr-model-comparison-stat-output.csv)
- [WA TTR threshold](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/ttr-binomial-model-comparison-output.csv)

Since I have water temperature information, I decided to update a few things. First, I updated all my figures with the average temperature! This means that MA figures will have 1.7 or 5.3, and WA figures will have 2.1 or 5.2. I also revisited the Q10 analysis. Carolyn cautioned me against calling it a Q10 in the manuscript since they are usually calculated with actual rates as output and not TTR, but I figured I could use an analogous approach to understand differences in acclimation by population. I had 21 crabs for the MA data and 28 crabs for the WA data that I could track throughout the experiment. For these crabs, I calculated Q10 using the average temperature conditions actually experienced, and not just the target conditions:

```
modTTRQ10 <- modTTR %>%
  select(., timepoint, crab.ID, TTRavg, treatment, presenceT) %>%
  group_by(., crab.ID) %>%
  pivot_wider(., names_from = timepoint, values_from = TTRavg, names_prefix = "timepoint") %>%
  mutate_all(~ifelse(is.nan(.), 91, .)) %>%
  filter(., is.na(timepoint0) == FALSE) %>%
  filter(., is.na(timepoint1) == FALSE) %>%
  filter(., is.na(timepoint2) == FALSE) %>%
  filter(., is.na(timepoint3) == FALSE) %>%
  filter(., is.na(timepoint4) == FALSE) %>%
  filter(., is.na(timepoint5) == FALSE) %>%
  mutate(temperature = case_when(treatment == "0C" ~ 1.56,
                                 treatment == "5C" ~ 5.39)) %>%
  arrange(., temperature) %>%
  mutate(., timepoint1_Q10 = Q10(R2 = timepoint0,
                                 R1 = timepoint1,
                                 T2 = 15,
                                 T1 = temperature)) %>%
  mutate(., timepoint2_Q10 = Q10(R2 = timepoint0,
                                 R1 = timepoint2,
                                 T2 = 15,
                                 T1 = temperature)) %>%
  mutate(., timepoint3_Q10 = Q10(R2 = timepoint0,
                                 R1 = timepoint3,
                                 T2 = 15,
                                 T1 = temperature)) %>%
  mutate(., timepoint4_Q10 = Q10(R2 = timepoint0,
                                 R1 = timepoint4,
                                 T2 = 15,
                                 T1 = temperature)) %>%
  mutate(., timepoint5_Q10 = Q10(R2 = timepoint0,
                                 R1 = timepoint5,
                                 T2 = 15,
                                 T1 = temperature)) %>%
  select(., crab.ID, treatment, presenceT,
         timepoint1_Q10, timepoint2_Q10, timepoint3_Q10, timepoint4_Q10, timepoint5_Q10) %>%
  pivot_longer(., cols = !c(crab.ID, treatment, presenceT), names_to = "timepoint", values_to = "Q10") %>%
  mutate(., timepoint = gsub(x = timepoint, pattern = "_Q10", replacement = "")) %>%
  mutate(., timepoint = gsub(x = timepoint, pattern = "timepoint", replacement = "")) %>%
  mutate(., timepoint = as.numeric(timepoint))
#Select columns of interest. Add columns for presence of C or T alleles. Pivot wider. Change NaN to 91 and remove any NA measurements. Add a column for temperature based on the treatment column, and arrange data by temperature. Use the Q10 function within the respirometry package to calculate Q10 values for timepoint 0 vs. all other timepoints. Select columns of interest and pivot longer. Use a series of gsub to modify timepoint column and convert to a character format that would be organized numerically. Use a similar approach for treatment.
```

I used this dataframe to update figures. I saved the wider version of this table (aka what I had before `pivot_longer`) so I could refer to numbers as needed (MA values [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/MA/03-TTR-analysis/Q10-calculations.csv) and WA values [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/Q10-calculations.csv)). I'll dig through the figures and the tables and think about how to present this information as a way to compare responses across populations.

### Going forward

1. Genotype Catlin's samples
2. Re-analyze Catlin's TTR data
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
