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



### Going forward

1. PLS-DA for temperature x time question
3. ASCA (include genotype as a random effect for temperature x time question)
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
