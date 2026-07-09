---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 25
tags: hawaii gigas-ploidy methylKit
---

## Revising `methylKit` analysis

Steven suggested I [move forward with the Roslin genome](https://github.com/RobertsLab/project-oyster-oa/issues/44) since the analysis is pretty much done. Yay! My next step is to revise the `methylKit` analysis. Previously (aka in February 2025) we noticed that the heatmaps generated for the DML [do not inspire any confidence](https://github.com/RobertsLab/project-oyster-oa/issues/42). To figure out why, I needed to [review the `methylKit` code](https://github.com/RobertsLab/project-oyster-oa/issues/43).

### Installing `methylKit`

The first headache I had to go through was setting up my workflow on the Raven R Studio server. I cloned the [Github repository](https://github.com/RobertsLab/project-oyster-oa) and downloaded the coverage files to the data folder:

```
wget -r \
--no-check-certificate --no-directories --no-parent --reject "index.html*" \
-P . \
-A "*CpG_report.merged_CpG_evidence.cov" https://gannet.fish.washington.edu/spartina/project-oyster-oa/Haws/bismark-2/
```

While that was going on, I returned to the hell of trying to install `methylKit` on the R Studio server. I started by running the installation using `BiocManager` similar to what I had successfully done on my laptop. However, the package would not install. I turned to Gemini for troubleshooting, which pointed out a package compatibility issue between the latest releases of `methylKit` and the dependency `data.table` and the versions needed for R 4.2.3 on the R Studio server. The following provided a successful installation:

```
install.packages("https://cran.r-project.org/src/contrib/Archive/data.table/data.table_1.16.4.tar.gz",
                 repos = NULL,
                 type = "source",
                 lib = "/home/shared/8TB_HDD_02/yaaminiv/R/x86_64-pc-linux-gnu-library/4.2") # R Studio server: need legacy data.table in order to install legacy methylKit

require(BiocManager)
BiocManager::install("methylKit") #Install methylKit
require(methylKit) #Load methylKit
```

### Checking `methylKit` parameters

Prior to DML identification, I wanted to determine that I had all of the right parameters coded. I went through [my script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit.Rmd) and cross-referenced coding decisions with the [`methylKit` vignette](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html#3_Comparative_analysis) and [my *C. gigas* female reproductive tissue DML code](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.Rmd):

- Confirmed that I should not destrand my data during the `unite` step.
- The draft manuscript says that `min.per.group = 8L` was used (8/12) for each condition during the `unite` step. However, the code suggests that 5x coverage was retained across all samples. Seems like this has been the case since August 2021?
  - If I'm using loci with coverage across all samples during `unite`, then I may not need to redo the `unite` step when switching between ploidy and pH DML identification. If I make the decision to change `min.per.group` to a different value, then I will need to redo the `unite` step. This is because `min.per.group` will make decisions about loci based on ploidy treatment, which may not be accurate for pH DML identification.
- Created [this metadata table](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Haws/sample_metadata.csv) with information from [Nightingales](https://docs.google.com/spreadsheets/d/1_XqIOPVHSBVGscnjzDSWUeRL7HUHXfaHxVzec-I-8Xk/edit?gid=0#gid=0). I used this [directly in my script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit.Rmd#L85) to create a metadata dataframe for use with `methylKit`.

I wanted to remake the figures from the comparative analysis section, but R Studio server kept hanging. I decided to skip remaking the figures for now until I decided on some `unite` and DML identification parameters.

### DML identification parameter testing

The next step is to review the actual parameters used for DML identification. There are a few things that can be modified:

- `unite`: Default is to retain loci with at least 5x coverage across all samples. This can be modified using the `min.per.group` parameter
- `difference`: Determine the baseline difference in percent methylation necessary to identify a loci as a DML. I usually don't go any lower than 50%, but at the same time this identification is relatively arbitrary (what does a 50% difference mean biologically?). However, the higher the difference, the more conservative.
- `covariates`: I am examining differences in ploidy and pH methylation separately. I can use the other variable as a covariate within the `calculateDiffMeth` command.
- `overdispersion`: Can use when there is more variability in the data than assumed by the distribution. An F-test is automatically used when using this correction, but a chi-squared test can be specified manually.

I decided to start by comparing the number of ploidy DML identified by tweaking various parameters.

**Table 1**. Number of DML identified based on various parameters. Identified DML have not been corrected for C->T SNPs.

| **Comparison** | **`min.per.group`** | **Covariate** | **Overdispersion** | **Statistical test** | **`difference`** | **Number of DML** |
|:--------------:|:-------------------:|:-------------:|:------------------:|:--------------------:|:----------------:|:-----------------:|
|     ploidy     |          12         |       pH      |          Y         |        F-test        |        25        |         0         |
|     ploidy     |          12         |       pH      |          Y         |        F-test        |        50        |         0         |
|     ploidy     |          12         |       pH      |          Y         |        F-test        |        75        |         0         |
|     ploidy     |          12         |       pH      |          Y         |      chi-squared     |        25        |         1         |
|     ploidy     |          12         |       pH      |          Y         |      chi-squared     |        50        |         0         |
|     ploidy     |          12         |       pH      |          Y         |      chi-squared     |        75        |         0         |
|     ploidy     |          12         |       pH      |          N         |      chi-squared     |        25        |         6         |
|     ploidy     |          12         |       pH      |          N         |      chi-squared     |        50        |         1         |
|     ploidy     |          12         |       pH      |          N         |      chi-squared     |        75        |         1         |
|     ploidy     |          12         |      N/A      |          Y         |      chi-squared     |        25        |         1         |
|     ploidy     |          12         |      N/A      |          Y         |      chi-squared     |        50        |         0         |
|     ploidy     |          12         |      N/A      |          Y         |      chi-squared     |        75        |         0         |
|     ploidy     |          12         |      N/A      |          N         |      chi-squared     |        25        |        741        |
|     ploidy     |          12         |      N/A      |          N         |      chi-squared     |        50        |         1         |
|     ploidy     |          12         |      N/A      |          N         |      chi-squared     |        75        |         0         |

Having this information is good, but I needed to figure out when using an overdispersion correction was appropriate. I found [this question in the `methylKit` Google Group](https://groups.google.com/g/methylkit_discussion/c/F3zwuTmepQs/m/13JuxBR1DgAJ?pli=1). The response suggested that I need to estimate dispersion by using `estimateDisp()` from `edgeR` or examine the coefficient of variation in methylation proportions across replicates to determine if I needed to use an overdispersion correction. If I used `estimateDisp()`, an overdispersion value of 1 or higher would suggest that I should use the overdispersion correction. If using the overdispersion correction, I need to use the F-test, not the chi-squared test.

The response to the question states that overdispersion is common in low-coverage WGBS. We aimed for 30x depth sequencing, so I don't think coverage would be an issue here. Looking at the sample PCA, most samples cluster together with their treatment groups except for two diploid and two triploid outliers. Pearson correlations are pretty strong between all the samples as well. For this reason, I don't actually think an overdispersion correction is appropriate. So, I think the most appropriate comparison in Table 1 is with or without covariates with no overdispersion correction and a chi-squared statistical test.

After discussing my approach with Ariana, she suggested that I perform a beta-dispersion test with my existing PCA data. This way, I could confirm that there weren't any dispersion differences between ploidy conditions or pH treatments, and choose the most appropriate `methylKit` parameters. I did end up needing to reformat the data in order to run the beta-dispersion test. It involved a lot of creative pivoting that I couldn't figure out by myself, so I had Gemini help. My lack of regex understanding has really hindered me in my ability to do things myself:

```
#Code generated with assistance from Gemini

methInfoDisp <- getData(methylationInformationFilteredCov5) %>% #Use methylKit convenience function to get data of of methylBase object
  tidyr::unite(., "chr_start_end", chr:end, remove = TRUE) %>% #Combine chr, start, and end information to one columns
  dplyr::select(-strand) %>% #Remove strand column
  pivot_longer(
    cols = -chr_start_end,
    names_to = c(".value", "sample_number"),
    names_pattern = "(coverage|numCs|numTs)(\\d+)") %>% #Pivot into a long format to separate the metric types from the sample numbers. The names_pattern regex splits your column names (like coverage1 or numCs24) into two pieces. The (coverage|numCs|numTs) part matches the text and sends it to .value (telling R these should remain as separate columns). The (\\d+) part captures the digits and places them into a new sample_id column.
  pivot_wider(
    names_from = chr_start_end,
    values_from = c(coverage, numCs, numTs),
    names_glue = "{.value}_{chr_start_end}") %>% #Pivot wider so that samples become rows and positions become columns. This takes your chr_start_end positions and spreads them out into columns. By passing coverage, numCs, numTs to values_from, it automatically pairs each position with each metric.
  mutate(sample_number = as.numeric(sample_number)) %>% #Convert character column to numeric
  right_join(x = (sampleMetadata %>% dplyr::select(c(sample_number:sampleID))) , y = ., by = "sample_number") #Join with sample metadata to get sampleID as well as treatment information in the dataframe
head(methInfoDisp) #Confirm dataframe creation
```

My resultant dataframe has sample ID and pH/ploidy information in columns, and either coverage, number of Cs, or number of Ts at a genomic position for other columns. Dataframe in hand, I was able to conduct the beta-dispersion test for ploidy and pH:

```{r}
dissim.meth <- vegdist(scale(methInfoDisp[c(5:5953594)]), "euclidean", na.rm = TRUE) #Create euclidean dissimilarity matrix to use for PERMDISP

disp.meth.ploidy <- betadisper(dissim.meth, group = as.factor(methInfoDisp$ploidy), type = 'centroid') #Run a beta dispersion model to assess if significant differences are due to differences in group centroid or variance
plot(disp.meth.ploidy)
boxplot(disp.meth.ploidy) #Show distance to group centroids
anova(disp.meth.ploidy) #Variance is not different between groups. Do not need overdispersion correction for ploidy!

disp.meth.pH <- betadisper(dissim.meth, group = as.factor(methInfoDisp$pH), type = 'centroid') #Run a beta dispersion model to assess if significant differences are due to differences in group centroid or variance
plot(disp.meth.pH) #Similar dispersion, but some potential outliers
boxplot(disp.meth.pH) #Similar dispersion differences, outliers seen more easily
anova(disp.meth.pH) #Variance is the same between groups. Do not need overdispersion correction for pH!
```

These results show that there are no significant differences in dispersion between ploidy condition or pH treatment! I modified my `methylKit` code as such. I also decided to go with covariates since that is an experimental necessity.

Sidenote: I forgot just how SLOW using the R Studio server can be. I guess that's partly the very large dataset?

### Going forward

1. Continue parameter testing for DML identification
2. Revise `methylKit` methods and results
4. `methylKIt` randomization test
1. ATAC-Seq data integration
2. `KOG-MWU` for *Crassostrea* methylation comparison
5. Revise discussion
7. Revise introduction
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)

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
