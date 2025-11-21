---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 75
tags: green-crab-wc metabolomics
---

## Supervised analyses

### Genotype-specific beta-dispersion tests

After talking with Ariana about my [genotype PCAs](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part73/), she said that it was pretty common to do a beta-dispersion test even if the perMANOVA is not significant. The perMANOVA is merely concered with centroid differences, but there can be dispersion differences without significant centroid differences, which is where the beta-dispersion test comes into play. All of the statistical output can be found [here](https://github.com/yaaminiv/wc-green-crab/tree/main/output/05-metabolomics-analysis/PCA).

Surprisingly, there were no significant differences in dispersion by any genotype factor (genotype, allele presence, heterozygote vs. homozygote) for 5ºC crabs. I think the outlier really shifted my perception of dispersion, but the outlier doesn't impact the significance of the test. I did, however, find a significant impact of the T allele on dispersion at 25ºC.

**Figure 1**. Presence of the T allele in 25ºC crabs

This effect was only marginally significant (p = 0.056) after multiple-test correction. I did the multiple test correction for the genotype perMANOVAs and dispersion tests because I was testing different genotype factors on the same dataset and I wanted to be conservative. I also did this for the temperature x time dispersion tests since again, I was testing multiple factors on the same dataset.

Since I did find a marginally significant effect of genotype in these PCAs, I think it's worth including genotype in the PLS-DAs. I found the significant effect when looking at temperatures individually, so I'll continue to do that for the PLS-DAs. 

### PLS-DAs

I first made a PLS-DA for the temperature x time question. When I wanted to tune the PLS-DA to determine the number of components, I actually ran into an error:

> Error: BiocParallel errors
1 remote errors, element index: 1
2 unevaluated and other errors
first remote error:
Error: TridiagEigen: eigen decomposition failed

Alas, I couldn't figure out what this error meant so I had to ask Gemini. The `eigen decomposition failed` means that there's an issue with the underlying structure of the dataset such that it can't run the analysis. One common reason for this is having columns with near-zero variance, so it recommended I use the `caret` package to check for any columns with near-zero variance:

```
nzv_indices <- nearZeroVar(X_tempTime) #Check to see if any variables have near-zero variance.
nzv_indices$Position #There aren't any columns with near-zero variance!
```

I actually didn't have any metabolites with near-zero variance so that wasn't the issue! My next thought was that I actually had some `-Inf` values in my dataframe which would have occured if I took the log of 0. Sure enough, when I dug into my dataset this was the issue. I then modified my log-transformation code to correct these values:

```
X_tempTime <- transMetabData %>% 
  filter(., day != "0") %>%
  dplyr::select(c(14:198)) %>%
  log(.) %>% 
  mutate_if(is.numeric, list(~na_if(., -Inf))) %>%
  replace(is.na(.), 0) #Assign known metabolite data to the X dataframe after log transformation. Replace any -Inf with NA, then replace NA with 0
```

Once I fixed this issue, I was good to go to tune the PLS-DA! I then ran the PLS-DA with the correct number of components. Looking at the PLS-DA, It was interesting to see that there was much stronger separation between early (days 1 and 7) and late (days 26 and 42) timepoints. These patterns were consistent when I did the sPLSDA with the optimal number of variables per component. This suggests that metabolic acclimation occurs within a month, and these temperatures weren't stressful enough to cause changes over the later two weeks of the exposure. Depending on the results of my other tests, I may consider lumping my early and late timepoints for the analysis.

<img width="1614" height="1239" alt="Image" src="https://github.com/user-attachments/assets/f7dfb13a-65c2-4cec-b299-6c9649cd5191" />

**Figure 1**. Temperature x time PLSDA

Next up, I wanted to do a PLSDA for each temperature separately. My decision to investigate each temperature separately is based on the fact that I did find a marginally significant impact of T allele presence on dispersion at 25ºC, and because I think the alleles do different things at different temperatures. The downside to this approach is that I have a unbalanced sample sizes, with 4-6 samples for the "mismatched" homozygote (CC at 25ºC or TT at 5ºC). This is something that I'll need to consider when building the story. It may be worth doing a temperature x genotype PLS-DA with both 5ºC and 25ºC, but I can see if that makes sense later.

When doing the PLS-DA for 5ºC, I noticed that the separation between crabs with and without a C allele wasn't very strong. While the centroids were different, there was a decent amount of overlap between the 95% confidence ellipses. This was not the case for the crabs with and without a T allele at 25ºC: the 95% confidence ellipses do not overlap at all.

<img width="1610" height="1238" alt="Image" src="https://github.com/user-attachments/assets/7d5f71d6-aca6-4b3f-bdc1-709d39a39121" />

**Figure 2**. Presence of the C allele at 5ºC.

<img width="1616" height="1238" alt="Image" src="https://github.com/user-attachments/assets/762124ad-3c67-4190-9d77-35041ad19bac" />

**Figure 3**. Presence of the T allele at 25ºC

Interestingly, when I was tuning the PLS-DA for both of these comparisons, I noticed that the optimal number of components identified was 1. However, I need two components to do the analysis. This confirms that genotype is not causing an outsized effect on the metabolome. I think this makes sense considering that the genotype of interest is just a section of the genome. It would be interesting to dig into the genes annotated in this inversion region and then look at a targeted metabolome associated with those genes, but on a global scale, the lack of difference isn't suprising. I think grouping metabolites together in an ASCA or doing some VIP comparisons may tease out things that are actually changing between these genotypes.

### Going forward

1. ASCA (include genotype as a random effect for temperature x time question)
2. VIP identification for interesting trends from PLS-DA and ASCA
3. Transcriptome assembly with `trinity`
4. Clean transcriptome with `EnTap` and `blastn`
5. Quantify transcript counts with `salmon`
4. Identify differentially expressed genes
5. Additional strand-specific analysis in the supergene region
2. Examine HOBO data from 2023 experiment
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
