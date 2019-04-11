---
layout: post
comments: true
title: DML Analysis Part 31 
tags: virginica MBDSeq
---

## Finalized general methylation trends

After taking many steps backwards and only some steps forward in [this issue](https://github.com/RobertsLab/resources/issues/675), Sam, Steven, and I chatted about the best way to characterize methylation trends in *C. virginica*. We decided it would be best to use [this file](http://gannet.fish.washington.edu/Atumefaciens/20190312_cvir_gonad_bismark/total_reads_bismark/cvir_bsseq_all_pe_R1_bismark_bt2_pe.bismark.cov.gz) Sam made, which is a concatenation of all CpGs in my data, each with one coverage and percent methylation metric.

### New method

In [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-03-18-Characterizing-CpG-Methylation.ipynb), I downloaded Sam's file, counted lines, and filtered out all loci with a minimum of 5x coverage.

```
!awk '{if ($5+$6 >= 5) { print $1, $2-1, $3, $4, $5+$6}}' cvir_bsseq_all_pe_R1_bismark_bt2_pe.bismark.cov \
> 2019-04-09-All-5x-CpGs.bedgraph
```

I then identified methylated, partially methylated, and unmethylated loci.

```
awk '{if ($4 >= 50) { print $1, $2, $3, $4 }}' 2019-04-09-All-5x-CpGs.bedgraph \
> 2019-04-09-All-5x-CpG-Loci-Methylated.bedgraph

awk '{if ($4 < 50) { print $1, $2, $3, $4}}' 2019-04-09-All-5x-CpGs.bedgraph \
| awk '{if ($4 > 10) { print $1, $2, $3, $4 }}' \
> 2019-04-09-All-5x-CpG-Loci-Sparsely-Methylated.bedgraph

awk '{if ($4 <= 10) { print $1, $2, $3, $4 }}' 2019-04-09-All-5x-CpGs.bedgraph \
> 2019-04-09-All-5x-CpG-Loci-Unmethylated.bedgraph
```

Here's the breakdown:

- 14,458,703 CG motifs in the *C. virginica* genome
- 14,026,131 CpGs with data in the concatenation file (97.0%)
- 4,304,257 loci with 5x coverage (30.7%)
- 3,181,904 methylated loci (73.9%)
- 481,788 sparsely methylated loci (11.2%)
- 640,565 unmethylated loci (14.9%)

Seeing a roughly 75% methylation rate seemed weird, given how methylation rates in *C. gigas* were low. Sam suggested I look at my data in IGV to confirm "it's legit."

![Screen Shot 2019-04-10 at 10 37 14 AM](https://user-images.githubusercontent.com/22335838/55901685-b27c2c80-5b7e-11e9-82a3-7c452d409493.png)

![Screen Shot 2019-04-10 at 10 33 47 AM](https://user-images.githubusercontent.com/22335838/55901688-b445f000-5b7e-11e9-857a-f22399321423.png)

![Screen Shot 2019-04-10 at 10 34 09 AM](https://user-images.githubusercontent.com/22335838/55901692-b60fb380-5b7e-11e9-8601-ab9cfab448d9.png)

**Figures 1-3**. All CpGs, methylated, sparsely methylated, and unmethylated loci in all *C. virginica* gonad samples.

Looking at the screenshots, it does seem like most of my data is methylated. It's possible that gonad tissue has a higher methylation rate than ctenidia (what Mac used to describe *C. gigas* methylation trends), or that there really is a species difference. It may also be beneficial to set a 75% cutoff to define a locus as methylated. These are things I need to dig into in my discussion. In the meantime, remade my frequency distribution with [this code](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2019-03-19-Characterizing-CpG-Methylation.Rmd).

![Screen Shot 2019-04-10 at 11 52 07 AM](https://user-images.githubusercontent.com/22335838/55905558-1276d100-5b87-11e9-8a34-61e2647325e0.png)

**Figure 4**. Frequency distribution of methylated CpGs in *C. virginica* gonad data.

Using the new list of methylated loci, I characterized the location of the loci in the *C. virginica* genome. Methylated CpGs were found primarily in genic regions, with 2,437,901 (76.6%) loci in 44,505 unique genes. Methylated loci were also concentrated in exons, with 1,013,691 CpGs (31.9%) in exons versus 1,448,786 (45.5%) loci in introns. Transposable elements contained (TE-all) 755,222  methylated CpGs (23.7%), with 610,208 loci (19.2%) overlapping with TE-Cg. Putative promoter regions 1 kb upstream of transcription start sites overlapped with 134,534 loci (4.2%). There were 386,003 methylated loci (12.1%) that did not overlap with either exons, introns, transposable elements, or promoter regions.

### Chi-squared tests and figures

I intially thought I would make one figure comparing total CpGs, methylated CpGs, and DML, but Steven said I should only compare a list of interest to its background. This means I need to make two separate figures: one to compare total and methylated CpGs, and one to compare methylated CpGs and DML. 

I quickly went into [this Jupyter notebook]() to count how many CG motifs did not overlap with either exons, introns, transposable elements (all), or putative promoters. I found 4760,788 CpGs did not overlap with any characterized region. Then, I created [this file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Overlap-Proportions.csv) in Excel (there's probably a way to automate this but I can't think of it right now) with genomic locations for all CpGs, methylated CpGs, and DML. From my chi-squared test of homogeneity in [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Proportion-Test.Rmd), I found that the distribution of methylated CpGs in the *C. virginica* genome was different than the distribution of all CpGs (chi-squared = 5,028,800, df = 4, p-value < 2.2e-16). Similarly, the distribution of DML was different than the distribution of methylated CpGs (chi-squared = 347.01, df = 4, p-value < 2.2e-16). I couldn't figure out the best way to conduct a post-hoc test, so instead I made figures.

<img width="801" alt="Screen Shot 2019-04-10 at 11 41 02 PM" src="https://user-images.githubusercontent.com/22335838/55936077-20aa0900-5bea-11e9-8cfa-3ae66b836c67.png">

**Figure 5**. Distribution of total CpG and methylated CpG in the C. virginica genome

<img width="801" alt="Screen Shot 2019-04-10 at 11 40 52 PM" src="https://user-images.githubusercontent.com/22335838/55936080-21db3600-5bea-11e9-853a-8d61386d8f98.png">

**Figure 6**. Distribution of methylated CpG and DML

### Going forward

1. Figure out post-hoc test for chi-squared test
2. Describe (somehow) genes with DML in them
3. Figure out what's going on with the gene background
4. Figure out what's going on with DMR
5. Work through gene-level analysis
6. Update paper repository
7. Start writing the discussion
8. Draft timeline to finish the paper by the end of the month

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
