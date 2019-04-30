---
layout: post
comments: true
title: DML Analysis Part 32
tags: virginica MBDSeq
---

## Revised finalized general methylation trends

Based on feedback from Katie, Steven, and Alan, I went back to my code and revised *C. virginica* gonad methylation trends.

### Revised timeline

But first...let's revist my grand procalamtion: "I'm going to finish this paper by the end of the month." (lol) I think I can finish my analyses by the end of this week and have some sort of draft discussion. Before the next E2O meeting, I'll definitely have a draft paper ready.

### Characterizing loci locations

In [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-03-18-Characterizing-CpG-Methylation.ipynb), I characterized the location of all 5x CpGs I had data for, sparsely methylated loci, and unmethyalted loci. Katie suggested I make a table with this information for statistical analyses, so I did.

**Table 1**. Locations of 5x CpGs enriched by MBD treatment, methylated loci, sparsely methylated loci, and unmethylated loci. "Other" refers to any loci that did not overlap with exons, introns, transposable elements (all), and putative promoters.

|           **Category**          | **All 5x CpGs** | **Methylated** | **Sparsely methylated** | **Unmethylated** |
|:-------------------------------:|:---------------:|:--------------:|:-----------------------:|:----------------:|
|            **Total**            |    4,304,257    |    3,181,904   |         481,788         |      640,565     |
|         **Unique genes**        |      54,619     |     44,505     |          47,243         |      47,584      |
|     **mRNA coding regions**     |    3,140,744    |    2,437,901   |         303,890         |      398,953     |
|            **Exons**            |    1,366,779    |    1,013,691   |         105,871         |      247,217     |
|           **Introns**           |    1,811,271    |    1,448,786   |         201,553         |      160,932     |
| **Transposable elements (all)** |    1,011,883    |     755,222    |         155,293         |      101,368     |
|  **Transposable elements (Cg)** |     767,604     |     610,208    |         108,858         |      48,538      |
|      **Putative promoters**     |     203,376     |     134,534    |          27,443         |      41,399      |
|            **Other**            |     627,257     |     386,003    |          86,923         |      154,331     |

### Chi-squared tests

For this round of chi-squared tests, I updated my [overlap proportions](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Overlap-Proportions.csv) file. I used [this code](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Proportion-Test.Rmd) to conduct chi-squared tests for various groupings. Every single time, I found that my distributions were significantly different. However, I'm now doubting how effective chi-squared tests are as a statistical approach. Looking at the observed, expected, and residual values from `chisq.test` from my test comparing all CG motifs with those enriched by MBD, I notice that my expected values are not what I want. In comparing these distributions, I want the background proportions (in this case, all CG motifs), to serve as the expected values, but that's not the case:

![Screen Shot 2019-04-29 at 7 14 20 PM](https://user-images.githubusercontent.com/22335838/56937698-ffcf2800-6ab2-11e9-8b16-fb7b96adb154.png)

I psoted [this issue](https://github.com/RobertsLab/resources/issues/685) to get some clarification on my methods.

### Revised figures

In the meantime, I revised my figures!

![1-totalenriched](https://user-images.githubusercontent.com/22335838/56938254-b92efd00-6ab5-11e9-9435-f49676c16451.png)

**Figure 1**. Distribution of CpGs enriched by MBD versus all CpGs in the *C. virginica* genome.

![2-enriched](https://user-images.githubusercontent.com/22335838/56938073-cd262f00-6ab4-11e9-85e9-93ef18ece039.png)

**Figure 2**. Distribution of methylated CpGs versus those enriched by MBD.

![3-allcat](https://user-images.githubusercontent.com/22335838/56938074-cdbec580-6ab4-11e9-89dc-2660a252bf5d.png)

**Figure 3**. Distribution of methylated, sparsely methylated, and unmethylated CpGs versus CpGs enriched by MBD.

![4-dml](https://user-images.githubusercontent.com/22335838/56938075-cdbec580-6ab4-11e9-9734-e79c204fe370.png)

**Figure 4**. Distribution of DML versus methylated CpGs.

Once I sort out my statstical tests I can actually add significances to my figures.

### Going forward

1. Sort out statistical tests
2. Describe (somehow) genes with DML in them
3. Figure out what's going on with the gene background
4. Figure out what's going on with DMR
5. Work through gene-level analysis
6. Update paper repository
7. Start writing the discussion

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
