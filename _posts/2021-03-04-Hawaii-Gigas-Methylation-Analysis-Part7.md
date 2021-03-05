---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 6
tags: hawaii gigas-ploidy bismark mox methylkit
---

## Preliminary methylation investigation

While my files are aligning to the new genome, I want to use the files I have to test out some workflows! First on the docket is getting a preliminary methylation assessment from [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html).

### Fixing genome preparation parameters

But first...my [`bismark`](https://github.com/FelixKrueger/Bismark) script failed RIP. Looking at the slurm file, I saw this error:

![Screen Shot 2021-03-04 at 8 43 52 AM](https://user-images.githubusercontent.com/22335838/110013468-828c2400-7cd6-11eb-9554-b9402ab98e49.png)

I used `--path_to_bowtie` in previous genome preparations and it's an argument for the alignment step, so I was a little confused. I looked at the [Bismark User Guide](https://rawgit.com/FelixKrueger/Bismark/master/Docs/Bismark_User_Guide.html#i-bismark-genome-preparation), I saw that `--path_to_aligner` is the argument that should be used instead! I made that change in [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/03-bismark-roslin.sh), then started running it again.

Then I ran into my second problem:

![Screen Shot 2021-03-04 at 10 44 28 AM](https://user-images.githubusercontent.com/22335838/110013545-9b94d500-7cd6-11eb-8d05-c9bf2949f48e.png)

NC_001276.1 is the mitochondrial sequence ID, and it's at the end of the genome. Since `bismark` is seeing repeated chromosome numbers, I thought it must be pulling information from the other two FASTA files in that folder: the original Roslin genome and the separate mitochondrial sequence. I removed these files from the directory, and used `rsync` to move the combined genome file I created to [this location on `gannet`](https://gannet.fish.washington.edu/spartina/project-oyster-oa/Haws/data/cgigas_uk_roslin_v1_genomic-mito.fa). I reran my script, and it started running with no errors! I checked on it after some time, and found that the genome was fully prepared and that it was starting to align samples to the bisulfite converted genome.

### Methylation assessment

Now that I knew `bismark` was running, I wanted to complete a preliminary methylation assessment. This would not only allow me to play with `methylKit` code (it's been a while), but also get an understanding for any differences in methylation between diploid and triploid oysters, or between treatments. I definitely want to use the PCA `methylKit` produces in the manuscript, and look at the methylation histograms and correlation plot it produces! There's definitely a better way to identify DML than two pairwise comparisons, but if there aren't any differences between ploidy status or OA treatment, then I can treat the experiment like a single-variable study.

To start, I created [this R Markdown script](https://github.com/RobertsLab/project-oyster-oa/tree/master/code/Haws/04-methylKit.Rmd). I downloaded the merged CpG coverage files from `gannet` to use in the analysis, similar to [what Mac did for the MethCompare project](https://github.com/hputnam/Meth_Compare/blob/master/code/MethCompare_methylKit_analysis.R). After downloading the files, I used `methRead` to import the coverage files. I previously used `processBismarkAlign`, but I'm working with merged coverage files this time, so I used `methRead` instead. I still used `filterByCoverage` to ensure that there was a minimum 5x coverage for each locus, and I excluded data with coverage in the 99.9th percentile:

```{r}
setwd(dir = "/Users/yaamini/Documents/project-oyster-oa/analyses/Haws_04-methylKit/") #Set working directory inside code chunk
processedFiles <- methylKit::methRead(analysisFiles,
                                      sample.id = list("2H-1", "2H-2", "2H-3", "2H-4", "2H-5", "2H-6",
                                                       "2L-1", "2L-2", "2L-3", "2L-4", "2L-5", "2L-6",
                                                       "3H-1", "3H-2", "3H-3", "3H-4", "3H-5", "3H-6",
                                                       "3L-1", "3L-2", "3L-3", "3L-4", "3L-5", "3L-6"),
                                      assembly = "oyster_v9",
                                      treatment = c(rep(0, times = 12),
                                                    rep(1, times = 12)),
                                      pipeline = "bismarkCoverage",
                                      mincov = 2) #Process files. Treatment specified based on ploidy status. Use mincov = 2 to quickly process reads.
```

```{r}
processedFilteredFilesCov5 <- methylKit::filterByCoverage(processedFiles,
                                                          lo.count = 5, lo.perc = NULL,
                                                          high.count = NULL, high.perc = 99.9) #Filter coverage information for minimum 5x coverage, and remove PCR duplicates by excluding data in the 99.9th percentile of coverage with hi.perc = 99.9.
```

After each of these steps, I used `save.image` to save the R data. R Studio crashed very frequently when I was testing out code, and I wanted to save the output of these time-consuming steps!

I looked at descriptive and comparative statistics, and saved any plots in [this folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_04-methylKit/general-stats). For the descriptive statistics, I got sample-specific CpG coverage and methylation information. Skimming through the plots, I saw the standard spikes in CpG methylation at 0%, and a super smol spike the end of the distribution. Coverage looked relatively consistent, but sample 7 had more reads covered at higher depths than other samples. I produced a [correlation plot](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/general-stats/Full-Sample-Pearson-Correlation-Plot-FilteredCov5Destrand.jpeg), [clustering diagram](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/general-stats/Full-Sample-CpG-Methylation-Clustering-FilteredCov5Destrand.jpeg), [PCA](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/general-stats/Full-Sample-Methylation-PCA-FilteredCov5Destrand.jpeg), and [scree plot](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/general-stats/Full-Sample-Methylation-Screeplot-FilteredCov5Destrand.jpeg) for the comparative analysis. The clustering diagram didn't reveal any ploidy or treatment similarities, and neither did the PCA. Sample concordance was at least 85%, which further supports the idea that there aren't ve3ry many differences in methylation patterns between samples. I'm curious to see how removing SNP data will affect the analysis. I also saw two potential outliers in the PCA (2H-1, 3H-2), so I'll also need to make sure that the DML have data represented in at least 10/12 samples (or something like that).

### Going forward

1. Create covariate matrix and complete pairwise DML assessment in `methylKit`
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
