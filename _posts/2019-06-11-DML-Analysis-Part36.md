---
layout: post
comments: true
title: DML Analysis Part 36
tags: virginica MBDSeq IGV bedtools
---

## Reworking DMR

### Changing `methylKit` parameters

One thing Mac mentioned to me at FROGER was the use of the `cov.bases` in `tileMethylCounts`. The argument `cov.bases` allows me to set the minimum number of bases to cover in a window. Looking at Mac's salmon paper, I saw that she set `cov.bases` to 1, which is different than the default 0. In [my R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd), I also set `cov.bases` to 1 and created 100 bp, 500 bp, and 1000 bp DMR. All of the data and figures I generated are tagged with the date "2019-06-05" and can be found [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-10-25-MethylKit/2018-10-25-Tiling-Analysis).

**Table 1**. Number of DMR identified using different window sizes. Step size and window size were equal.

| **Window Size (bp)** | **Number of DMR** |
|:--------------------:|:-----------------:|
|          100         |         71        |
|          500         |         12        |
|         1000         |         5         |

### Visualizing DMR in IGV

My gut feeling was to go with the 100 bp DMR, just because it gives me a larger dataset to work with. Obviously gut feelings aren't enough, so I visualized the different DMR sizes in IGV.

![Screen Shot 2019-06-11 at 3 51 56 PM](https://user-images.githubusercontent.com/22335838/59313791-3fb72c80-8c67-11e9-96eb-6378bde5e5b7.png)

![Screen Shot 2019-06-11 at 3 52 13 PM](https://user-images.githubusercontent.com/22335838/59313792-3fb72c80-8c67-11e9-89f4-4d5505da9f34.png)

![Screen Shot 2019-06-11 at 3 52 49 PM](https://user-images.githubusercontent.com/22335838/59313793-404fc300-8c67-11e9-865b-0c675012ce33.png)

**Figures 1-3**. 100 bp, 500 bp, and 1000 bp DMR tracks in IGV.

I found that the 100 bp DMR more consistently matched with the location of DML on various chromosomes (Figures 1-3). For example, there would be a genomic region with no DML, but a 500 bp DMR. When I looked closely at these DMR, I found that these were regions with one or two CpG loci with data for only a few samples. Some chromosomes did not have any DMR when looking at the 500 bp or 1000 bp tracks even though they had DML. After looking at the data in IGV, I trust the 100 bp DNMR more, so I'll continue to use that for analyses. I quickly generated separate BEDfiles for hypermethylated and hypomethylated DMR so I could compare that to the [breakdowns I had for hyper- and hypomethylated DML](https://yaaminiv.github.io/DML-Analysis-Part35/). Out of 71 total DMR, 37 are hypermethylated and 34 are hypomethylated.

### Characterizing overlaps with DMR

I returned to [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb) to characterize DMR overlaps with various genome feature tracks. I looked at overlaps for all DMR, as well as hyper- and hypomethylated DMR separately.

**Table 2**. Overlaps between DMR and various genome feature tracks.

|               **Feature**               | **Hypermethylated DMR** | **Hypomethylated DML** | **All DMR** |
|:---------------------------------------:|:-----------------------:|:----------------------:|:-----------:|
|                  Genes                  |            33           |            33          |      66     |
|               Unique Genes              |            33           |            33          |      65     |
|                  Exons                  |            19           |            19          |      38     |
|                 Introns                 |            27           |            24          |      51     |
|       Transposable Elements (All)       |             3           |             8          |      11     |
| Transposable Elements (*C. gigas* only) |             3           |             6          |       9     |
|            Putative promoters           |             1           |             7          |       8     |
|                  Other                  |             2           |             0          |       2     |

### Correcting DML chi-squared tests

Before creating DMR figures, I decided to take a quick DML detour and address a comment Steven gave me. When I initially conducted chi-squared tests with DML, I set the methylated CpGs as the background. While this is an interesting comparison, the methylated CpGs are not the appropriate background, since `methylKit` pulls DML from MBD-enriched loci. In [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Proportion-Test.Rmd), I conducted chi-squared tests for MBD-enriched vs. DML and found significantly different distributions (chi-squared statistic = 342.69, df = 4, p-value < 2.2e-16). I also created a figure for this comparison.

<img width="731" alt="Screen Shot 2019-06-11 at 6 12 26 PM" src="https://user-images.githubusercontent.com/22335838/59316668-7ba4be80-8c74-11e9-840a-cf9e166b5312.png">

**Figure 4**. Comparing overlap proportions between MBD-enriched loci and DML.

### DMR overlap figures

Since DMR are 100 bp and loci are well...1 bp, I decided that comparing distribution of loci with distribution of DMR did not make sense. If I were to do a chi-squared tests, I'd need to use the appropriate background: all the tiles generated by `methylKit` in the sliding window analysis. These 100 bp windows are all possible DMR. I exported all the tiles from `methylkit` in [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd). I then returned to [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb) to characterize the locations of the DMR background. 

**Table 3**. Overlaps between DMR background and various genome feature tracks. There were 152,226 possible tiles.

|               **Feature**               |   **DMR Background**   |
|:---------------------------------------:|:----------------------:|
|                  Genes                  |         142153         |
|               Unique Genes              |          11578         |
|                  Exons                  |          92552         |
|                 Introns                 |          93707         |
|       Transposable Elements (All)       |          25117         |
| Transposable Elements (*C. gigas* only) |          20228         |
|            Putative promoters           |           8238         |
|                  Other                  |           4649         |

I added the background overlap and DMR overlap counts to [this table](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Overlap-Proportions.csv). I found that the distribution of the DMR background and DMR themselves were not significantly different (chi-squared statistic = 5.8078, df = 4, p-value = 0.214). I did, however, get a warning that the chi-squared approximation may be incorrect.

While Mac didn't do a chi-squared test with her salmon DMR, she did create plots that compared the proportion DMR in various genomic features with the DMR background. I decided to follow her precedent and do the same in [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Proportion-Test.Rmd).

<img width="727" alt="Screen Shot 2019-06-12 at 11 10 54 AM" src="https://user-images.githubusercontent.com/22335838/59375324-c1a86380-8d02-11e9-8139-ec6b13332764.png">

**Figure 5**. Comparing overlap proportions between the DMR background and DMR. There were no significant differences in the distribution.

### Going forward

1. Create an annotated table of DML and DMR
2. Conduct a gene enrichment for DML and DMR
3. Work through gene-level analysis
4. Update methods and results
5. Update paper repository
6. Outline the discussion
7. Share draft paper at the next Eastern Oyster Project Meeting
8. Write the discussion
9. Write the introduction
10. Revise my abstract
11. Share the draft with collaborators and get feedback
12. Post the paper on bioRXiv
13. Prepare the manuscript for publication

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
