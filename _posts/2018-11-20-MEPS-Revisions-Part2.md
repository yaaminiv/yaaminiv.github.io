---
layout: post
comments: true
title: MEPS Revisions Part 2
tags: DNR SRM MEPS-Revisions
---

## Protein abundance and environmental data NMDS

### Protein abundance

[Previously](https://yaaminiv.github.io/MEPS-Revisions-Part1/), I conducted an NMDS using hellinger-transformed data and euclidean distances. I also conducted global Analysis of Similarities (ANOSIM) testing, and found a significant result when comparing regions (Puget Sound vs. Willapa Bay). After confirming this approach with Julian, he suggested I also conduct pairwise ANOSIM tests to understand where the differences are coming from. [In this R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-ANOSIM-for-Cluster-Analysis.R), I conducted pairwise ANOSIM tests between all different site combinations.

**Table 1**. Results of pairwise ANOSIM tests between sites for protein abundance data. R values are listed above the diagonal, and p values are listed below the diagonal. Values significant at the 0.05 level are marked by **, and values significant at the 0.10 level are marked by *.

|        | **CI** |   **FB**  |    **PG**   |    **SK**    |   **WB**   |
|:------:|:------:|:---------:|:-----------:|:------------:|:----------:|
| **CI** |    -   | 0.0196793 | -0.07069971 |  -0.05830904 | 0.09259259 |
| **FB** |  0.331 |     -     |   -0.03125  |  0.09486607  |  0.2567829 |
| **PG** |  0.78  |   0.599   |      -      | -0.006138393 | 0.07267442 |
| **SK** |  0.733 |   0.086   |    0.382    |       -      |  0.1540698 |
| **WB** |  0.16  |  0.035**  |    0.192    |    0.079*    |      -     |

Looks like the comparisons driving any regional differences are between Willapa Bay, Fidalgo Bay, and the Skokomish River Delta. Fidalgo Bay is the northern-most site, and Skokomish River Delta is in Hood Canal. It will be interesting to see how the environmental data compares.

### Environmental data

Since my environmental data analysis is new, I created an [R Markdown file](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2018-11-18-Environmental-Data-NMDS.Rmd). Based on my discussion with Julian, I first estimated means and variances for environmental conditions at each site. I originally was going to calculate minimums, maximums, means and variances, but he mentioned that minimum and maximum values will be autocorrelated with means and variances, so there is no need to include them. To calculate these variables, I used a for loop that iterated over dates as character strings and ignored missing values in my calculations. I (obviously) encountered errors doing this, which you can refer to [here](https://github.com/RobertsLab/resources/issues/495).

Once I calculated all the daily values I needed, I log(x+1) transformed my data. My dataframe includes missing values, since Gower's is able to ignore missing values. After transforming my data, I transposed my dataframe so my environmental variables at each site-habitat combination were objects and my descriptors were dates. Originally, I wanted to use my dates as objects and environmental variables as descriptors. This, however, would mean that my dates would be the points clustering. I'm more interested in knowing how my environmental variables cluster in relation to eachother, so I figured it would be better to have those as objects. I can then use dates to understand where the variability comes from (and see if my later dates contributed to differences in variables more than earlier dates...thereby understanding temporal variation and allowing me to answer a certain reviewer comment).

With my transposed dataframe, I was ready to ordinate my data! However, I was unable to use `metaMDS` in the `vegan` package with missing data. In the past, I converted all my N/As to 0s to successfully use `metaMDS`. I don't want to do this again, because then `metaMDS` will count those 0s as similarities, instead of discarding comparisons with missing values like it is supposed to. I posted my issue in the multivariate class Canvas page, so hopefully I get an answer. I can't really move forward until then :/

### Going forward

1. Get environmental data NMDS to work
2. Perform a constrained ordination
3. Discuss all results with Julian
4. Update manuscript with new analyses

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
