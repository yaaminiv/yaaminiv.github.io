---
layout: post
comments: true
title: DML Analysis Part 20
tags: virginica MBDSeq prop.test
---

## Proportion test results

So far, I've used `bedtools` to find overlaps bewteen DML, DMR, the gene background, and various genome features (exons, introns, mRNA coding regions, and transposable elements). I calculated proportions between DML, DMR, and genome features in [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb), and  overlap proportions between the gene background and genome features in [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-12-02-Gene-Enrichment-Analysis.ipynb). The gene background refers to the output from `unite` in `methylKit` (see [`methylKit` script for more information](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd)).

My next step was to see if these proportions were significantly different from eachother using `prop.test` in [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Proportion-Test.Rmd). I pulled the number of overlaps from my Jupyter notebooks and used that as the number of successes. The line counts for each genome feature file were used as totals. I compared all three proportions, but also did pairwise comparisons between the gene background and either DML or DMR. My `prop.test` output can be found in [this file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Proportion-Test-Results.csv) and in Table 1 below.

**Table 1**. Results from `prop.test` in R. Test results are organized first by the genomic feature overlaps being tested (ex. exons), then by the comparisons included. "All" refers to DML, DMR, and gene background proportions, "DML-GB" for only DML and gene background proportions, and "DMR-GB" for only DMR and gene background proportions. Significant p-values at the 0.05 level are bolded.

|           Feature           |  Test  | Chi-Squared Statistic | df |   P-Value  |
|:---------------------------:|:------:|:---------------------:|:--:|:----------:|
|             Exon            |   All  |         63.44         |  2 | *1.67e-14* |
|             Exon            | DML-GB |         25.58         |  1 | *4.24e-07* |
|             Exon            | DMR-GB |         36.64         |  1 | *1.42e-09* |
|            Intron           |   All  |         136.63        |  2 | *2.15e-30* |
|            Intron           | DML-GB |         19.59         |  1 | *9.62e-06* |
|            Intron           | DMR-GB |         115.04        |  1 | *7.73e-27* |
|             mRNA            |   All  |         10.18         |  2 |   *0.006*  |
|             mRNA            | DML-GB |          2.85         |  1 |    0.09    |
|             mRNA            | DMR-GB |          6.43         |  1 |   *0.01*   |
| Transposable Elements (All) |   All  |         26.13         |  2 | *2.12e-06* |
| Transposable Elements (All) | DML-GB |          0.67         |  1 |    0.41    |
| Transposable Elements (All) | DMR-GB |         24.15         |  1 | *8.90e-07* |
|  Transposable Elements (Cg) |   All  |         14.62         |  2 |  *0.0007*  |
|  Transposable Elements (Cg) | DML-GB |          8.18         |  1 |   *0.004*  |
|  Transposable Elements (Cg) | DMR-GB |          5.48         |  1 |   *0.02*   |

When comparing all three proportions, all proportions were significantly different from eachother. For the DML-GB tests, all comparisons were significant except for mRNA and transposable elements (all) overlaps. It was interesting that the overlap proportions were significantly different for exons and introns, but not mRNA. All DMR-GB comparisons were significant as well. The differences in significance between DML-GB and DMR-GB could be attributed to the way I calculated overlaps. Each overlapping region is listed as one line entry by `bedtools`. DMR overlapping regions can be multiple base pairs long because each DMR is 100 bp. However, DML and gene background overlapping regions can only be one base pair because DML and the gene background are each listed locus by locus. It will be interesting to calculate the actual length of each DMR overlap, then use that in a proportion test.

For now, I can conclude that DML and DMR locations are different from the gene background's location. That will be interesting to interpret in my paper!

### Going forward

1. See how `min_cov`, alignment stringency, or SNPs affect clustering
2. Determine if a formal gene enrichment is necessary
3. If necessary, select the most appropriate gene enrichment method
4. Describe functions of most interesting genes with DML and DMR

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
