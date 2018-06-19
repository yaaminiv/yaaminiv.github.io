---
layout: post
comments: true
title: DML Analysis Part 2
tags: virginica MBDSeq DML bedtools
---

## Understanding gene enrichment

TL;DR: I got confused a lot but I think I'm on the right track, but this will take a while.

I switched over to [an R Markdown file](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-11-DML-Analysis/2018-06-14-Gene-Enrichment-Analysis.Rmd) and tried working with [`topGO`](https://bioconductor.org/packages/release/bioc/manuals/topGO/man/topGO.pdf) for gene enrichment. I wanted to use an R package over DAVID to improve the reproducibility of my work.

I set up some code and reformatted by DML-mRNA overlap file in the script. `topGO` requires GOterms as inputs as well, so I tried using `org.Hs.eg.db` to match gene IDs to GOterms. However, I ran into my main problem: I don't have Entrez Gene IDs (official NCBI IDs) for my *C. virginica* genes. Without these, I cannot use any sort of gene enrichment R package, let anlone convert gene IDs to GOterms! Steven suggested I `blastx` the *C. virginica* genome against the UNIPROT database. This would give me UNIPROT accession codes and GOterms. I can find a way to convert UNIPROT accession codes to Entrez Gene IDs to use `topGO`. He also suggested I use DAVID for gene enrichment and compare the results.

I rearraged my R Markdown file and started `blastx`. We'll see how this goes...

Want to know more about my thought process for this analysis? I started using Wordpress to document intermediate thoughts! Click on the "Feed" link in the top right corner of this webpage, or navigate to these links:

- [Gene Enrichment methods](https://genefish.wordpress.com/2018/06/12/dml-analysis-possible-gene-enrichment-methods/)
- [Getting GOterms](https://genefish.wordpress.com/2018/06/15/dml-analysis-how-to-get-goterms/) and [this associated issue](https://github.com/RobertsLab/resources/issues/292)

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
