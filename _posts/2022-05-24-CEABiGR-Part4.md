---
layout: post
comments: true
title: CEABiGR
tags: virginica gene-expression CEABiGR
---

## Gene expression variability analyses

Shelly and I worked on a gene expresion variability analysis in [this R Markdown script](). A while back, Laura showed us a PCA of female gene expression data where the expression data from the low pH samples were visually less variable than those in ambient pH conditions. This seems in line with our hypothesis that environmentally-sensitive DNA methylation reduces transcriptional noise. Before we could try and integrate transcriptional noise with methylation and expression data similar to [Wu et al. (2020)](https://doi.org/ 10.1371/journal.ppat.1008397), Shelly and I wanted to quantify differneces in gene expression variation between treatments for each sex.

To do this, we used [gene-level coefficient of variation data](https://github.com/sr320/ceabigr/blob/main/code/CoV_exp_x_meth.Rmd) Sam produced from `ballgown` FPKM output. We used a generalized linear model (ANOVA) to determine if there were differences in CoV by treatment for males and females separately. While we found significant differences, plotting boxplots of CoV by treatment didn't make us feel good about that significant p-value. It's possible that while we have a significant difference, the effect size is small. 

Shelly and I weren't sure how to proceed, so we posted [this Github Discussion](https://github.com/sr320/ceabigr/discussions/61). Hopefully we get some ideas on how to proceed before doing an integrative transcriptional noise analysis! We're also going to read some papers and look for other analysis options.

### Going forward

1. Read gene expression variability papers
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
