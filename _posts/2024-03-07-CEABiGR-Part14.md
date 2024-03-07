---
layout: post
comments: true
title: CEABiGR Part 14
tags: virginica CEABiGR gene-expression alt-splicing topGO
---

## Analyzing changes in the maximum number of transcripts expressed

Now that I have a more solid analytical approach developed [using the predominant isoform data](https://yaaminiv.github.io/CEABiGR-Part12/) and [other annotations](https://yaaminiv.github.io/CEABiGR-Part13/), I'm ready to tackle the maximum number of transcript analyses. I conducted my analyses in [this script](https://github.com/sr320/ceabigr/blob/main/code/75-max-transcript-enrichment.qmd).

### Initial statistical testing

After importing and formatting all data, I conducted a simple z-test to understand if the number of genes with changes in the maximum transcript was different than what would be expected by chance. I did this in three ways: 1) increases in maximum transcripts, 2) decreases in maximum transcripts, and 3) changes to maximum transcripts:

```
maxTransIncFemTest <- prop.test(x = 1529, n = 39104, p = 0.33,
                                alternative = "two.sided",
                                conf.level = 0.95) #Conduct a 1-sample z test
maxTransIncFemTest$statistic #X-squared = 14965
maxTransIncFemTest$p.value #p = 0. Increases in max transcripts happen significantly less than what would be expected by random chance
```

```
maxTransDecFemTest <- prop.test(x = 2437, n = 39104, p = 0.33,
                                alternative = "two.sided",
                                conf.level = 0.95) #Conduct a 1-sample z test
maxTransDecFemTest$statistic #X-squared = 12671.25
maxTransDecFemTest$p.value #p = 0. Decreases in max transcripts happen significantly less than what would be expected by random chance
```

```
maxTransFemTest <- prop.test(x = (1529 + 2437), n = 39104, p = 0.5,
                                alternative = "two.sided",
                                conf.level = 0.95) #Conduct a 1-sample z test
maxTransFemTest$statistic #24847.36
maxTransFemTest$p.value #p = 0. Changes in max transcripts happen significantly less than what would be expected by random chance
```

In all cases, the changes to the number of maximum transcripts in female samples occured less than what would be expected by random chance. The same was true for males.

### Enrichment test

The next step was to conduct an enrichment test to see if there were any biological processes overrepresented in genes with changes in the maximum number of transcripts expressed. I specifically chose to do this separately for genes with increases or decreases in the maximum number of transcripts since transcript variation can have a confounding biological role. Just like [with the predominant isoform data](using the predominant isoform data](https://yaaminiv.github.io/CEABiGR-Part12/), I used `topGO` to conduct the enrichment.

**Table 1**. Number of enriched biological process GOterms for each sex x maximum transcript change category

| **Sex** | **Increased Expression** | **Decreased Expression** |
|:-------:|:------------------------:|:------------------------:|
| Females |            211           |            469           |
|  Males  |            391           |            238           |

All enrichment output files can be found [here](https://github.com/sr320/ceabigr/tree/main/output/75-max-transcript-enrichment), including files with gene annotations. I could now [close this issue](https://github.com/sr320/ceabigr/issues/94)!

### Influence of methylation

To understand how methylation may or may not affect all of this, I examined the connection between methylation and enriched BP GOterms and methylation and changes in the maximum number of transcripts. For the first part, I again broke this down into increases in transcript count due to low pH or decreases. Both of these explorations yielded some very uninteresting plots that probably won't even make it past this Github repository. Just counting the number of DML in genes with changes in transcript count, I couldn't see any patterns.

**Tables 2-3**. Number of DML in genes based on difference in maximum transcript counting for females (top) and males (bottom)

| **DML Count** | **Difference** | **Number of Genes** |
|:-------------:|:--------------:|:-------------------:|
|       0       |       -17      |          1          |
|       0       |       -16      |          1          |
|       0       |       -15      |          1          |
|       0       |       -13      |          1          |
|       0       |       -10      |          2          |
|       0       |       -9       |          3          |
|       0       |       -8       |          1          |
|       0       |       -7       |          3          |
|       0       |       -6       |          5          |
|       0       |       -5       |          10         |
|       0       |       -4       |          26         |
|       0       |       -3       |          72         |
|       0       |       -2       |         273         |
|       0       |       -1       |         1624        |
|       0       |        1       |         1067        |
|       0       |        2       |         106         |
|       0       |        3       |          15         |
|       0       |        4       |          5          |
|       0       |        5       |          1          |
|       0       |        7       |          1          |
|       1       |       -3       |          1          |
|       1       |        1       |          1          |
|       2       |       -2       |          4          |
|       2       |       -1       |          2          |

| **DML Count** | **Difference** | **Number of Genes** |
|:-------------:|:--------------:|:-------------------:|
|       0       |       -7       |          1          |
|       0       |       -5       |          2          |
|       0       |       -4       |          3          |
|       0       |       -3       |          13         |
|       0       |       -2       |          87         |
|       0       |       -1       |         863         |
|       0       |        1       |         2134        |
|       0       |        2       |         325         |
|       0       |        3       |          72         |
|       0       |        4       |          26         |
|       0       |        5       |          13         |
|       0       |        6       |          3          |
|       0       |        7       |          3          |
|       0       |        8       |          1          |
|       0       |       13       |          1          |
|       1       |       -3       |          1          |
|       1       |       -2       |          7          |
|       1       |       -1       |          57         |
|       1       |        1       |          95         |
|       1       |        2       |          20         |
|       1       |        3       |          5          |
|       1       |        4       |          4          |
|       1       |        6       |          1          |
|       2       |       -2       |          2          |
|       2       |       -1       |          16         |
|       2       |        1       |          58         |
|       2       |        2       |          20         |
|       2       |        3       |          2          |
|       2       |        4       |          2          |
|       2       |        7       |          2          |
|       3       |       -3       |          3          |
|       3       |       -2       |          6          |
|       3       |       -1       |          18         |
|       3       |        1       |          21         |
|       3       |        2       |          9          |
|       3       |        3       |          3          |
|       3       |        4       |          3          |
|       4       |       -1       |          4          |
|       4       |        1       |          8          |
|       4       |        2       |          4          |
|       5       |        3       |          10         |
|       6       |       -2       |          6          |
|       7       |        3       |          7          |
|       9       |        1       |          18         |
|       13      |        1       |          13         |
|       14      |       -1       |          14         |
|       15      |       -1       |          15         |
|       17      |       -1       |          17         |
|       25      |        1       |          25         |


[Another issue closed](https://github.com/sr320/ceabigr/issues/99).

### Plots on plots on plots

Then I proceeded to make some plots that are not exciting enough to be in the main text of the manuscript.

![Screenshot 2024-03-07 at 3 12 21 PM](https://github.com/sr320/ceabigr/assets/22335838/ff714090-8b03-44d1-b4fd-094cf6b033f6)

**Figure 1**. Methylation vs. expression in control and treatment conditions for genes with an increase in transcripts, decrease in transcripts, or no change in maximum transcript count

![Screenshot 2024-03-07 at 3 12 54 PM](https://github.com/sr320/ceabigr/assets/22335838/8cf0638d-af2e-45a1-aa0b-b3f631fdc17f)

**Figure 2**. Change in methylation for genes with an increase in transcripts, decrease in transcripts, or no change in maximum transcript count

But, it meant that I could close [this issue](https://github.com/sr320/ceabigr/issues/89)!

### The main event: a model

I wanted to understand the effect of a change in gene methylation, DML count, change in gene expression, and gene length on the difference in maximum transcript expressed. In my head, there were three different potential explanatory variables:

1. Numerical difference in transcript count
2. Categorical differences (increase, decrease, or no change)
3. Binary differences (change or no change)

I couldn't figure out how to actually do the second option, so I ran two models:

```
maxTransFemModel <- glm(difference ~ change_meth + DMLcount +
                          log2(change_exp + 1) + log10(length),
                        data = maxTransFemWideDMLLengths) #Model the difference in maximum transcripts based on change in methylation, DML count, change in expression, and gene length
```

```
maxTransFemModel2 <- glm(I(difference == 1) ~ change_meth + DMLcount +
                         log10(length) + log2(change_exp + 1),
                       family = binomial(),
                       data = maxTransFemWideDMLLengths) #Create a binomial model, where 1 = the predominant isoform shifted when comparing low and control pH conditions. Use change in methylation, gene length, and change in gene expression as explanatory variables
```

The output for all models can be found [here](https://github.com/sr320/ceabigr/tree/main/output/75-max-transcript-enrichment). For females, only gene expression and length were significant. This was true for the numerical male model, but for the binomial male model change in methylation had marginal significance. Yet [another issue](https://github.com/sr320/ceabigr/issues/96) bites the dust.

### Going forward

1. Update maximum transcript methods
2. Update maximum transcript results
2. Perform formal DML enrichment analysis
2. Update methods and results
2. Compare with Aranda and Liew models in discussion section
4. Read recent papers
3. Write discussion
4. Write abstract
5. Outline introduction
6. Write introduction
7. Send to co-authors for approval
8. Submit to journal special issue

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
