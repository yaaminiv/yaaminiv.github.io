---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 8
tags: hawaii gigas-ploidy methylkit
---

## Preliminary DML identification

After [obtaining descriptive and comparative metrics](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part7/), the next thing I wanted to do was identify ploidy or pH DML. While using [DSS](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#34_DMLDMR_detection_from_general_experimental_design) or [ramwas](https://bioconductor.org/packages/release/bioc/html/ramwas.html) would provide more robust DML testing, I thought I might as well try this method. If I can use two different methods to identify DML, that would also increase the confidence in my findings.

### Diploid vs. triploid

#### `min.per.group`

I opened [this R Markdown script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit.Rmd) and planned out the analysis. I wanted to play around with the `min.per.group` argument in `unite`. I set `min.per.group` to 10 per treatment, then united the files:

```{r}
methylationInformationFilteredCov5 <- methylKit::unite(processedFilteredFilesCov5,
                                                       min.per.group = 10L
                                                       destrand = FALSE) #Combine all processed files into a single table. Use destrand = TRUE to not destrand. Loci need data from 10/12 samples per treatment to remain in the analysis
head(methylationInformationFilteredCov5) #Confirm unite
```

(Side note: I have no idea why I need to specify `methylKit::unite` instead of `unite` since I've never had to do that before. My guess is that it has something to do with the way I loaded `methylKit` since Bioconductor is funky.)

After uniting the files, I tried remaking the correlation plot. Instead, I got a matrix and plot filled with NAs! I found [this issue](https://github.com/al2na/methylKit/issues/137) on the [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html) Github and learned that because not every CpG has data in each sample using `min.per.group`, the correlation and clustering functions produce NAs. I dug into the [user guide description of `unite`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html#31_Merging_samples) and reminded myself that the default for the `unite` function is that all loci need data in all samples! Since the default is more stringent than the `min.per.group` argument, I decided to keep it.

#### Specifying covariates

I've seen a few papers use the covariate specification within `calculateDiffMeth`, and I wanted to give it a shot! The first thing I needed to do was create a covariate matrix. I made a dataframe with ploidy and pH treatment information, but got an error. The covariate matrix example in [the `methylKit` user guide](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html#38_Accounting_for_covariates) had only one covariate, so I figured it would be safer to make separate ploidy and pH covariate matrices. Since my initial treatment specification was diploid vs. triploid, I started by incorporating pH information:

```{r}
covariatepH <- data.frame("pH" = c(rep("H", times = 6),
                                 rep("L", times = 6),
                                 rep("H", times = 6),
                                 rep("L", times = 6))) #Create dataframe with pH covariate information
head(covariatepH) #Check dataframe format

differentialMethylationStatsPloidy <- methylKit::calculateDiffMeth(methylationInformationFilteredCov5,
                                                                          covariates = covariatepH) #Calculate differential methylation statistics based on treatment indication from methRead. Include pH as a covariate
head(differentialMethylationStatsPloidy) #Look at differential methylation statistics
```

This took FOREVER. Usually `unite` is the limiting step, but this took SO LONG. I guess this is what happens when you complicate the model. I killed the command so I could work on modifying other pieces of code.

I set up DML identification to use a 50% difference and q-value of 0.01. While I used this specification to identify hyper- and hypomethylated DML, I also included code to see how many DML would be identified using a 75% or 100% difference.

#### `normalizeCoverage`

According to the [FAQ](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html#36_Finding_differentially_methylated_bases_or_regions), I should normalize coverage between samples so reads from one sample aren't oversampled during statistical testing. So, I tried to do just that:

```{r}
processedFilteredFilesCov5 <- methylKit::filterByCoverage(processedFiles,
                                                          lo.count = 5, lo.perc = NULL,
                                                          high.count = NULL, high.perc = 99.9) %>%
  methylKit::normalizeCoverage(processedFilteredFilesCov5) #Filter coverage information for minimum 5x coverage, and remove PCR duplicates by excluding data in the 99.9th percentile of coverage with hi.perc = 99.9. Normalize coverage between samples to avoid over-sampling reads from one sample during statistical testing
```

This is when I started running into vector memory issues with R. Since I was running this code on `genefish`, I decided to try the R Studio server. I would have to start running the script from the beginning, but I basically had to rerun code anyways after normalizing coverage. Also, I played around with covariates and realized that running it on the server may also take less time (see complaints below).

I tried to use the server, but no dice. I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1137), and proceeded optimizing my code.

#### Randomization trial

Another thing I've seen papers do is randomize the treatment information, create a distribution of DML identified, and determine if the number of DML identified is significant. I based my analysis off of [this script](https://github.com/smcnew/epigbs/blob/main/gamo_analysis_clean.R#L227). First, I randomized the order of treatment specifications. Then, I used [`methylKit::reorganize`](https://www.rdocumentation.org/packages/methylKit/versions/0.99.2/topics/reorganize) to match the existing data with a new sample treatment specification in my `methRead` object. I then piped that information into `filterByCoverage`, `unite`, `calculateDiffMeth`, and `getMethylDiff`. Finally, I created a loop to go through this process 1000 times:

```{r}
ploidyRandTest <- NULL #Create an empty matrix to store randomization trial results
```

```{r}
for (i in 1:1000) { #For each iteration
  ploidyRandTreat <- sample(sampleMetadata$ploidyTreatment,
                            size = length(sampleMetadata$ploidyTreatment),
                            replace = FALSE) #Randomize treatment information
  ploidyRandDML <- methylKit::reorganize(processedFiles,
                                         sample.ids = sampleMetadata$sampleID,
                                         treatment = ploidyRandTreat) %>%
    methylKit::filterByCoverage(., lo.count = 5, lo.perc = NULL,
                                hi.count = NULL, hi.perc = 99.9) %>%
    methylKit::normalizeCoverage(.) %>%
    methylKit::unite(., destrand = FALSE) %>%
    methylKit::calculateDiffMeth(., covariates = covariatepH,
                                 mc.cores = 4) %>%
    methylKit::getMethylDiff(., difference = 50, qvalue = 0.01) %>% #Reorganize, filter, normalize, and unite samples. Calculate diffMeth and obtain DML that are 50% and have a q-value of 0.01
    nrow() -> ploidyRandTest[i] #Save the number of DML identified
}
```

```{r}
hist(ploidyRandTest) #Plot a histogram with randomization trial results

#ploidyRandTest %>% filter(. > nrow(diffMethStatsPloidy50)) %>% nrow(.) / nrow(ploidyRandTest) #Calculate p-value
```

### Low pH vs. ambient pH

I used all of the "finalized" code chunks above to recreate the workflow to identify treatment-specific DML! I used `reorganize` to assign treatment specifications, then merged all sample information with `unite`:

```{r}
methylationInformationFilteredCov5T <- methylKit::reorganize(processedFiles,
                                                     sample.id = sampleMetadata$sampleID,
                                                     treatment = sampleMetadata$pHTreatment) %>%
  methylKit::filterByCoverage(.,
                              lo.count = 5, lo.perc = NULL,
                              high.count = NULL, high.perc = 99.9) %>%
  methylKit::normalizeCoverage(.) %>%
  methylKit::unite(processedFilteredFilesCov5T,
                   destrand = FALSE,
                   mc.cores = 4) #Reorganize processedFiles to provide pH treatment specification. Filter by coverage and normalize coverage between samples. Combine all processed files into a single table. Use destrand = TRUE to not destrand. By default only bases with data in all samples will be kept
head(methylationInformationFilteredCov5T) #Confirm unite
```

Similar to the ploidy DML, I based my hyper- and hypomethylated DML identification on a 50% difference, but tested 75% and 100% differences as well. I also coded a randomization test.

The code has been written, but now to my bigger problem: running it. Let's see what happens when I can get it to run without memory errors.

### Going forward

1. Actually run `methylKit` script
1. Get WGS resequencing quote
1. Try [BS-SNPer](https://github.com/hellbelly/BS-Snper) and [EpiDiverse](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
5. Investigate comparison mechanisms for samples with different ploidy in oysters and other taxa
4. Test-run [DSS](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#34_DMLDMR_detection_from_general_experimental_design) and [ramwas](https://bioconductor.org/packages/release/bioc/html/ramwas.html)
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
