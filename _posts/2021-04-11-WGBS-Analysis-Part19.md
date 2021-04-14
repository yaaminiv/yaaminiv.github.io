---
layout: post
comments: true
title: WGBS Analysis Part 19
tags: manchester gigas-broodstock methylKit
---

## Separating female and indeterminate samples

My [intended R code works](https://yaaminiv.github.io/WGBS-Analysis-Part18/), so now I need to finalize my script and create an analogous [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html) script for `mox`!

Before creating a `mox` script, I wanted to separate out the indeterminate and female oyster samples. When looking for covariate metadata, I [realized that not all samples are from female oysters](https://yaaminiv.github.io/WGBS-Analysis-Part15/)! When I picking extracted DNA samples to sequence, I tried to get as many female samples as possible, but due to some issues with having enough material for sequencing, I ended up sequencing one indeterminate sample per treatment. My plan was to use this indeterminate sample to see if any DML I identified were more related to maturation status, and how immature oyster gonad methylation could be impacted. I promptly forgot I had this plan until looking at previous lab notebooks, so thank you past Yaamini.

My first instinct was to try subsetting the `methylRawListDB` created by `methRead` like a standard dataframe, but that didn't work. I returned to the [`methylKit` handbook](https://bioconductor.riken.jp/packages/3.9/bioc/vignettes/methylKit/inst/doc/methylKit.html#53_selection:_subsetting_methylkit_objects) and found a convenience function to subset objects, `select`. However, this function doesn't work for `methylRawListDB`! A quick search lead my back to [`reorganize`](https://rdrr.io/bioc/methylKit/man/reorganize-methods.html), which is the function I used to [set up randomization trials](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part9/). Looking at the examples for the function, I realized I could subset the `methylBase` object created by `unite`, instead of subseting the `methylRawListDB`, filtering data, and combining! I think the comparative analysis portion (PCA and correlation plot) is useful when all samples are included, and that's created using the `unite` methylBase object. If I could subset the female or indeterminate samples from this object, then I can identify sex-specific DML in a less computationally-intensive way.

I successfully used the following code to separate female samples from the `methylBase` object:

```{r}
methylationInformationFilteredCov5Fem <- methylKit::reorganize(methylationInformationFilteredCov5,
                                                               sample.ids = c("1", "3", "4",
                                                                              "6", "7", "8"),
                                                               treatment = c(rep(1, times = 3),
                                                                             rep(0, times = 3))) #Get female sample information from methylBase object created by unite by specifying sample IDs. Include treatment information as well
```

By specifying `sample.ids`, I could pull out the samples I wanted. I confirmed the function worked by looking at my original object (`methylationInformationFilteredCov5`) and the subsetted object (`methylationInformationFilteredCov5Fem`):

![Screen Shot 2021-04-11 at 1 24 08 PM](https://user-images.githubusercontent.com/22335838/114441235-fc3dea00-9b7f-11eb-8f13-6863a83e1c32.png)

![Screen Shot 2021-04-11 at 1 24 15 PM](https://user-images.githubusercontent.com/22335838/114441246-fe07ad80-9b7f-11eb-9742-53615d373883.png)

**Figures 1-2**. `methylBase` objects before and after subsetting

Sample 2 in the subsetted object is the same as sample 3 in the original object, so I think it worked! I then cleaned up [my R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.Rmd) and created separate sections for female- and indeterminate-DML identification. I used covariate and overdispersion corrections for the female data, but not for the 1 vs. 1 indeterminate sample comparison. I ran separate comparative analyses on the each data subset just to see what it might look like, but I didn't pay too much attention to the output since I haven't updated the data I'm working with to the Roslin alignments (I modified the code, but I haven't rerun those chunks because they're time-consuming and I'm going to do it on `mox` anyways). Knowing that I could subset from the `methylBase` object and assign treatment information in that subsetting process, I also updated my randomization trial code:

```{r}
for (i in 1:1000) { #For each iteration
  pHRandTreat <- sample(sampleMetadataFem$pHTreatment,
                        size = length(sampleMetadataFem$pHTreatment),
                        replace = FALSE) #Randomize female treatment information
  pHRandDML <- methylKit::reorganize(methylationInformationFilteredCov5Fem,
                                     sample.ids = sampleMetadataFem$sampleID,
                                     treatment = pHRandTreat) %>%
    methylKit::calculateDiffMeth(., covariates = covariateMetadataFem,
                                 overdispersion = "MN",
                                 test = "Chisq") %>%
    methylKit::getMethylDiff(., difference = 25, qvalue = 0.01) %>%
    nrow() -> pHRandTest25Fem[i] #Shuffle treatments from methylBase object created by unite. Calculate diffMeth and obtain DML that are 25% and have a q-value of 0.01. Save the number of DML identified
}
```

### Going forward

1. Write methods
2. Obtain relatedness matrix and SNPs with [EpiDiverse/snp](https://github.com/EpiDiverse/snp)
3. Run R script on `mox`
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
