---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 17
tags: hawaii gigas-ploidy DSS mox
---

## Trying `DSS` for DML identification

Initially, I wanted to test-run [`DSS`](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#34_DMLDMR_detection_from_general_experimental_design) and [`ramwas`](https://bioconductor.org/packages/release/bioc/html/ramwas.html). I looked into the manuals for both packages, and wasn't convinced that `ramwas` was a good fit for my data. I could associate methylation information with genotype data, which seemed valuable. However, my genotype data came from `BS-Snper` analyses with my WGBS data, and looking for associations between WGBS and WGBS-derived genotype data didn't make sense to me! Instead, I focused on using `DSS` to identify differentially methylated loci.

### `DSS` background

`DSS` uses a Bayesian hierarchical model to estimate dispersions, then uses a Wald test for differential methylation, to calculate CpG methylation in a general experimental design. I was interested in using this package because I wanted to test pH and ploidy simultaneously, instead of using each variable as a covariate in my analysis! Coverage data (i.e. count data) are modeled using a beta-binomial distribution with an arcisne link function and log-normal priors, and the model is fit on the transformed data using the generalized least squares method. A Wald test is used at each CpG site for statistical testing. The test itself depends on sequencing depth and a dispersion parameter. This dispersion parameter is defined by a shrinkage estimator from the Bayesian hierarchical model. After conducting the Wald test, I can define DML based on p-value and FDR thresholds.

According to the package creators, this method is better than the Fisher's test used by [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html). With the Fisher's test, read counts are added between samples in a treatment (think `unite`). By doing this, the test assumes that data in each sample are distributed in the same way, and obscures any variation between samples in a treatment. Based on the data I've seen from `methylKit`, the data do have similar distributions, but I have seen certain samples look like outliers in the PCA. Retaining the biological variation in each treatment with the dispersion parameters seems like a good step in the right direction.

Another thing `DSS` does while modeling is smoothing, which involves using information from nearby CpG sites to better inform methylation calculations at a specific CpG site. [The manual](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#4_Frequently_Asked_Questions) made it sound like smoothing was always recommended to improve calculations, but [the paper](https://academic.oup.com/nar/article/42/8/e69/1074350#119438087) only suggested using smoothing when the data is "dense" and there are CpG sites closeby. This immediately made me think of differences between the vertebrate and invertebrate methylation model. It's not likely that I'll have many CpGs nearby to estimate methylation information, so I decided not to use any smoothing.

Oh, did I mention that `DSS` was designed to be less computationally intensive? Very important.

### Installing packages on R Studio server

The first step of my analysis was installing packages on `mox` to use with the R Studio server! I needed to install `DSS` and its dependency `bsseq`. At first, I opened a build node and tried installing the packages that way, but I ran into continual errors. I then tried to install the packages from the R Studio server interface itself, and ran into similar issues. Turns out Aidan was having similar issues and [posted this discussion](https://github.com/RobertsLab/resources/discussions/1218). Sam mentioned that if we wanted to install packages to use on the server, we needed to create our own singularity container.

I followed [Sam's instructions in the handbook](https://robertslab.github.io/resources/mox_RStudio-Server/#createcustomize-your-own-singularity-rstudio-server-container) to create a singularity container. First I created a definition file:

![Screen Shot 2021-05-25 at 2 35 52 PM](https://user-images.githubusercontent.com/22335838/119571880-c424ec00-bd66-11eb-9297-87aaa3fb78f8.png)

I then created the script `r_packages_installs.R` to install specific R packages, like `methylKit`, `bsseq`, and `DSS`:

![Screen Shot 2021-05-25 at 2 36 14 PM](https://user-images.githubusercontent.com/22335838/119571886-c5eeaf80-bd66-11eb-9093-da92251f3bd2.png)

After logging into a build node, I loaded the singularity module and built the container `rstudio-4.0.2.yrv-v1.0.sif`. Finally, I modified my R Studio SLURM script, found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-DSS-RStudio.sh), to call my new singularity container! I think I [ran into some errors](https://github.com/RobertsLab/resources/discussions/1222) later on because my singularity container was in my login node instead of somewhere wiht more storage, but that didn't seem to be the sole cause. There might be something weird with my container or SLURM script, but it didn't fully affect my ability to do the analysis.

### Working through `DSS`

To work through this analysis, I relied heavily on the [DSS manual](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#3_Using_DSS_for_BS-seq_differential_methylation_analysis)! I pasted the code that ran on the R Studio server into [this R Markdown script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-DSS.Rmd) so it was easier to follow.

First, I modified my merged coverage files from `bismark` to fit DSS input requirements. Each sample needed a corresponding text file with chromosome, position, read coverage, and reads methylated, and required a header. For position, I used the start of the CpG dinucleotide.

```
#Create chr, pos, read cov, and reads meth columns

for f in *cov
do
  awk '{print $1"\t"$2"\t"$5+$6"\t"$5}' ${f} \
  > ${f}.txt
done

#Add header

for f in *txt
do
  sed  -i '1i chr\tpos\tN\tX' ${f}
done

#Rename files: zr3644_#.txt

for f in *txt
do
   [ -f ${f} ] || continue
    mv "${f}" "${f//_R1_val_1_val_1_val_1_bismark_bt2_pe..CpG_report.merged_CpG_evidence.cov/}"
done
```

To import multiple files into R Studio, I used a method with `list2env` [I found two years ago](https://genefish.wordpress.com/2019/08/20/importing-multiple-files-to-r/):

```{r}
coverageFiles <- list.files(pattern = "*txt") #Create a file list for all 24 files to import
list2env(lapply(setNames(coverageFiles,
                         make.names(gsub("", "", coverageFiles))), read.table, header = TRUE),
         envir = .GlobalEnv) #Import all coverage files into R global environment, using first line as header
```

Once I had the data in R Studio, I created a BSseq object (this is why I needed the `bsseq` dependency). This object is just a large conglomeration of coverage information for each sample and sample ID:

```{r}
BSobj <- makeBSseqData(dat = list(zr3644_1.txt, zr3644_2.txt, zr3644_3.txt, zr3644_4.txt, zr3644_5.txt, zr3644_6.txt,
                                  zr3644_7.txt, zr3644_8.txt, zr3644_9.txt, zr3644_10.txt, zr3644_11.txt, zr3644_12.txt,
                                  zr3644_13.txt, zr3644_14.txt, zr3644_15.txt, zr3644_16.txt, zr3644_17.txt, zr3644_18.txt,
                                  zr3644_19.txt, zr3644_20.txt, zr3644_21.txt, zr3644_22.txt, zr3644_23.txt, zr3644_24.txt),
                       sampleNames = c("2H-1", "2H-2", "2H-3", "2H-4", "2H-5", "2H-6",
                                       "2L-1", "2L-2", "2L-3", "2L-4", "2L-5", "2L-6",
                                       "3H-1", "3H-2", "3H-3", "3H-4", "3H-5", "3H-6",
                                       "3L-1", "3L-2", "3L-3", "3L-4", "3L-5", "3L-6")) #Make BSseq object. dat = list of dataframes with coverage information. sampleNames = sample names.
```

The BSseq object is what's used when fitting the model! After creating the BSseq object, I defined the experimental design, making sure to match sample ID information in the row names with what I included in the BSseq object:

```{r}
design <- data.frame("ploidy" = c(rep("D", times = 12),
                                  rep("T", times = 12)),
                     "pH" = c(rep("H", times = 6),
                              rep("L", times = 6),
                               rep("H", times = 6),
                               rep("L", times = 6))) #Create dataframe with experimental design information
row.names(design) <- c("2H-1", "2H-2", "2H-3", "2H-4", "2H-5", "2H-6",
                       "2L-1", "2L-2", "2L-3", "2L-4", "2L-5", "2L-6",
                       "3H-1", "3H-2", "3H-3", "3H-4", "3H-5", "3H-6",
                       "3L-1", "3L-2", "3L-3", "3L-4", "3L-5", "3L-6") #Set sampleID as rownames
```

To fit the general linear model to the data, I used the `DMLfit.multiFactor` function, which allowed me to specify a model with ploidy, pH, and in the interaction between the two variables. After fitting the model, I checked the column names to 1) confirm that the model fit the variables I wanted and 2) see which condition was picked at the "treatment" condition (this usually occurs alphabetically, and luckily for me my treatments are L and T, which come after H and D):

```{r}
DMLfit <- DMLfit.multiFactor(BSobj, design = design, formula = ~ploidy + pH + ploidy:pH) #Formula: intercept, additive effect of ploidy and pH, and interaction
colnames(DMLfit$X) #Column names of the design matrix (ploidyT = ploidy triploid, pHL = pH low, ploidyT:pHL = interaction)
```

For each coefficient, I extracted model fit information, then identified DML using a p-value < 0.05 and FDR < 0.01. This is roughly equivalent with a p-value < 0.05 and q-value < 0.01 I used with `methylKit`:

```{r}
DMLtest.pH <- DMLtest.multiFactor(DMLfit, coef = "pHL") #Test the pH effect
pH.DML <- subset(DMLtest.pH, subset = pvals < 0.05 & fdrs < 0.01) #Obtain loci with p-value < 0.05 and FDR < 0.01
```

One important thing to note is that I couldn't select a percent methylation difference with a generalized linear model. The model doesn't produce methylation values to begin with, and using multiple regressions makes it more complicated to return these kinds of values (see the FAQ [here](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#4_Frequently_Asked_Questions)).

### Looking at DML in IGV

Next step: look at DML in IGV! I opened [this IGV session](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_07-DML-characterization/dml.xml) and added in DSS DML BEDfile tracks I produced in [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/07-Genomic-Location-of-DML.ipynb). When I looked at the DML, it was easier to understand why some DML were called over others. I definitely ended up in a confusing/existential place: if we don't quite understand what methylation levels really mean, can I look at a DML in IGV and make a judgement call as to what looks like differential methylation? I decided to keep my DSS DML moving forward, even if I couldn't understand why certain loci were identified as differentially methylated.

### DML overlaps between methods

Based on the differences I saw with `methylKit` and `DSS` DML, I was interested in what overlaps existed between the different sets. In [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/07-Genomic-Location-of-DML.ipynb), I used `intersectBed` to look at those overlaps.

**Table 1.** Number of DML common between contrasts and methods. 25 DML overlapped between all DSS lists.

|      **DML List**      | **`methylKit` pH** | **`methylKit` ploidy** | **`DSS` pH** | **`DSS` ploidy** | **`DSS` ploidy:pH** |
|:----------------------:|:------------------:|:----------------------:|:------------:|:----------------:|:-------------------:|
|   **`methylKit` pH**   |         42         |            2           |       1      |        N/A       |         N/A         |
| **`methylKit` ploidy** |          -         |           29           |      N/A     |         3        |         N/A         |
|      **`DSS` pH**      |          -         |            -           |      178     |        21        |          11         |
|    **`DSS` ploidy**    |          -         |            -           |       -      |        154       |          17         |
|   **`DSS` ploidy:pH**  |          -         |            -           |       -      |         -        |          53         |

I found it interesting that only 1 DML overlapped between the different pH lists, and there were 3 common between the ploidy lists. This is probably a testament to the different methods used to call differential methylation (Fisher's exact test vs. Wald test).

### DML overlaps with genomic features

For each DSS list, I used `intersectBed` to look at genomic locations of DML in [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/07-Genomic-Location-of-DML.ipynb).

**Table 2.** DML distribution in genomic features for DSS lists

|   **Genome Feature**  | **pH** | **ploidy** | **ploidy:pH** |
|:---------------------:|:------:|:----------:|:-------------:|
|         Total         |   178  |     154    |       53      |
|         Genes         |   123  |     145    |       48      |
|        Exon UTR       |    4   |      8     |       3       |
|          CDS          |   15   |     26     |       6       |
|        Introns        |   104  |     114    |       41      |
|    Upstream flanks    |    2   |      8     |       0       |
|   Downstream flanks   |    2   |      8     |       0       |
|   Intergenic regions  |   27   |     18     |       5       |
|         lncRNA        |    4   |      1     |       2       |
| Transposable elements |   86   |     66     |       14      |
|    Unique C/T SNPs    |    5   |      3     |       1       |

Even though the number of DML are different between the two methods, the genomic locations are basically the same: most DML are in introns, followed by transposable elements. One thing that is important to note is that the percent of DML that overlap with C/T SNPs is much less with DSS than `methylKit` (~2% vs. ~20%).

So, I guess I just have multiple DML lists now? I'll probably move forward with each list separately for enrichment analysis, then make some sort of decision after that. I will also need to find a way to construct a randomization test with `DSS`! My guess is that I could get results from that randomization test sooner rather than later, since `DSS` was less computationally intensive than `methylKit`.

### Going forward

1. Perform enrichment
2. Conduct randomization test with `DSS`
2. Try [EpiDiverse/snp](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
1. Run `methylKit` randomization test on `mox`
5. Investigate comparison mechanisms for samples with different ploidy in oysters and other taxa
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)
6. Update methods
7. Update results
8. Create figures

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
