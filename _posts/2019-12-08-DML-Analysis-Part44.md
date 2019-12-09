---
layout: post
comments: true
title: DML Analysis Part 44
tags: virginica MBDSeq GO-MWU
---

## My last (I promise) analyses

I'm almost done revising my paper for another read-through! Before I update my discussion, conclusion, and abstract, I want to try two new analyses based on a conversation I had with Alan on Wednesday.

### GO-MWU for gene body and promoter data

Alan mentioned that he only used CpG data in gene bodies for his GO-MWU analysis and got significantly enriched processes for his methylation data. This, and the paper I reviewed for Evolutionary Applications, got me thinking about my own enrichment methods. In [this R Markdown script](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-Gene-Enrichment-with-GO-MWU.Rmd), I conduct a gene enrichment analysis using CpGs in the DML background that overlap with mRNA. I wondered if my results would be any different if I also included background CpGs that overlapped with promoter regions.

To do this, I characterized CpG overelaps with the DML background and promoter track [in this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb). I then imported the DML background-promoter overlaps into [my R Markdown script](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-Gene-Enrichment-with-GO-MWU.Rmd). In order to format the information for GO-MWU, I needed to match promoters with Genbank IDs, Uniprot accession codes, and GOterms. The final column of the overlap information had Genbank IDs, so I found a way to isolate them.

```{bash}
cut -f13 ../2018-11-01-DML-and-DMR-Analysis/2019-06-20-DMLBackground-Promoters.txt \
| awk  '{gsub(";","\t",$0); print;}' \
| awk  '{gsub(",","\t",$0); print;}' \
| awk  '{gsub(":","\t",$0); print;}' \
> 2019-06-20-DMLBackground-PromoterOverlap-Descriptions.txt #Isolate mRNA description column and convert ";" and "," to "\t". Save the output as a new file
```

```{r}
promoterDescriptions <- read.delim2("2019-06-20-DMLBackground-PromoterOverlap-Descriptions.txt", header = FALSE, col.names = paste("V", 1:80, sep = "")) #Import document with gene descrptions. Name 80 columns so all rows are imported with 80 columns
DMLBackgroundPromoterOverlaps$Genbank <- promoterDescriptions$V6 #Save Genbank ID as a new column
```

Using dataframes I created for my previous GO-MWU analysis, I merged the overlaps with Uniprot and GOterm information, as well as p-values from `methylKit`.

```{r}
DMLBackgroundPromoterOverlapsUniprotGOterms <- unique(merge(x = DMLBackgroundPromoterOverlaps, y = geneBackgroundGOterms, by = "Genbank")) #Merge promoter overlaps by gene background information with Uniprot and GOterm information using Genbank columns. Only retain unique entries
DMLBackgroundPromoterOverlapsUniprotGOterms <- DMLBackgroundPromoterOverlapsUniprotGOterms[,c(1:7, 12:13)] #Keep pertinent columns
colnames(DMLBackgroundPromoterOverlapsUniprotGOterms) <- c("Genbank", "chr", "DMLB.start", "DMLB.end", "promoter.start", "promoter.end", "Uniprot", "Product", "GO") #Rename columns
h"promoter.start", "promoter.end", "Uniprot", "Product", "GO") #Rename columns
head(DMLBackgroundPromoterOverlapsUniprotGOterms) #Confirm merge
```

```{r}
DMLBackgroundPromoterOverlapsUniprotGOterms <- unique(merge(x = DMLBackgroundPromoterOverlapsUniprotGOterms, y = allTested, by = "DMLB.start")) #Merge by DMLB start and end to add p-values to overlap information
DMLBackgroundPromoterOverlapsUniprotGOterms <- DMLBackgroundPromoterOverlapsUniprotGOterms[,c(1:9, 13:15)] #Keep pertinent columns
colnames(DMLBackgroundPromoterOverlapsUniprotGOterms) <- c("DMLB.start", "Genbank", "chr", "DMLB.end", "promoter.start", "promoter.end", "Uniprot", "Product", "GO", "pvalue", "qvalue", "meth.diff") #Rename columns
head(DMLBackgroundPromoterOverlapsUniprotGOterms) #Confirm changes
```

Finally, I formatted my GO-MWU inputs. I isolated the information from `DMLBackgroundPromoterOverlapsUniprotGOterms` I needed. The tab-delimited GO annotation table had two columns: Genbank ID and GOterms. The comma-separated table of significance measures had Genbank IDs with signed p-values. I used `rbind` to merge promoter overlap with gene body overlap information.

When I ran GO-MWU, there were no significant terms. Whoops.

### Biomineralization genes

If methylation changes in adults can impact biomineralization processes in larvae, then biomineralization genes should be differentially methylated in adult gonad tissue. The last thing I wanted to do before jumping back to my discussion was to see how many biomineralization genes (if any) had DML. Looking through the literature, Alan found 82 biomineralization genes. I saved the information from his Google Sheet [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-12-09-Biomineralization-Genes.csv). In [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-Gene-Enrichment-Analysis.Rmd), I characterized overlaps between the biomineralization genes and my DML. All I had to do was merge my DML-gene overlap list with biomineralization genes using gene IDs! I saved the biomineralization gene-DML overlaps [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-12-09-Biomineralization-Genes-with-DML.csv).

There were four unique biomineralization genes with DML out of eight entries total. One biomineralization gene with DML codes for a EF-hand protein with a calcium-binding domain (hypermethylated). Four DML were in a gene coding for calmodulin-regulated spectrin-associated protein. It's interesting that a biomineralization gene had 4 DML (1 hypermethylated, 3 hypomethylated DML). Another biomineralization gene had multiple DML. The gene coding for calmodulin-lysine N-methyltransferaze had 2 DML (both hypermethylated). The last biomineralization gene with DML was for calmodulin-binding transcription activator (hypermethylated).

### Going forward

2. Update methods and results
4. Revise the discussion
7. Revise my abstract
8. Send to collaborators and committee members
8. Address any new edits and clean up the text
9. Update paper repository
10. Submit to the Special Issue
9. Post the paper on bioRXiv

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
