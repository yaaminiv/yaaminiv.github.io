---
layout: post
comments: true
title: CEABiGR Part 11
tags: virginica CEABiGR gene-expression alt-splicing
---

## Continuing gene activity analyses

We made Github issues with all of the analytical things and [figures](https://github.com/sr320/ceabigr/issues/89) that we need to complete before the paper's done, so I guess no better time to do them than Leap Day (Nothing is impossible on Leap Day! Real life is for March! 30 Rock fans know what I'm talking about).

### Summarize gene activity information

The first thing I did was create a table in the Google Doc to summarize the output from maximum transcript and predominant isoform shift analyses.

**Table 1**. Gene and transcript expression patterns attributable to low pH. Changes in maximum number of transcripts are described with respect to OA exposure, with increases in maximum number of transcripts representing more transcripts expressed in OA-exposed samples, and decreases representing more transcripts expressed in control samples. Genes with multiple (≥ 2 transcripts) were considered for predominant isoform analyses, with a varied number of genes having shifts in the predominant transcript due to OA exposure.

|   Sex  | Total Genes Expressed | Increases in Maximum Number of Transcripts | Decreases in Maximum Number of Transcripts | Genes with ≥ 2 Transcripts | Shift in Predominant Isoform |
|:------:|:---------------------:|:------------------------------------------:|:------------------------------------------:|:--------------------------:|:----------------------------:|
| Female |         39,047        |                    1,529                   |                    2,437                   |           38,575           |             2,252            |
|  Male  |         39,299        |                    3,123                   |                    1,204                   |           10,559           |             1,768            |

I opted for the table instead of a plot, since the main information that needed to be communicated was how many genes had changes in the maximum number of transcripts or shifts in the predominant isoform, and there was no point in having a bar chart just for a few numbers.

### Visualizing the predominant isoform analysis

I hopped back into [this R Markdown script](https://github.com/sr320/ceabigr/blob/main/code/42-predominant-isoform.Rmd) to make some plots looking at the relationship between methylation and whether or not the predominant isoform shifted under low pH conditions. My first instinct was to make XY plots looking at methylation vs. gene expression, with facets for if there was a shift in the predominant isoform:

```
femPredIsoXY <- femMethExp %>%
  ggplot(aes(x = avg_meth, y = log(avg_exp), color = as.factor(change))) +
  geom_point() +    
  geom_smooth(aes(x = avg_meth, y = log(avg_exp)), method = lm, colour = "grey20", linewidth = 2, na.rm = TRUE) +
  stat_regline_equation(aes(x = avg_meth, y = log(avg_exp), label = paste(..eq.label..)), formula = y ~ x, size = 5, inherit.aes = FALSE) +
  facet_wrap(~change, labeller = labeller(change = facet_labels)) +
  labs(x = "", y = "log FPKM", title = "A. Females") +
  scale_color_manual(values = c(plotColors[5], plotColors[8])) +
  theme_classic(base_size = 15) + theme(legend.position = "none")

malePredIsoXY <- maleMethExp %>%
  ggplot(aes(x = avg_meth, y = log(avg_exp), color = as.factor(change))) +
  geom_point() +    
  geom_smooth(aes(x = avg_meth, y = log(avg_exp)), method = lm, colour = "grey20", linewidth = 2, na.rm = TRUE) +
  stat_regline_equation(aes(x = avg_meth, y = log(avg_exp), label = paste(..eq.label..)), formula = y ~ x, size = 5, inherit.aes = FALSE) +
  facet_wrap(~change, labeller = labeller(change = facet_labels)) +
  labs(x = "Methylation (%)", y = "log FPKM", title = "B. Males") +
  scale_color_manual(values = c(plotColors[5], plotColors[8])) +
  theme_classic(base_size = 15) + theme(legend.position = "none")

femPredIsoXY / malePredIsoXY #Create patchwork plot
ggsave("pred-isoform-meth-exp-xy.pdf", height = 8.5, width = 11)
```

![Screenshot 2024-02-29 at 5 36 48 PM](https://github.com/sr320/ceabigr/assets/22335838/2ccdce51-b9dc-4843-bc6e-fd21cf0321e1)

**Figure 1**. Gene methylation vs. gene expression for genes with and without a shift in the predominant isoform

However, [my model](https://yaaminiv.github.io/CEABiGR-Part11/) looked at change in methylation, not just average gene methylation. So I made plots to understand the distribution for changes in methylation for genes in each category:

```
femPredIsoMethChange <- predIsoFemWide %>%
  ggplot(aes(x = change_meth, fill = as.factor(change))) +
  geom_histogram() +    
  labs(x = "", y = "Number of Genes", title = "A. Females") +
  scale_x_continuous(limits = c(-30,30), breaks = seq(-30,30,10)) +
  scale_fill_manual(values = c(plotColors[5], plotColors[8])) +
  theme_classic(base_size = 15) + theme(legend.position = "none")

malePredIsoMethChange <- predIsoMaleWide %>%
  ggplot(aes(x = change_meth, fill = as.factor(change))) +
  geom_histogram() +    
  labs(x = "Change in Methylation (%)", y = "Number of Genes", title = "B. Males") +
  scale_x_continuous(limits = c(-30,30), breaks = seq(-30,30,10)) +
  scale_fill_manual(values = c(plotColors[5], plotColors[8]),
                    labels = c("No Shift", "Shift"),
                    name = "") +
  theme_classic(base_size = 15) + theme(legend.position = "bottom")

femPredIsoMethChange / malePredIsoMethChange
ggsave("pred-isoform-change-meth-distribution.pdf", height = 8.5, width = 11)
```

![Screenshot 2024-02-29 at 4 20 36 PM](https://github.com/sr320/ceabigr/assets/22335838/a3075edd-3bfc-4b2d-8127-05dd94714309)

**Figure 2**. Change in average gene methylation (%) for genes in A) female and B) male oysters with (dark green) or without (light green) a shift in the predominant transcript due to OA exposure. Change in methylation was a significant predictor of whether or not there was a predominant transcript shift in males, not females.

### Biological process enrichment in genes with a predominant isoform shift

The next step was doing a gene enrichment analysis to understand the influence of OA on changes to predominant isoforms, and then understand how methylation fits in. I thought this was a task assigned to me, but it seems like [Steven took care of it](https://github.com/sr320/ceabigr/issues/94). I can't find his code, so I'm going to do a more statistically sound enrichment.

To understand if gene function influenced which genes had a shift in predominant isoform, I decided to use `topGO`. I'd conduct the enrichment by using all genes with data for multiple isoforms as the background, and then the list of genes with a shift in the predominat isoform as the gene set of interest. To prepare, I reviewed [this old lab notebook post about `topGO`](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part21/).

I started with the female dataset for the enrichment. I used the set of genes with > 1 transcript as the background:

```
predIsoFemFiltAnnot %>%
  dplyr::select(., gene_name, GO) %>%
  unique(.) %>%
  group_by(., gene_name) %>%
  summarise(GO = paste(GO, collapse = ",")) %>%
  write.table(., "geneid2go-fem_predIso.tab", quote = FALSE, sep = "\t", col.names = FALSE, row.names = FALSE)
  #Take list of genes and GOterms for female predominant isoform data, and remove any duplicate entries (precaution). Using genes since that was the "input" for the predominant isoform analsyis. Group by transcript ID, then summarise using paste to get all GOterms for each transcript into the same row separated by ",". Save as a tab delimited file
```

I found `summarise` with `paste` and `collapse` to be EXTREMELY useful to get GOterms in separate rows to be in one column separated by a comma delimiter. Once I had the file written out, I read it in with `readMappings` to get it into the `topGO` format and extracted the gene names.

```
geneID2GO <- readMappings(file = "geneid2go-fem_predIso.tab") #Loading the GO annotations and GeneIDs. Each line has one transcript ID and all associated GOterms
length(geneID2GO) #31,276 genes with annotations out of 38,575 genes with data for ≥ 2 transcripts
```

```
geneNames <- names(geneID2GO) #Extract names to use as a gene universe
head(geneNames)
```

There were 31,276 genes with annotations out of the 38,575 genes with data for > 1 transcript. For the genes of interest, I filtered out genes where there was a shift in the predominant isoform. I initially saved this as a dataframe, but got an error about the input not having the correct number of factors. Looking at discrepancies between this code and my [old code](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/09-Functional-Enrichment.Rmd), I tried saving the genes of interest as a vector instead. This ended up solving the issue!

```
predIsoFemFiltChange <- predIsoFemFilt %>%
  filter(., change == 1) %>%
  dplyr::select(., gene_name) #Take data and retain genes where the predominant isoform shifted (change == 1). Select the gene ID column and convert to a dataframe. Save as a new object
predIsoFemFiltChange <- predIsoFemFiltChange$gene_name #Save column as a vector (will not work otherwise!)
```

```
predIsoFemFiltGeneList <- factor(as.integer(geneNames %in% predIsoFemFiltChange)) #Create a factor vector to indicate genes that shifted as significant (1) and didn't shift as not significant (0)
names(predIsoFemFiltGeneList) <- geneNames
```

Once I had my inputs ready, I ran the gene enrichment analysis just for biological processes. I thought adding molecular functions would be too confusing and not add as much information.

```
predIsoFemdataBP <- new("topGOdata", ontology = "BP", allGenes = predIsoFemFiltGeneList,
                      annot = annFUN.gene2GO, gene2GO = geneID2GO) #Create biological process topGO object

test.stat <- new("classicCount", testStatistic = GOFisherTest, name = "Fisher test") #Set test statistic
resultFisher.predIsoFemdataBP <- getSigGroups(predIsoFemdataBP, test.stat) #Get enrichment results

pvalFis.predIsoFemdataBP <- score(resultFisher.predIsoFemdataBP) #Extract p-values


allRes.predIsoFemdataBP <- GenTable(predIsoFemdataBP, classic = resultFisher.predIsoFemdataBP, ranksOf = "classic", orderBy = "classic", topNodes = length(pvalFis.predIsoFemdataBP)) #Create a statistical results table with statistical test results. Order by p-value (classic), and include all results (topNodes)
```

The output file can be found [here](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/fem-predIso-BP-FisherTestResults.csv). For females, I ended up with 226 significantly enriched GOterms. I then added this information back to the list of genes with a shift in the predominant isoform. The resultant list can be found [here](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/fem-predIso-BP-EnrichedGO-geneAnnot.csv).

```
sigRes.predIsoFemdataBP <- allRes.predIsoFemdataBP[1:226,c(1, 6)] #Filter significantly enriched GOterms, only keep GOID and p-value
colnames(sigRes.predIsoFemdataBP) <- c("GO", "p.value") #Change column names
head(sigRes.predIsoFemdataBP)

sigRes.predIsoFemdataBP %>%
  left_join(., y = (predIsoFemFiltAnnot %>% filter(., change == 1)), by = "GO") %>%
  dplyr::select(., gene_name, change, t_name, Predom_exp, GO, p.value, product) %>%
  separate(., col = product, into = c("product", "transcriptVar"), sep = "%2C") %>%
  dplyr::select(., -transcriptVar) %>%
  unique(.) %>%
  write.csv("fem-predIso-BP-EnrichedGO-geneAnnot.csv", quote = FALSE, row.names = FALSE) #Take significant BPGO data and join with annotation information. Select columns of interest. Separate product column by "%2C" and remove transcriptVar column. Select unique rows. This removes all "transcript variant" information so each gene is listed once per significant GOterm. Save as a csv file.
```

I did the same thing for males and produced a [list of significant BP GOterms](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/male-predIso-BP-FisherTestResults.csv) and [matched them to gene annotations](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/male-predIso-BP-EnrichedGO-geneAnnot.csv). There were 33 significantly enriched BP GOterms in genes with a shift in the predominat isoform for male oysters.

### Adding methylation to the mix

While I was going through the gene enrichment process I had an idea. We really want to understand how methylation may influence the shift in the predominant isoform. [Our models](https://yaaminiv.github.io/CEABiGR-Part11/) show that change in gene methylation does not influence the predominant isoform shift in females, but it does in males. But perhaps we can learn a bit more by looking at the methylation of the genes where there was a shift in the predominant isoform AND we have enriched GOterms. Is this a fishing expedition? You bet.

The fishing expedition didn't produce anything except supplemental figures that can be found in [this folder](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform). HOWEVER it made me realize I was missing the obvious: how many DML were in genes where the predominant isoform shifted?! Were these DML concentrated in genes with enriched BPGO?! I created [this issue](https://github.com/sr320/ceabigr/issues/99) to keep track of this revelation.

### Going forward

1. Finish maximum transcript analysis
2. Perform formal DML enrichment analysis
3. Quantify DML in genes with a predominant isoform shift
4. Quantify DML in genes with a change in maximum transcript
2. Update methods and results
2. Compare with Aranda and Liew models in discussion section
4. Read recent papers
3. Write discussion
4. Write abstract
5. Outline introduction
6. Write introduction
7. Send to co-authors for approval
8. Submit to journal special issue

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
