---
layout: post
comments: true
title: WGBS Analysis Part 18
tags: manchester gigas-broodstock methylKit mox
---

## Preparing R scripts for `mox`

Alright, it's time to run [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html) on `mox`! Steven and Sam suggested I run my `methylKit` code in [this discussion](https://github.com/RobertsLab/resources/discussions/1137#discussioncomment-451773), so I'll finish testing my code, make some modifications, and create a script to run on `mox`.

### Percent difference for DML

The first thing I needed to make a decision about was the percent difference I wanted to use as a threshold for DML identification. In [this discussion](https://github.com/RobertsLab/resources/discussions/1140), I pointed out that some papers use a 25% difference, while others (including my *C. virginica* paper) use a 50% difference. Initially, I set up my code to include 25%, 50%, 75%, and 99% differences in methylation between treatments. After loading in my saved .RData, I ran `getMethylDiff` using objects generated from `calculateDiffMeth` with no additional modifications and just the overdispersion correction.

Using a 25% or 50% methylation difference, I got > 1000 DML when not applying any covariate matrix or overdispersion correction! The number of DML identified were in the 100s when using a 75% or 99% difference. When I used the output from `calculateDiffMeth` with the overdispersion correcrtion, I identified 2119 DML that were 25% different, 564 that were 50% different, and 80 that were 75% different. Clearly making the model more complicated reduces the amount of DML identified. I think if I used output with a covariate matrix and overdispersion correction, a 25% or 50% methylation difference wouldn't get me more than ~500 DML. I set up the DML identification code to use both a 25% and 50% methylation difference so I can compare the output from `mox`.

### Testing randomization code

The main parts of [my R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.Rmd#L232) I needed to still test were the randomization trial and subsequent statistical testing. I started by running a modified version of the randomization trial that did not include any overdispersion or covariate information for `calculateDiffMeth`:

```{r}
for (i in 1:1000) { #For each iteration
  pHRandTreat <- sample(sampleMetadata$pHTreatment,
                        size = length(sampleMetadata$pHTreatment),
                        replace = FALSE) #Randomize treatment information
  pHRandDML <- methylKit::reorganize(processedFiles,
                                     sample.ids = sampleMetadata$sampleID,
                                     treatment = pHRandTreat) %>%
    methylKit::filterByCoverage(., lo.count = 5, lo.perc = NULL,
                                hi.count = NULL, hi.perc = 99.9) %>%
    methylKit::normalizeCoverage(.) %>%
    methylKit::unite(., destrand = FALSE) %>%
    methylKit::calculateDiffMeth(.) %>%
    methylKit::getMethylDiff(., difference = 50, qvalue = 0.01) %>%
    nrow() -> pHRandTest50[i] ##Reorganize, filter, normalize, and unite samples. Calculate diffMeth and obtain DML that are 50% and have a q-value of 0.01. Save the number of DML identified
}
```

I thought the code would finish running in a few hours, but since it was taking a while and I didn't get any error messages, I interrupted the code chunk and commented it out. I then set up two randomization tests: one for a 25% methylation difference, and another for a 50% methylation difference:

```{r}
pHRandTest25 <- NULL #Create an empty matrix to store randomization trial results for 25% difference

for (i in 1:1000) { #For each iteration
  pHRandTreat <- sample(sampleMetadata$pHTreatment,
                        size = length(sampleMetadata$pHTreatment),
                        replace = FALSE) #Randomize treatment information
  pHRandDML <- methylKit::reorganize(processedFiles,
                                     sample.ids = sampleMetadata$sampleID,
                                     treatment = pHRandTreat) %>%
    methylKit::filterByCoverage(., lo.count = 5, lo.perc = NULL,
                                hi.count = NULL, hi.perc = 99.9) %>%
    methylKit::normalizeCoverage(.) %>%
    methylKit::unite(., destrand = FALSE) %>%
    methylKit::calculateDiffMeth(., covariates = covariateMetadata,
                                 overdispersion = "MN",
                                 test = "Chisq") %>%
    methylKit::getMethylDiff(., difference = 25, qvalue = 0.01) %>%
    nrow() -> pHRandTest25[i] ##Reorganize, filter, normalize, and unite samples. Calculate diffMeth and obtain DML that are 50% and have a q-value of 0.01. Save the number of DML identified
}
```

```{r}
pHRandTest50 <- NULL #Create an empty matrix to store randomization trial results for 50% difference

for (i in 1:1000) { #For each iteration
  pHRandTreat <- sample(sampleMetadata$pHTreatment,
                        size = length(sampleMetadata$pHTreatment),
                        replace = FALSE) #Randomize treatment information
  pHRandDML <- methylKit::reorganize(processedFiles,
                                     sample.ids = sampleMetadata$sampleID,
                                     treatment = pHRandTreat) %>%
    methylKit::filterByCoverage(., lo.count = 5, lo.perc = NULL,
                                hi.count = NULL, hi.perc = 99.9) %>%
    methylKit::normalizeCoverage(.) %>%
    methylKit::unite(., destrand = FALSE) %>%
    methylKit::calculateDiffMeth(., covariates = covariateMetadata,
                                 overdispersion = "MN",
                                 test = "Chisq") %>%
    methylKit::getMethylDiff(., difference = 50, qvalue = 0.01) %>%
    nrow() -> pHRandTest50[i] ##Reorganize, filter, normalize, and unite samples. Calculate diffMeth and obtain DML that are 50% and have a q-value of 0.01. Save the number of DML identified
}
```

The output of the randomization test is a vector with the number of DML identified in each trial. I needed a way to determine if the number of DML I identified with `getMethylDiff` was significantly greater than what may be identified by chance. I dug deep into my stats brain (you know, that part of my brain that barely turns on) and decided to run a one-tailed t-test:

```{r}
pHRandTestResults50 <- t.test(pHRandTest50, alternative = "less", mu = nrow(diffMethStatsTreatment50)) #Conduct a one-sample t-test to determine the probability of identifying more DML by chance. mu is the number of DML identified with a 50% methylation difference
pHRandTestResults50$statistic #t
pHRandTestResults50$parameter #df
pHRandTestResults50$p.value #P-value
```

I also wrote code to create a histogram with the random distribution of DML identified, and include a vertical line for the number of DML obtained with `getMethylDiff`:

```{r}
jpeg(filename = "../output/06-methylKit/rand-test/Random-Distribution-diff50.jpeg", height = 1000, width = 1000) #Save file with designated name
hist(pHRandTest50, plot = TRUE, main = "", xlab = "Number of DML (50% difference)") #Plot a histogram with randomization trial results
dev.off()
```

Now that I've tested my base code, I will work on separating female and indeterminate samples and creating the actual `mox` script!

### Going forward

1. Separate female and indeterminate samples
2. Write a `mox` script
3. Run R script on `mox`
3. Update methods
2. Obtain relatedness matrix and SNPs with [EpiDiverse/snp](https://github.com/EpiDiverse/snp)
2. Revise `methylKit` study design: separate tests for indeterminate and female oysters
3. Write results
4. Identify genomic location of DML
2. Determine if RNA should be extracted
3. Determine if larval DNA/RNA should be extracted
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
