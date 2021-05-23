---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 16
tags: hawaii gigas-ploidy DML methylKit SNP
---

## Reworking pH-DML

When writing up my methods, I realized that I incorrectly re-assigned treatments in `methylKit` when I was shuffling between ploidy and pH treatment assignments! Now that I was using `min.per.group` with `unite`, I couldn't just re-assign treatments because the `min.per.group` would always be based off of the ploidy treatment information, not the pH treatment information. This could mean that I would call DML with less than eight samples per pH treatment! So, back to the code I go.

### Returning to `methylKit`

I opened the R Studio server and [my R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit.R) to recode the treatment re-assignment. I just needed to re-assign treatments the step before `unite`:

```
methylationInformationFilteredCov5T <- methylKit::reorganize(processedFilteredFilesCov5,
                                                             sample.id = sampleMetadata$sampleID,
                                                             treatment = sampleMetadata$pHTreatment) %>%
  methylKit::unite(., destrand = FALSE, min.per.group = 8L) #Re-assign treatment information to filtered and normalized 5x CpG loci, then unite 5x loci with coverage in at least 8/12 samples/treatment
```

With this re-assignment, I had 5,086,421 CpGs with data in at least eight samples per pH treatment. I then used this new object to identify DML! After finishing up on `mox`, I used `rsync` to move my revised DML lists and data to `gannet`.

### Unique C/T SNPs

The next thing I wanted to do was determine how many unique C/T SNPs were in my data in [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/05-BS-SNPer.ipynb). First, I used `cat` to combine all SNP lists:

```
#Combine C/T SNPs into one file
!cat *CT-SNPs.tab >> all-CT-SNPs.tab
```

Then, I used `sort` and `uniq` to pull out all the unique SNPs:

```
!sort all-CT-SNPs.tab \
| uniq \
> unique-CT-SNPs.tab
```

When I looked at the output, I realized there were still duplicate C/T SNPs! The eighth column had information from the VCF file that was unique from each sample, so that was likely preventing me from pulling out the unique SNPs properly. Instead, I used `awk` to pull the first five columns out (chr, bp, ?, C, T), and used that as input into `sort` and `uniq`.

```
!awk '{print $1"\t"$2"\t"$3"\t"$4"\t"$5}' all-CT-SNPs.tab \
| sort \
| uniq \
> unique-CT-SNPs.tab
```

That gave me 289,826 unique C/T SNPs in my data.

### DML characterization

In [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/07-Genomic-Location-of-DML.ipynb), I used my new pH-DML and unique C/T SNP lists to look at DML locations in the genome.

**Table 1** Overlaps between DML and various genome features.

|   **Genome Feature**  | **pH-DML** | **ploidy-DML** | **Common DML** |
|:---------------------:|:----------:|:--------------:|:--------------:|
|         Total         |     42     |       29       |        2       |
|         Genes         |     36     |       25       |        2       |
|        Exon UTR       |      5     |        0       |        0       |
|          CDS          |      7     |        7       |        1       |
|        Introns        |     24     |       18       |        1       |
|    Upstream flanks    |      0     |        0       |        0       |
|   Downstream flanks   |      4     |        1       |        0       |
|   Intergenic regions  |      2     |        3       |        0       |
|         lncRNA        |      3     |        0       |        0       |
| Transposable elements |     16     |        8       |        0       |
|    Unique C/T SNPs    |      6     |        5       |        0       |

### Going forward

1. Test-run [DSS](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#34_DMLDMR_detection_from_general_experimental_design) and [ramwas](https://bioconductor.org/packages/release/bioc/html/ramwas.html)
1. Try [EpiDiverse/snp](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
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
