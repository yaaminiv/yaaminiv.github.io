---
layout: post
comments: true
title: WGBS Analysis Part 36
tags: manchester topGO
---

## Miscellaneous enrichment investigations

I have a two outstanding things I want to look at before I close the book on my enrichment analysis results.

### Unannotated GOterms and potential nesting

When I was creating my annotation lists, I noticed that [not all GOterms from `topGO` were present in the genome annotation I generated](https://yaaminiv.github.io/WGBS-Analysis-Part34/). I want to see if these terms were nested inside of other GOterms that were enriched, or if they were parent terms of other terms present in my annotation.

My biological process GOterms without matching annotations were GO:0001539 (cilium or flagellum-dependent cell motility), GO:0003341 (cilium movement), GO:0060285 (cilium dependent cell motility), GO:0060294 (cilium movement involved in cell motility), GO:0097722 (sperm motility), and GO:0001700 (embryonic development via the syncytial blastoderm). The terms GO:0002165 (instar larval or pupal development), GO:0009791 (post-embryonic development), and GO:0007391 (dorsal closure) had matching annotations for some transcripts but not others. I looked for parent and child terms on the [QuickGO](https://www.ebi.ac.uk/QuickGO/).

Several motility terms were related to eachother, including ones with and without annotations:

![Screen Shot 2021-10-25 at 2 51 58 PM](https://user-images.githubusercontent.com/22335838/138754121-31265ad3-4e16-40af-aee7-c710f4a24c30.png)

I encountered a similar situation looking at two GOterm lineages for developmental terms:

![Screen Shot 2021-10-25 at 3 21 45 PM](https://user-images.githubusercontent.com/22335838/138756934-8d74dad8-4f27-4e62-93f8-3bbdc3bd7ce5.png)

Most of the cellular component GOterms were missing matching annotations, and were all related to eachother:

![Unknown](https://user-images.githubusercontent.com/22335838/138757779-dec5bc4c-e259-473e-a885-33377b917abf.png)

I had one cellular component term with some annotations, and it was not related to the other terms:

![Unknown-2](https://user-images.githubusercontent.com/22335838/138757778-850e60ea-7c0c-49f2-9bdf-69d955d9d40a.png)

I'm not sure why some of the `topGO` GOterms show up in the annotation or not, but my guess is that `topGO` may consider the entire term "family tree" when performing enrichment in a way that's different than the annotation.

### Enrichment of hyper- vs. hypomethylated DML

I wanted to see if the genes with enriched GOterms contained more hyper- or hypomethylated DML. While we found a pretty even split of hyper- and hypomethylated DML, any differential enrichment of these DML may give us additional insight into the function of methylation. To do this, I took my dataframe with enriched GOterms and used it as a transcript index to filter my master DML list:

```{r}
sigRes.allBPMethDiff <- unique(sigRes.allBPProduct %>%
  dplyr::select(., transcript) %>%
  left_join(., allDMLGOtermsFiltered, by = "transcript") %>%
  filter(., GOcat == "P") %>%
  dplyr::select(., geneID, chr, start, end, meth.diff)) #Isolate transcript column; join with DML list; remove all rows that aren't BP GOterms, select gene ID/chr/start/end/meth.diff columns
```

Based on my filtering, I identified 16 unique DML in genes with enriched BP GOterms. Of these 16 DML, 13 were hypermethylated and 3 were hypomethylated. When I did the same thing with my CC GOterms, six of the seven unique DML were hypermethylated. There is definitely an enrichment bias for genes with hypermethylated DML.

### Going forward

1. Update methods
2. Update results
3. Revise discussion
4. Revise introduction
5. Identify journals for submission
6. Format for submission
7. Submit preprint to bioRXiv
8. Submit paper for publication
9. Report `mc.cores` issue to [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html)
10. Perform randomization test
11. Update `mox` handbook with R information
12. Determine if larval DNA/RNA should be extracted

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
