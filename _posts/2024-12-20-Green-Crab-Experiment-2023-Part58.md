---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 58
tags: green-crab-wc genotype TTR mixed-effects-models
---

## Re-analyzing Julia's data

I'm exploring the potential for genotype to be linked to short-term thermal plasticity in green crab, so I figured I'd re-analyze some of Julia's data to explore those possibilities further! All my analysis was done in [this R Markdown script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/03-TTR-analysis-julia.Rmd).

### Genotype information

I figured I'd start by examining the genotypes of the crabs Julia used. She intended to have a balanced design with the three genotypes, but she did have a whole tank of crabs die which may have skewed things.

<img width="1085" alt="Screenshot 2024-12-20 at 3 37 53 PM" src="https://github.com/user-attachments/assets/e4cc58df-8e11-461b-8374-998e3b338eac" />

**Figure 1**. Genotype distribution for Julia's experiment

The genotypes are in Hardy-Weinberg equilibrium!

### Applying a mixed effects model

My next step was to examine differences in average TTR using a mixed effects model to account for crab as a random effect. As expected, I found that treatment, time, and their interaction had a significant impact on righting response! I also found that the presence of the T allele had a marginally significant impact on average TTR! The output can be found [here](https://github.com/yaaminiv/wc-green-crab/blob/main/output/03-TTR-analysis-julia/ttr-model-comparison-stat-output.csv). Of course I made some figures to try and reflect these results:

<img width="1332" alt="Screenshot 2024-12-20 at 3 37 00 PM" src="https://github.com/user-attachments/assets/09ce3f36-af09-42c0-b5c9-cbfc540e8500" />

<img width="1332" alt="Screenshot 2024-12-20 at 3 37 21 PM" src="https://github.com/user-attachments/assets/de1a8ba6-82ea-4a94-a2dd-80bca97f11c5" />

**Figures 2-3**. Average TTR figures

First and foremost, the raincloud plot is ugly. It really doesn't work with this formatting, so that's unfortunate. The boxplots look clean though! It's apparent that having a T allele increases your average TTR.

### Examining differences in TTR

I reformatted the data to be in a wide format to calculate differences in TTR between the initial and final timepoints. I also added the demographic data from the final timepoint:

```
modTTRWide <- modTTR %>%
  select(-c(date, trial.1:trial.3, integument.color:carapace.length, integument.cont, TTRSE:TTRavgFullHigh)) %>%
  pivot_wider(., names_from = "day", names_prefix = "day", values_from = "TTRavg") %>%
  select(., -c(day2:day3)) %>%
  mutate(., TTRdiff = day5 - day1) %>%
  left_join(x = .,
            y = (modTTR %>%
                   filter(., day == 5) %>%
                   select(., crab.ID, integument.cont, weight, carapace.width, missing.swimmer)),
            by = "crab.ID") #Take data and remove extraneous columns. Pivot dataframe and remove intermediate timepoints to have day 1 (before the experiment) and day 5 (after every crab experienced the 24 hr pulse). Calculate the difference in TTR between timepoints. Add demographic information from the final sampling point back to the dataframe.
```

Since I didn't have multiple timepoints for each individual (just one value for `TTRdiff` per crab), I used a standard linear model with step to identify the best-fit model. Interestingly, treatment did not have a significant impact on the difference in TTR. Unsurprisingly, neither did any genotype factor. I made some figures that included treatment and genotype, as well as other significant factors from the model, in order to understand the patterns.

<img width="1345" alt="Screenshot 2024-12-20 at 3 39 04 PM" src="https://github.com/user-attachments/assets/a9e1938e-cebc-4047-bebd-bfea7db1a60a" />

<img width="1345" alt="Screenshot 2024-12-20 at 3 39 43 PM" src="https://github.com/user-attachments/assets/3426b34a-0711-46e2-85dd-26a317efa754" />

<img width="1345" alt="Screenshot 2024-12-20 at 3 40 10 PM" src="https://github.com/user-attachments/assets/cfa25896-c6f1-4538-a86f-9042e4a97629" />

<img width="1345" alt="Screenshot 2024-12-20 at 3 40 24 PM" src="https://github.com/user-attachments/assets/d719d459-27d9-468e-b312-818c9e3f6243" />

**Figures 4-7**. Differences in TTR by treatment, genotype, and other factors

Alright, I think a story is somewhat coming together regarding how genotype may or may not be responsible for short-term thermal responses! I'll have to massage it out a bit more in a talk outline before I get cranking.

### Going forward

1. Clarify which crabs were actually sampled when and fix NAs in TTR data
5. Identify samples for gene expression and metabolomics
4. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper
6. Start methods and results of 2023 paper

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
