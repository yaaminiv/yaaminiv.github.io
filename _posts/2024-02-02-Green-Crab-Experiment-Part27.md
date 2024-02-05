---
layout: post
comments: true
title: Green Crab Experiment Part 27
tags: green-crab respirometry
---

## GLM for respirometry data

[I used a GLM for the TTR data](https://yaaminiv.github.io/Green-Crab-Experiment-Part24/) which got me thinking...why not also use a GLM for the respirometry data? I returned to [this R Markdown notebook](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/04-respirometry-analysis.Rmd) and revised my code. First, I recoded integument color as a continuous variable:

```
demTTRdata <- read.csv("../../data/time-to-right.csv", header = TRUE) %>%
  dplyr::select(., -c(sampling.ID, notes)) %>%
  filter(., is.na(probe.number) == FALSE) %>%
  dplyr::rename(., probe_num = probe.number) %>%
  mutate(., day = case_when(date == "7/8/2022" ~ "04",
                            date == "7/12/2022" ~ "08",
                            date == "7/15/2022" ~ "11",
                            date == "7/19/2022" ~ "15",
                            date == "7/22/2022" ~ "18",
                            date == "7/26/2022" ~ "22")) %>%
  filter(., day != "11" & day!= "18") %>%
  mutate(., treatment = case_when(treatment.tank == "1" | treatment.tank == "4" ~ "13C",
                                  treatment.tank == "2" | treatment.tank == "5" ~ "5C",
                                  treatment.tank == "3" | treatment.tank == "6" ~ "30C")) %>%
  dplyr::rename(., tank = treatment.tank) %>%
  mutate(., integument.cont = recode(integument.color,
                                     "B" = 0,
                                     "BG" = 0.5,
                                     "G" = 1,
                                     "YG" = 1.5,
                                     "Y" = 2,
                                     "YO" = 2.5 ,
                                     "O" = 3,
                                     "RO" = 3.5)) %>%
  mutate(., missing.legs = case_when(number.legs < 10 ~ "Y",
                                     number.legs == 10 ~ "N")) %>%
  select(., c(day, treatment, tank, probe_num, sex, integument.cont, weight, carapace.width, missing.legs)) #Import TTR data and modify to include only the desired demographic data and average TTR data for crabs used during respiration trials
```

Then I ran a GLM:

```
lmAllSlopeResults <- glm(log(-slope_nmol_hr_corrected) ~ treatment*as.numeric(day) + sex + weight + carapace.width + integument.cont + missing.legs, data = (all_MO2_slope_results_demTTRdata %>% filter(., blank_ratio < 0.10))) #Examine the influence of treatment, day, and various demographic factors on respiration rate. Remove outliers based on blank ratios prior to analysis.
lmAllSlopeResultsStep <- step(lmAllSlopeResults, direction = "backward") #Use backwards-deletion approach to identify the most significant model (lowest AIC)
summary(lmAllSlopeResultsStep)
```

```{r}
lmAllSlopeResultsSig <- glm(log(-slope_nmol_hr_corrected) ~ treatment + as.numeric(day), data = all_MO2_slope_results_demTTRdata) #Run the most significant model identified by step
summary(lmAllSlopeResultsSig) #Significant impact of treatment and day (not not day x treatment)
```

My results didn't change since I still have a significant impact of treatment and day! But I think that these results are easier to understand than the ANOVA results. I saved them [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/04-respirometry-analysis/all-slope-results-model-output.csv).

So I ran a GLM...but I don't know if the way I set up the GLM is the most appropriate way to analyze the data. Should some variables be included as random vs. fixed effects? I need to find someone with more modeling expertise to provide advice or some sort of "here's how you determine the best way to analyze your data" primer.

### Going forward

1. Evaluate statistical methods
2. Repeat lipidomics WCNA with temperature and day separately
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
