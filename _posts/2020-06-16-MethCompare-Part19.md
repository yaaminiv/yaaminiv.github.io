---
layout: post
comments: true
title: MethCompare Part 19
tags: MethCompare
---

## Multivariate analysis for compositional data

Last week I reached out to Evan and Julian to see if they could help me decide on a statistical approach. I posted Evan's suggestions in this [issue](https://github.com/hputnam/Meth_Compare/issues/68). In short, he suggested pairing multivariate and generalized linear model approaches, giving me more power to discern if sequencing method influences CpG detection in genome features or methylation status.

### Data transformation

I opened [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.Rmd) to get started. To create my dataframe, I used `cbind` to combine the CpG type and location percentages for each sample. I used the genomic location data the full sample, and not the individual columns for a sample that looked at proportino of a specific methylation status in that genomic location. I figured the point of a multivariate analysis would be to determine if these variables are related in any sense, so I didn't want to add any variables that would be redundant.

Since I was working with proportion data, Evan suggested I borrow methods from compositional data analysis techniques. To do this, I used a centered log-ratio transformation on my matrix:

```
McapCpGMultiTrans <- data.frame(clr(McapCpGMultiMaster)) #Use centered log-ratio transformation
```

This is where I ran into my first point of confusion. Looking over Julian's email, he suggested I use a Hellinger's transformation on my data to give low weights to smaller proportions:

```
McapCpGMultiTrans <- data.trans(McapCpGMultiMaster, method = "hellingers", plot = FALSE) #Hellinger (asymmetric) transformation
```

When I tried this code, I got a data matrix filled with N/As! I wasn't sure why this was happening, so I decided to just continue with the CLR transformation and ask Evan which method would be more suitable later.

### NMDS and ANOSIM

Although we already had stacked barplots for visualization, I wanted to go through the exercise of creating and NMDS to look at any influential loadings. I followed the same process I used for my DNR paper:

1. Create a scree plot to determine the optimal number of axes
2. Confirm that the number of axes returns a low stress value using a Montecarlo permutation
3. Calculate loadings
4. Plot an NMDS with loadings that have p-values < 0.05
6. Conduct global and pairwise ANOSIM tests
7. Conduct post-hoc tests for significant ANOSIM if necessary

At steps 1 and 2, I got errors that suggested I had insufficient data. Additionally, I was unable to create a scree plot, and my stress value was very close to zero. I recorded these errors but decided to proceed.

For *M. capitata*, all loadings were significant below 0.05, but for *P. acuta*, CDS was not an influential loading! Interesting. When I created my NMDS, all samples for the same method were plotted on top of eachother for *M. capitata*. For *P. acuta*, my WGBS and RRBS plotted on top of eachother. However, my MBD-BS samples were separated, and sample 8 looked like an outlier within that group.

Related to my lack of sufficient data, I was able to conduct a global ANOSIM for each species, but I got errors suggesting I had insufficient data when I investigated the significant global ANOSIM results with pairwise tests. During the pairwise tests, all possible permutations were conducted ("complete enumeration"). My pairwise tests were not significant, but since my global ANOSIM was, I have a feeling that I do not have enough statistical power to investigate that difference using this multivariate method.

I knit the script into [this document](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.md#multivariate-data-analysis-with-compositional-data) and shared it with Evan to get his take on my insufficient data and transformation issues. Most importantly, I wonder if his original suggestion to pair multivariate and generalized linear model approaches is still the best option considering I am data limited.

### Going forward

1. [Conduct glm analysis and revise multivariate analysis](https://github.com/hputnam/Meth_Compare/issues/68)
1. [Locate TE tracks](https://github.com/hputnam/Meth_Compare/issues/56)
2. [Characterize intersections between data and TE, and create summary tables]
4. Look into program for mCpG identification

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
