---
layout: post
comments: true
title: Green Crab Experiment Part 54
tags: green-crab glmm
editor_options:
  markdown:
    wrap: 72
---

## Modifying the oxygen consumption analysis

I wanted to modify the oxygen consumption analysis to also include a random effect for treatment, since there were some crabs that were likely assayed across the experiment. I followed the workflow I established [with my TTR model revision](https://yaaminiv.github.io/Green-Crab-Experiment-Part52/). A very brief digest of what I did:

- Confirmed that I could [run all my code](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/04-respirometry-analysis.Rmd) before any modeling was done with this new version of R and R Studio
  - There were some things that required tweaking (specifically, adding package information for some `broom` commands), but she runs!
- Used a Gamma model with a log link and added tank as a random effect
  - I used a Gamma model with a log link to match the previous log-transformation I did with the model. I checked this with Gemini. I wish I still had my Skalski notes.
- There were no assumption violations with my final model!
  - Well, it seemed like there was some minor quantile deviations, but the p-value was 0.0455.....so a rounding error that could be reflective of a smaller sample size
  - I tried modeling dispersion by temperature, day, and the two of them additively, but that did not improve model AIC values or change the quantile deviations. Additionally, there were no significant differences in dispersion by temperataure
  - I tried using a log normal or tweedie family model, but that led to higher AIC scores
  - So, a Gamma model with log link it is!
- My final model had significant impacts of temperature and day, but no interaction
  - This is consistent with my previous model
- The random effect was negligible
- I updated my methods and results accordingly with the model output and revised post-hoc test information
- I made a new figure with average oxygen consumption to match the figure I made for TTR
  - I only had marginal means for temperature and day separately since those were the significant predictors in the model, so I wasn't sure if the most appropriate choice would be to have a two panel plot with EMMs for temperature and EMMs for day
  - I settled for having a plot that showed temperature and day at the same time, but calculating my own means and standard errors (code below)
  - I also added an inset for 0-4,000 nmol/hr to show the cold data better!
- One of the reviewers mentioned that seeing raw oxygen values over time would be worthwile, but anothe reviewer mentioned that there is too much supplementary material. I will deal with this later.

```
all_MO2_slope_results_reorder <- all_MO2_slope_results %>%
  mutate(., treatment = gsub(x = treatment, pattern = "5C", replacement = "05C")) %>% #Reorder temperature treatments for plotting by renaming 5C as 05C so it is the first alphanumerically
  group_by(., day, treatment) %>% #Group by day and treatment
  mutate(., OCavgFull = -mean(slope_nmol_hr_corrected)) %>% #Average OC rates by day and treatment, then take the negative of that to get positive rates
  mutate(., OCSEFull = std.error(slope_nmol_hr_corrected)) %>% #Calculate SE for avg OC
  mutate(., OCavgFullLow = OCavgFull - OCSEFull) %>% #Lower bound
  mutate(., OCavgFullHigh = OCavgFull + OCSEFull) %>% #Upper bound
  ungroup(.) #Ungroup
```

<img width="1528" height="1162" alt="Image" src="https://github.com/user-attachments/assets/a6239f03-32f0-472a-888d-eefeeec282a5" />

**Figure 1**. Revised oxygen consumption rate plot

### Going forward

1. Address remaining methods comments
2. Address results comments
3. Address figure comments
4. Address discussion comments
5. Modify supplementary material section
5. Send revised manuscript to co-authors

{% if page.comments %}

::: {#disqus_thread}
:::

```{=html}
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
```

<noscript>Please enable JavaScript to view the
<a href="https://disqus.com/?ref_noscript">comments powered by
Disqus.</a></noscript>

{% endif %}

```{=html}
<script id="dsq-count-scr" src="//the-responsible-grad-student.disqus.com/count.js" async></script>
```
