---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 78
tags: green-crab-wc metabolomics PLSDA ASCA sPLS
---

## Early vs. late timepoints

My [PLSDA](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part75/) and [ASCA](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part77/) analyses showed a strong separation between early and late timepoints for metabolomes at both temperatures. Based on this, I decided to go back and repeat the PCA, perMANOVA, ASCA, and PLSDA analyses using the early vs. late distinction.

- PCAs found [here](https://github.com/yaaminiv/wc-green-crab/tree/main/output/05-metabolomics-analysis/PCA/figures)
- perMANOVA results [here](https://github.com/yaaminiv/wc-green-crab/blob/main/output/05-metabolomics-analysis/PCA/known-metabolites-PERMANOVA-temperaturTimepoint.csv), beta dispersion test results [here](https://github.com/yaaminiv/wc-green-crab/blob/main/output/05-metabolomics-analysis/PCA/known-metabolites-PERMDISP-temperatureTimepoint.csv)
- ASCA figures [here](https://github.com/yaaminiv/wc-green-crab/tree/main/output/05-metabolomics-analysis/ASCA/figures), and loadings [here](https://github.com/yaaminiv/wc-green-crab/tree/main/output/05-metabolomics-analysis/ASCA)
- PLSDAs [here](https://github.com/yaaminiv/wc-green-crab/tree/main/output/05-metabolomics-analysis/PLSDA/figures)

Overall, I see some good separation between early and late timepoints (which is what I expected). Redoing all of these things was just due diligence. Having the PLS-DA split by early and late timepoints also allows me to pull out some higher-order VIPs. I will spend some time talking about the ASCA output because I think that is the most different.

<img width="1620" height="1244" alt="Image" src="https://github.com/user-attachments/assets/f1875e19-11f2-4825-a177-c755ef36838a" />

<img width="1612" height="1243" alt="Image" src="https://github.com/user-attachments/assets/f8c47fb9-c785-474d-8d00-b30b4d3c8aae" />

<img width="1613" height="1244" alt="Image" src="https://github.com/user-attachments/assets/384ded3b-34b8-4c03-8589-85f74de56681" />

**Figures 1-3**. Effect plots for significant PCs from timepoint ASCA

PC 1 shows metabolites with higher abundance at 25ºC, but that decrease over time across both temperatures. This is pretty consistent with what I saw when considering all timepoints! PC 2 shows metabolites with higher abundance at 25ºC that increase over time across both temperatures. The slope, however, is steeper for 25ºC than 5ºC. This is actually much cleaner to interpret when compared to the ASCA done with all timepoints, particularly at 5ºC. The loadings are also stronger (no overlap with 0). PC 3 shows metabolites that increase over time at 5ºC but decrease over time at 25ºC. Even though the loadings aren't as strong at PC 3 barely explains > 10% of the variation, I think there are some interesting potential trends here.


### Going forward

1. VIP analysis for major trends
1. Metaboanalyst for VIP and ASCA compounds
3. Index, get advanced transcriptome statistics, and pseudoalign with `salmon`
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
