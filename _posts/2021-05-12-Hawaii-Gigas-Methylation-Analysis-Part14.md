---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 14
tags: hawaii gigas-ploidy methylKit
---

## Messing around with `methylKit` DML settings

### The R Studio server works!

[Last time](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part13/) I mentioned how the R Studio server wasn't playing well with my R Markdown script, and that I started running an [R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit.R) for the analysis instead. Turns out making the switch is what I needed to run my code! When I was working on the [Manchester analysis](https://yaaminiv.github.io/WGBS-Analysis-Part23/), I did end up using an R script instead of an R Markdown file. This makes me think that there's some sort of issue using R Markdown on R Studio server, specifically with `calculateDiffMeth`. Something to investigate at a later date.

### Evaluating `methylKit` DML output

After `unite`, I had 1,984,530 CpG loci with data in all 24 samples. Using this object with `calculateDiffMeth` (including covariates and an overdispersion correction), I didn't obtain many ploidy- or pH-DML:

**Table 1**. Number of DML obtained from various methylation difference thresholds.

| **Contrast** | **25%** | **50%** | **75%** |
|:------------:|:-------:|:-------:|:-------:|
|    Ploidy    |    1    |    0    |    0    |
|      pH      |    4    |    1    |    0    |

**Table 2**. DML obtained at various percent methylation differences for ploidy or pH contrasts.

| **Contrast** |   **chr**   | **start** |  **end** | **pvalue** | **qvalue** | **meth.diff** |
|:------------:|:-----------:|:---------:|:--------:|:----------:|:----------:|:-------------:|
|    ploidy    | NC_047566.1 |  46447078 | 46447080 |  8.71E-13  |  1.62E-06  |   37.3155448  |
|      pH      | NC_047561.1 |  7843128  |  7843130 |  5.50E-08  |  0.0044294 |   -26.315789  |
|      pH      | NC_047565.1 |  4762558  |  4762560 |  3.96E-08  | 0.00364724 |   -26.731667  |
|      pH      | NC_047565.1 |  44578741 | 44578743 |  1.23E-07  | 0.00671761 |   -26.789645  |
|      pH      | NC_047567.1 |  9113140  |  9113142 |  9.18E-08  | 0.00610252 |   50.1223071  |
|      pH      | NC_047567.1 |  9113140  |  9113142 |  9.18E-08  | 0.00610252 |   50.1223071  |

I wasn't expecting many DML from this analysis because of the covariate information, but I'm surprised I don't have at least a hundred for one of the contrasts. It's also interesting that all of the DML are found in one chromosome. I should look into that further when I have my final DML list.

### Changing `min.per.group` within `unite`

Steven suggested I change the `min.per.group` parameter in the `unite` command to allow for loci without data in all samples to be included in the analysis. I started by allowing a CpG to be present in 9/12 samples per treatment (75%):

```
methylationInformationFilteredCov5 <- methylKit::unite(processedFilteredFilesCov5,
                                                       destrand = FALSE,
                                                       min.per.group = 9L) #Combine all processed files into a single table. Use destrand = TRUE to not destrand. Based with data in at least 9/12 samples peer treatment will be included
```

With this change, I had 4,557,452 CpG loci with data in at least 9 samples per treatment. I got more DML this time, but still less than 100! Looking at [my reference script from another paper](https://github.com/smcnew/epigbs/blob/main/gamo_analysis_clean.R#L141), it seemed like they tried out several `min.per.group` options. For reference, I have between 5,173,369 and 8,057,460 CpG loci with data with at least 5x coverage, depending on the sample. Since I'm getting 4,557,452 after `unite` with `min.per.group = 9L`, I'm not sure if it's worth doing something similar to 1) count the number of CpG loci with data and 2) count the number of DML obtained with multiple settings. However, I decided to try `min.per.group = 8L`, which gave me 5,103,729 CpGs.

**Table 3**. Number of DML obtained from various methylation difference thresholds using `min.per.group = 9L` or `min.per.group = 8L`.

| **Contrast** | **min.per.group** | **25%** | **50%** | **75%** |
|:------------:|:-----------------:|:-------:|:-------:|:-------:|
|    Ploidy    |         9         |    19   |    0    |    0    |
|    Ploidy    |         8         |    29   |    1    |    0    |
|      pH      |         9         |    32   |    2    |    0    |
|      pH      |         8         |    42   |    3    |    0    |

Based on Table 3, I think I need to use a 25% threshold for calling DML. Again, I'm pretty sure my use of covariates is affecting the *P*- and q-values I get from `methylKit`. I think I'll also use the `min.per.group = 8L` output, since I get marginally more DML.

### Redoing comparative analysis with `min.per.group = 8L`

[My previous comparative analysis](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part13/) used the default settings: CpGs must have data in all samples to be included in downstream analysis. I re-did the comparative analysis with the `min.per.group = 8L` parameter.

![corr-plot](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/Haws_04-methylKit/general-stats/Full-Sample-Pearson-Correlation-Plot-FilteredCov5Destrand.jpeg)

**Figure 1**. Correlation plot

![clustering](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/Haws_04-methylKit/general-stats/Full-Sample-CpG-Methylation-Clustering-FilteredCov5Destrand.jpeg)

**Figure 2**. Clustering diagram

![PCA](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/Haws_04-methylKit/general-stats/Full-Sample-Methylation-PCA-FilteredCov5Destrand.jpeg)

**Figure 3**. PCA

The PCA and clustering diagram don't look very different, but the correlations between samples are lower 0.01. Overall, nothing that rocks the boat.

### Going forward

1. Test-run [DSS](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#34_DMLDMR_detection_from_general_experimental_design) and [ramwas](https://bioconductor.org/packages/release/bioc/html/ramwas.html)
2. Look at DML in IGV
1. Try [EpiDiverse/snp](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
1. Run `methylKit` randomization test on `mox`
5. Investigate comparison mechanisms for samples with different ploidy in oysters and other taxa
5. Characterize the general methylation landscape
6. Identify genomic locations of DML
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)
6. Update methods
7. Update results

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
