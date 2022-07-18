---
layout: post
comments: true
title: CEABiGR
tags: virginica DML CEABiGR
---

## Genomic locations of DML

I did a first pass on DML genomic location in [this notebook](https://github.com/sr320/ceabigr/blob/main/code/Genomic-Location-of-DML.ipynb) a while back, but at the end of a CEABiGR meeting I realized I needed to remove SNPs from the DML. To do this, I first redownloaded the BS-SNPer output from from `gannet` in [this notebook](https://github.com/sr320/ceabigr/blob/main/code/General-Methylation-Landscape.ipynb). In [this notebook](https://github.com/sr320/ceabigr/blob/main/code/Generating-Genome-Feature-Tracks.ipynb), I then concatenated all C->T SNPs and identified unique SNPs to create a [C->T SNP genome feature track](https://github.com/sr320/ceabigr/blob/main/genome-features/unique-CT-SNPs.tab). I used `subtractBed` to remove C->T SNPs from the putative female and male DML in [this notebook](https://github.com/sr320/ceabigr/blob/main/code/Genomic-Location-of-DML.ipynb), then determined the genomic locations of the DML. I used the DML genome location information to run chi-squared tests and determine if the number of DML in a genome feature differed significantly from the distribution of 10x CpGs.

**Table 1**. DML-feature overlaps and associated chi-squared P-values

|    **Feature**    | **Female DML** | **Female DML Chi-Squared P-value** | **Male DML** | **Male DML Chi-Squared P-value** |
|:-----------------:|:--------------:|:----------------------------------:|:------------:|:--------------------------------:|
|       Total       |       89       |                 N/A                |     2916     |                N/A               |
|        Gene       |   74 (83.1%)   |                 N/A                | 2322 (79.6%) |                N/A               |
|      Exon UTR     |    8 (9.0%)    |          0.106115408445601         |  190 (6.5%)  |       4.93391020603575e-08       |
|        CDS        |   29 (32.6%)   |        2.13354244672506e-08        |  815 (27.9%) |       1.96810864720674e-131      |
|       Intron      |   37 (41.6%)   |          0.607674107438834         | 1332 (45.7%) |       1.15727576627379e-15       |
|  Upstream Flanks  |    4 (4.5%)    |          0.999999999982003         |   60 (2.1%)  |       2.01744854185524e-12       |
| Downstream Flanks |    4 (4.5%)    |          0.999999999982003         |  163 (5.6%)  |        0.00297688587400269       |
|       lncRNA      |    1 (1.1%)    |          0.999999999982003         |   52 (1.8%)  |         0.772437558180899        |
|         TE        |    1 (1.1%)    |          0.49408763658884          |  144 (4.9%)  |         0.186665654830967        |
|     Intergenic    |    7 (7.9%)    |        6.03052947450674e-07        |  279 (9.6%)  |       7.93581496923608e-147      |

It's interesting that only the number of female DML in CDS and intergenic regions differed significantly from 10x CpGs, while the number of male DML in exon UTR, CDS, introns, upstream flanks, downstream flanks, and intergenic differed significantly from 10x CpGs.

### Going forward

1. Update methods and results
2. Quantify methylation level of each gene for various treatment:sex combinations
2. Read gene expression variability papers
2. Continue with gene expression variability analysis
2. Tune sPLS parameters
3. Run sex-specific SPLS
4. Identify drivers
3. Revise with new expression data from Ariana

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
