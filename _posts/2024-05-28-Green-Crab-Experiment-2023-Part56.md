---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 56
tags: green-crab-wc genotype TTR
---

## Finishing genotyping

I got my [sequences back](https://github.com/yaaminiv/wc-green-crab/tree/main/data/genotype)! I went through the sequences with Sequencher to call genotype. Most samples were good quality, but there were four (5-065, 5-075, 25-113, and 5-130) with garbage F reads. Luckily, I could still call genotype from the R reads. I followed [this protocol](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part25/). I also learned that once all individual sample contigs have been assembled into a larger contig, I can open this larger contig, select "View >> Sort/Clean Up >> By Name" to reorganize my samples. This is SUPER helpful because once organized into a large contig, the F and R reads from individual samples get separated (and are presented in a completely nonsensical order). My updated genotype data can be found [here](https://github.com/yaaminiv/wc-green-crab/blob/main/data/genotype/2023-crab-genotype.csv).

Once I had my genotype data, I returned to [my TTR script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/03-TTR-analysis.Rmd). I wanted to see if adding the final genotypes would change anything on the modeling side, or give me more insight to physiological responses. The first thing I did was use genotype as a variable in GLM:

```
modTTRGenotype <- modTTR %>%
  mutate(., presenceC = case_when(final.genotype == "CC" ~ "Y",
                                  final.genotype == "CT" ~ "Y",
                                  final.genotype == "TT" ~ "N")) %>%
  mutate(., presenceT = case_when(final.genotype == "CC" ~ "N",
                                  final.genotype == "CT" ~ "Y",
                                  final.genotype == "TT" ~ "Y")) #Subset data with genotype information. Add two columns for presence of C or T allele
```

```
TTRmodelSubset <- glm(log(TTRavg) ~ treatment + day + treatment*day + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + as.factor(final.genotype), data = modTTRGenotype) #ANOVA, with log(TTRavg) as the response variable, and treatment, day, the interaction between treatment and day, sex, integument color (continuous), carapace width, weight, whether or not a crab is missing legs, and genotype as explanatory variables. Cannot add presence of C or T allele into the model since they are colinear with genotype
summary(TTRmodelSubset) #Various factors significant
step(TTRmodelSubset, direction = "backward") #No genotype variable significant
```

```
TTRmodelSubset2 <- glm(log(TTRavg) ~ treatment + day + treatment*day + as.factor(sex) + integument.cont + carapace.width + weight + as.factor(missing.swimmer) + as.factor(presenceC) + as.factor(presenceT), data = modTTRGenotype) #ANOVA, with log(TTRavg) as the response variable, and treatment, day, the interaction between treatment and day, sex, integument color (continuous), carapace width, weight, whether or not a crab is missing legs, and presence of C or T allele
summary(TTRmodelSubset2) #Various factors significant
step(TTRmodelSubset2, direction = "backward") #No genotype variable significant
```

When looking at genotype (ex. CC, CT, or TT) or binary genotype (presence of C or T), there was no significant effect of genotype on average TTR. This made sense because there weren't crabs in any treatment that "stood out" in terms of TTR that I could suspect were due to genotype. With Julia's project, genotype seemed to impact change in TTR between temperatures. I decided to examine this statistically. I made a new dataframe with differences in TTR between day 0 and day 2, day 7, day 21, and day 42 for crabs that had data at least for days 0, 2, and 7:

```
modTTRSubsetDiffStats <- modTTRSubsetDiff %>%
  filter(., is.na(day0) == FALSE & is.na(day2) == FALSE & is.na(day7) == FALSE) %>%
  select(., -c(day0, day2, day7, day21, day42, day7v2, day21v2, day42v2)) %>%
  pivot_longer(., cols = c(day2v0, day7v0, day21v0, day42v0), names_to = "day", values_to = "TTRdiff") %>%
  mutate(., day = gsub(pattern = "day", replacement = "", x = day)) %>%
  mutate(., day = gsub(pattern = "v0", replacement = "", x = day)) %>%
  mutate(., day = as.numeric(day))
  #Take difference in TTR information and subset crabs with data for days 0, 2, and 7. Remove extraneous columns. Pivot data longer with values in TTRdiff and day in a new column. Remove extra characters from day values and convert into a numeric column
```

Then I ran GLM:

```
TTRdiffGLM <- glm(log(TTRdiff + 1) ~ final.genotype + treatment*day, data = modTTRSubsetDiffStats) #Run GLM to examine if difference in TTR from day 0 is impacted by genotype, treatment, or day
summary(TTRdiffGLM) #No effect of genotype
step(TTRdiffGLM, direction = "backward") #Genotype not included in most parsimonious model
```

```
TTRdiffGLM2 <- glm(log(TTRdiff + 1) ~ presenceC + presenceT + treatment*day, data = modTTRSubsetDiffStats) #Run GLM to examine if difference in TTR from day 0 is impacted by binary genotype, treatment, or day
summary(TTRdiffGLM) #No effect of genotype
step(TTRdiffGLM2, direction = "backward") #Genotype not included in most parsimonious model
```

No statistically significant impact of genotype! So, plotting it is. I made a plot of average TTR by treatment with different symbols for genotype for the 5ºC and 25ºC treatments. I wanted to see if a specific genotype was more commonly an outlier for a specific treatment.

![Screenshot 2024-05-28 at 3 00 53 PM](https://github.com/yaaminiv/wc-green-crab/assets/22335838/1d42ca8b-ea3e-4b9a-8ef4-ff2b6a36641c)

**Figure 1**. Average TTR by treatment and genotype.

It looks like CT crabs are more commonly outliers for 5ºC and 25ºC, but only TT crabs are outliers for 5ºC. I wonder how much of this is "real," seeing how the TT crabs are righting slower even though TT is the putatively cold-adapted genotype (maybe they're just REALLY cold adapted and are slowing down). I also don't have many CC crabs for the 5ºC treatment, so that could be skewing things! I'll have to think about this a bit more.

I also modified a plot with difference in TTR by treatment and genotype for days 7, 21, and 42. I think this plot does have interesting trends, but it also just shows where I'm at a loss for sample size.

![Screenshot 2024-05-28 at 3 01 11 PM](https://github.com/yaaminiv/wc-green-crab/assets/22335838/0bebb429-7eeb-4abf-b578-989e3459a077)

**Figure 2**. Difference in TTR by treatment and genotype

After sharing these plots with Carolyn and a lack of genotype effect, she did mention that it could underscore the importance of plasticity for this particular trait! My next step is to see if there was any genotype-specific impact on survival throughout the experiment, since I actually have individual-level data! My [previous work has shown me that genotype is in the final model (but it's not significant)](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part48/), so that's...something.

There's also the possibility for genotype x functional genomics interactions. Maybe the whole-organism physiology doesn't change between genotypes in a treatment, but perhaps there are genotype-specific differences in molecular pathways utilized to respond to temperature. So ANOTHER thing I need to do is finish sorting the mess of my lab notebook last summer to determine which samples to use for functional genomics!

### Going forward

1. Complete genotype-specific analysis
2. Clarify which crabs were actually sampled when and fix NAs in TTR data
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
