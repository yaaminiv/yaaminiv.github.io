---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 88
tags: green-crab-wc RNA-Seq DESeq2
---

## Preliminary gene expression analysis

Ashray is working on a preliminary gene expression analysis with `DESeq2`. We will determine the best methods for understanding how six weeks of exposure to either 25ºC or 5ºC impacted gene expression. Once we have those methods streamlined, we can focus on exploring how genotype also impacts gene expression. In addition to working on getting the transcriptome files ready for him (see [this saga](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part87/)), I'm troubleshooting part of the R code. This is because `DESeq2` doesn't have the best user manual, and my limited familiarity with gene expression analysis means that I have to actually code to remember how to do things! I've sent him a previous script I've worked on for him to use as an additional reference.

### 2026-03-13

#### Quick recap

Ashray is leading the analysis in [this R Markdown script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/06f-gene-expression.Rmd). What's been done so far:

- Took isoform-level count matrix from raw transcriptome and ran it through `DESeq2`
- Tried making volcano plots
- Made preliminary PCA

I outlined the next steps for the analysis for him:

- Import gene-level count matrix
- [Remove contaminated sequences](https://github.com/yaaminiv/wc-green-crab/issues/5)
- Count number of genes with higher expression in 25ºC, and vice versa
- [Annotate differentially expressed genes](https://github.com/yaaminiv/wc-green-crab/issues/6)
- [Create a PCA and heatmap](https://github.com/yaaminiv/wc-green-crab/issues/8)

The next steps for me:

- Determine the best plan forward for functional level analysis
  - Option 1: Manually looking at certain gene groups (ex. HSPs, ubiquitinases, oxidative stress genes)
  - Option 2: WGCNA and `GO-MWU`
  - Option 3: ASCA and `GoSeq` enrichment for clusters
- Provide scaffolding for poster creation

I think Option 1 would be a good option for breaking up a heatmap by function. While Option 2 seems to be "gold-standard" for RNA-Seq analysis right now, I think Option 3 is more powerful and would dovetail nicely with the metabolomics analysis. I'll have to see how long it takes to complete the tasks that are already at hand, and what path is more feasible with the time remaining before WCBSURC.

#### Reviewing Ashray's code

The last thing I wanted to do was review Ashray's script. Because of the back-and-forth nature of the analysis, I've noticed that his code is not well-annotated, and there are often superfluous code chunks that either generate incorrect datasets or make it hard to figure out what is going on. I wanted to review his script to point out all the places where the code could be made more reproducible. That issue can be found [here](https://github.com/yaaminiv/wc-green-crab/issues/7). This will also make it easier for me to review his code and see what he's working on.

### Going forward

1. Use `DESeq2` for preliminary temperature-specific gene expression analysis
2. Determine methods for functional analysis
3. Compare `DESeq2` with `edgeR` and pick a method to move forward with
4. Repeat analysis with clean transcriptome and fuller annotations
5. Identify temperature- and genotype-specific differentially expressed genes at the end of the experiment
6. Identify genes influenced by both temperature and time
7. Additional strand-specific analysis in the supergene region
8. Examine HOBO data from 2023 experiment
9. Demographic data analysis for 2023 paper

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
