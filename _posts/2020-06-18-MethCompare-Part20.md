---
layout: post
comments: true
title: MethCompare Part 20
tags: MethCompare glm
---

## Preliminary follow-up GLMs

Previously, [I tried using NMDS and ANOSIM](https://yaaminiv.github.io/MethCompare-Part19/) to understand if sequencing method influenced differences in proportions of CpGs in various methylation statuses or genomic locations. I'm still waiting on a response to some questions I had, but in the meantime I'll try using generalized linear models to get at some of these differences. Since we have small sample sizes, using a combination of approaches gives us more power. Specifically I will be testing if sequencing method or genomic location influences the proportion of CpGs in a particular methylation status.

In [this script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.Rmd), I started creating my models. Evan suggested I use `glmmTMB` to run beta family models with a logit link. Since the proportions of highly, moderately, and lowly methylated CpGs add up to 1, I need to run separate models:

```
McapCpGHighModel <- glmmTMB(percentMeth ~ CDS + Introns + Upstream.Flanks + Downstream.Flanks + Intergenic + seqMethod,
                            family = beta_family(link = "logit"),
                            data = McapCpGMasterHigh) #Run the full model (all genomic locations) using a beta distribution and a logit link
```

When I ran the full model, I got a convergence issue error. Digging into `glmmTMB` vignettes, it suggested that my model could be overparametrized. This made sense, since I have minimal data and five model parameters. I decided to run two models: one for gene features and another for non-gene features. First, I ran a null model (without sequencing method) and a full model (with sequencing method):

```
McapCpGHighModel.GeneNull <- glmmTMB(percentMeth ~ CDS + Introns,
                                     family = beta_family(link = "logit"),
                                     data = McapCpGMasterHigh) #Run null model (without sequencing method) using a beta distribution and a logit link
McapCpGHighModel.Gene <- glmmTMB(percentMeth ~ CDS + Introns + seqMethod,
                                 family = beta_family(link = "logit"),
                                 data = McapCpGMasterHigh) #Run model using a beta distribution and a logit link
```

Then, I compared these models with a likelihood ratio test:

```
McapCpGHighModel.GeneANOVA <- anova(McapCpGHighModel.Gene, McapCpGHighModel.GeneNull, test = "LRT") #Compare the null and treatment models with a likelihood ratio test. 
McapCpGHighModel.GeneANOVA #Look at ANOVA output
```

Finally, I looked at the summary output for the best model using `summary`. Usualy, the best-fit model was one that included the sequencing method. I'm not sure how (or if) to compare the gene and non-gene models, but AIC might be useful in that respect.

Still have questions, but at least it's something to discuss at our meeting!

### Going forward

1. [Finalize analysis](https://github.com/hputnam/Meth_Compare/issues/68)
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
  
