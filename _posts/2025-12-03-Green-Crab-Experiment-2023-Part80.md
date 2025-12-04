---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 80
tags: green-crab-wc metabolomics metaboanalyst
---

## Enrichment for metabolites of interest

Final piece of this preliminary metabolomics analysis is to do some enrichment! I mainly followed methods I'd already worked through before, with a few tweaks that I'll mention below. Based on my [previous analysis](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part79/), I will be performing enrichment on metabolite lists from the following comparisons:

- Temperature x time ASCA components 1-3
- All pairwise temperature x time VIP comparisons
- Temperature x genotype ASCA components 1-3

### Temperature x time ASCA

The first thing I wanted to do was identify a list of metabolites in a quantitative fashion. Previously, I just took the top 20 strongly-loaded metabolites, meaning I had 10 with strong positive loadings and 10 with strong negative loadings. Ariana found a way to determine the cumulative variance explained by each metabolite, and establish a cutoff. I simplified [her code](https://github.com/AHuffmyer/larval_symbiont_TPC/blob/main/scripts/lipids_ASCA.Rmd) to meet my needs:

```
tempTimepoint.loadingsPC1[[1]] %>%
  dplyr::rename(metabolite = covars) %>%
  mutate(abs_loading = abs(loading)) %>%
  arrange(desc(abs_loading)) %>%
  mutate(rank = 1:nrow(.),
         cum_abs = cumsum(abs_loading),
         var_contrib = loading^2,
         cum_var = cumsum(var_contrib) / sum(var_contrib)) %>%
  filter(., cum_var <= 0.50) %>%
  left_join(., (rawMetabolomicsData %>%
                  dplyr::rename(metabolite = BinBase.name)),
            by = "metabolite") %>%
  dplyr::select(., PC, metabolite, loading, cum_var, PubChem, KEGG) %>%
  unique(.) %>%
  write.table("PLSDA/tempTimepoint-PC1_cumvar50_metabolites.txt", sep = "\t", quote = FALSE, row.names = FALSE) #Save list of metabolites to use with Metaboanalyst
```

The list of metabolites can be found [here](https://github.com/yaaminiv/wc-green-crab/tree/main/output/05-metabolomics-analysis/ASCA). I then used the metabolites as input with Metaboanalyst (which seems to have been updated Dec 1!). All Metaboanalyst can be found [here](https://github.com/yaaminiv/wc-green-crab/tree/main/output/05-metabolomics-analysis/metaboanalyst). For each metabolite list, I had a plof the pathway enrichment results, one showing the significant log FDR pathways, and then the abundance of metabolites in any significantly enriched pathways. There are way too many figures, so I'll just focus on the top-level enrichment highlights.

Only PC 1 had any significant pathways! Starch and sucrose metabolism was significantly enriched. This is interesting, since starch and sucrose metabolism was not significantly enriched in my previous experiment.

### Temperature x time VIPs

When it rains it pours.......starch and sucrose metabolism was significant for EVERY COMPARISON EXCEPT FOR EARLY VS. LATE AT 5ºC! Interestingly, some of these comparisons had different metabolites involved in this enriched pathway. I'm sure a composite image is in my future, but not today. Some other enriched pathways:

Early 5ºC vs. Early 25ºC: Arginine biosynthesis
Late 5ºC vs. Late 25ºC: Glutathione metabolism
Early 5ºC vs. Early 25ºC: Taurine and hypotaurine metabolism

### Temperature x genotype ASCA

PCs 1 and 2 had significantly enriched pathways. Glutathione metabolism was enriched in PC 1, and arginine biosynthesis was enriched in PC 2. It's interesting to see some consistencies between the temperature x time comparisons and temperature x genotype comparisons. It's also interesting to see arginine biosynthesis and glutathione metabolism pop up again, since those were pathways that were also enriched in the 2022 experiment.

All that's left to do is the RNA-Seq analysis....and integrative analysis....and demographic analysis....and temperature analysis.....and write. But I'm feeling good about these metabolomics methods and results!

### Going forward

1. Index, get advanced transcriptome statistics, and pseudoalign with `salmon`
4. Clean transcriptome with `EnTap` and `blastn`
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
