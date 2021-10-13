---
layout: post
comments: true
title: WGBS Analysis Part 34
tags: manchester topGO
---

## Digging into enrichment results

I'm returning to the [`topGO` analysis](https://yaaminiv.github.io/WGBS-Analysis-Part33/) I did previously. One of Steven's main concerns after looking at the enrichment results was that several enriched GOterms appeared to be related to sperm motility. Since I only used female samples, having terms related to sperm didn't make a lot of sense. Additionally, I wrote and discussed my results in the context of significantly enriched GOterms, but didn't tie back to the genes. So I'm going back to [this R Markdown file](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/12-Functional-Enrichment.Rmd) to look at the enrichment results, annotations, and related genes.

### Revising annotations

The first thing I noticed when I looked at my [results spreadsheet](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/12-functional-enrichment/all-BP-EnrichedGO-DML-withTranscript.csv) was that I never actually added gene product annotations. To do this, I imported the list of gene products and transcript IDs I generated in [this Jupyter Notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/11-GOterm-Annotation.ipynb). Then, I used `merge` to combine both spreadsheets and saved the file [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/12-functional-enrichment/all-BP-EnrichedGO-DML-Product.csv).

When I used `simplifyEnrichment` to visualize the enrichment results, I created a dataframe that clustered my enriched GOterms by semantic similarity. I wanted to append the cluster information to my spreadsheet so I could use that to understand the different gene functions. Cluster 1 is related to cilia and motility, while clusters 2-5 are related to development. It's possible that clusters 2-5 are really a supercluster instead of four clusters with one enriched GOterm each. Looking at my dataframe after merging the product information with the clusters, I saw I was missing information for cluster 4. It was at this point that I realized I should have used `left_join` when combining dataframes (now and upstream). It's likely that the GOterms related to the fourth cluster didn't have any transcript annotations from the annotation table I generated, so those rows were being dropped with `merge`.

I went back to the top of my script and proceeded to use `left_join` instead of `merge`. A couple of significant GO IDs had `topGO` annotations, but were not present in my manual `blastx` annotations. I thought it would be good to keep the GOterms associated with these GO IDs only present in the `topGO` database. I also wanted to pull the transcript IDs associated with these enriched GOterms. I used `genesInTerm` to extract the genes of interest, converted it into a dataframe, then extracted the transcript IDs so they were on separate rows:

```{r}
sigRes.allBPGenes <- genesInTerm(allGOdataBP, whichGO = allRes.allBP$GO.ID[1:10]) #Extract genes associated with each GOterm from topGO's annotations
sigRes.allBPGenes <- as.data.frame(vapply(sigRes.allBPGenes, paste, collapse = ",", character(1L))) #Collapse all transcript IDs into one column and convert into a dataframe. GO IDs will be the rownames
colnames(sigRes.allBPGenes) <- c("transcript")
sigRes.allBPGenes$GOID <- rownames(sigRes.allBPGenes) #Convert row names to a separate column
rownames(sigRes.allBPGenes) <- 1:nrow(sigRes.allBPGenes) #Convert row names to numeric values
sigRes.allBPGenes <- separate_rows(sigRes.allBPGenes, transcript, sep = ",") #Separate transcripts so each transcript is on a separate row
```

I used `left_join` to match this information with `topGO` enrichment scores and GO IDs. As expected, every GO ID had at least one corresponding transcript. Then, I used `left_join` based on GO ID and transcript to add my manual annotations and `methylKit` information. At this step, I realized that the GO IDs not present in my manual annotations did not get any corresponding `methylKit` information, even though there was a transcript ID. I tried for a while to think of a way to complete the annotation process, but got stuck and figured if those cells had `N/A` values, then I would know those were the GOterms only present in the `topGO` database. Finally, I appended the product and cluster information to all rows and saved the output [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/12-functional-enrichment/all-BP-EnrichedGO-DML-Cluster.csv). I repeated this process with the CC GOterms and found an overlap between the genes in each list! I didn't look at cluster information, since there was only one cluster determined by `simplifyEnrichment`. I saved the spreadsheet with product information [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/12-functional-enrichment/all-CC-EnrichedGO-DML-Product.csv).

### Going forward

1. Look into unannotated GOterms and potential nesting
2. Understand function of genes with enriched GOterms
3. Determine if there's a bias towards hyper- or hypomethylated genic DML with enriched functions
2. Report `mc.cores` issue to [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html)
2. Perform randomization test
2. Update `mox` handbook with R information
3. Determine if larval DNA/RNA should be extracted

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
