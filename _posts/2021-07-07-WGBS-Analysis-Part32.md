---
layout: post
comments: true
title: WGBS Analysis Part 32
tags: manchester topGO simplifyEnrichment
---

## Miscellaneous analyses

After updating my methods and results sections with information from [my 4 vs. 4 analysis](https://yaaminiv.github.io/WGBS-Analysis-Part31/), I realized there were a few things I needed to complete before sending my draft to my Reading Committee.

### Modifying GOterm annotations

The first thing I wanted to do was add methylation difference information from `methylKit` to the GOterm annotations. This way, I could see if certain genes or biological processes were more hyper- or hypomethylated in my low pH samples. I returned to [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/11-GOterm-Annotation.ipynb) and modified my `paste | awk` code to include the column with methylation difference information:

```
#Column 5 = meth.diff

!paste DML-pH-50-Cov5-All.GeneIDs ../../output/10_DML-characterization/DML-pH-50-Cov5-All-Gene-wb.bed \
| awk -F'\t' -v OFS='\t' '{print $1, $2, $3, $4, $5}' \
| sort \
> DML-pH-50-Cov5-All.GeneIDs.geneOverlap
```

Since all of the files build off of eachother in my code, my final annotation file now included methylation difference information.

I then wanted to count the number of genes and transcripts associated with the enriched GOterms. I opened [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/12-Functional-Enrichment.Rmd), then created a dataframe with only the enriched GOterms (*P*-value < 0.01). I merged the enriched GOterms with the annotation information, then counted the number of unique genes or transcripts:

```{r}
sigRes.allBP <- allRes.allBP[1:35,c(1, 6)] #Filter significantly enriched GOterms, only keep GOID and p-value
colnames(sigRes.allBP) <- c("GOID", "p.value") #Change column names
head(sigRes.allBP)
```

```{r}
sigRes.allBPAnnot <- merge(sigRes.allBP, allDMLGOtermsBP, by = "GOID") #Additional annotations
sigRes.allBPAnnot <- unique(sigRes.allBPAnnot[,-11]) #Drop GOcat column and retain only unique lines
length(unique(sigRes.allBPAnnot$geneID)) #20 unique genes
length(unique(sigRes.allBPAnnot$transcript)) #42 unique genes
head(sigRes.allBPAnnot) #Confirm formatting
```

Interestingly, there were 20 genes associated with enriched biological process GOterms, but 42 transcripts! This tells me that the DML associated with enriched GOterms are found in genes that produce multiple transcripts. This could be important for alternative splicing as a way to control transcription. I repeated this process with the molecular function GOterms, and found that they were associated with 12 unique genes and 50 unique transcripts.

### Visualizing enriched terms with `simplifyEnrichment`

In the same R Markdown script, I decided to create heatmaps similar to [Chille et al. (2021)](https://www.biorxiv.org/content/10.1101/2021.04.14.439692v1) to visualize my enriched GOterms. I started by installing the package [`simplifyEnrichment`](https://bioconductor.org/packages/release/bioc/html/simplifyEnrichment.html). When I tried installing the package, I realized it was only available for the latest version of R! I couldn't update R on genefish, so instead I updated R on my own computer. After I got the latest R version, I was able to install `simplifyEnrichment` with `BiocManager`.

To make the heatmap, I needed to create a semantic similarity matrix of my enriched GOterms. [Following the instruction manual](https://bioconductor.org/packages/release/bioc/vignettes/simplifyEnrichment/inst/doc/simplifyEnrichment.html), I calculated the semantic similarity matrix using the "Relevance" measure (default) using `GO_similarity`. Then, I used `simplifyGO` to create the plot! I had to dig into the manual to find how to change aesthetics related to the word cloud and text sizing:

```{r}
matBP <- GO_similarity(go_id = sigRes.allBP$GOID, ont = "BP") #Calculate the semantic similarity matrix using the Rel method (default)
```

```{r}
#pdf("figures/simplifyEnrichment-BP.pdf", width = 11, height = 8.5) #Save figure
simplifyGO(matBP,
           column_title = "", col = rev(plotColors), fontsize_range = c(10,40),
           word_cloud_grob_param = list(col = "black", max_width = 25)) #Plot GOterms based on semantic similarity. Do not include a column title. Set colors to be plot colors, and set fontsize to range from 10 to 40. Pass arguments to word_cloud_grob_param to dictate the colors of the words and maximum width
#dev.off()
```

![Screen Shot 2021-07-06 at 4 38 55 PM](https://user-images.githubusercontent.com/22335838/124995501-55d97780-dffc-11eb-9537-67b7d38bc991.png)

<img width="889" alt="Screen Shot 2021-07-08 at 2 54 37 PM" src="https://user-images.githubusercontent.com/22335838/124995589-6d186500-dffc-11eb-8182-4a9667183cb9.png">

**Figures 1-2**. Biological process and molecular function `simplifyEnrichment` figure

I used the biological process figure in the main text, and kept the molecular function one in the supplement.

### Sequencing summary table

The last thing I wanted to do was concatenate all the information related to sequence trimming and alignment. I looked through the [`fastqc`](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/05-bismark-roslin/sequencing-summary-table.csv) and [`bismark` alignment](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/05-bismark-roslin/multiqc_data/multiqc_bismark_alignment.txt), [deduplication](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/05-bismark-roslin/multiqc_data/multiqc_bismark_dedup.txt), and [methylation extraction]() MultiQC reports to create [this monster table for the supplement](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/05-bismark-roslin/multiqc_data/multiqc_bismark_methextract.txt).

### Going forward

1. Report `mc.cores` issue to [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html)
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
