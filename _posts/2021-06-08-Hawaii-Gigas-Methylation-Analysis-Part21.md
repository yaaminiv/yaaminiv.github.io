---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 21
tags: hawaii gigas-ploidy topGO
---

## Trying `topGO` for GOterm enrichment

I decided to use `topGO` because it was easier to figure out than `goseq`, and two different methylation analysis papers have used `topGO`: [Li et al. (2018)](https://advances.sciencemag.org/content/4/8/eaat2142) and [Chandra Rajan et al. (2021)](https://onlinelibrary.wiley.com/doi/full/10.1111/gcb.15675#support-information-section). Chandra Rajan et al. (2021) also published their script, found [here](https://onlinelibrary-wiley-com.offcampus.lib.washington.edu/action/downloadSupplement?doi=10.1111%2Fgcb.15675&file=gcb15675-sup-0002-FileS2.txt), which I modeled my workflow off of.

To prepare for the enrichment analysis, I made the input files in [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/08-GOterm-Annotation.ipynb). The first thing I needed to create was a list of gene IDs and all associated GOterms. I extracted transcript IDs from the annotated union 5x BEDgraph. Then, I used the annotated `blastx` information before I unfolded the GOterms onto separate lines:

```
#Extract columns 2 and 3 to create list needed for topGO gene enrichment
#Convert ; to , for topGO formatting
!cut -f2,3 _blast-annot.tab \
| tr ";" "," \
> geneid2go.tab
!head geneid2go.tab

#Filter the gene ID-GO file based on transcript IDs in 1x union data
!awk 'BEGIN { while(getline <"union_1x.transcriptIDs.unique") id[$0]=1; } id[$1] ' geneid2go.tab \
> geneid2go-union1x.tab
```

In addition to making all of the input files, I also annotated the union 1x BEDfile (CpG background) and DML lists with Uniprot Accession codes, GOterms, and GOSlim information.

I then ran the `topGO` analysis in [this R Markdown file](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/09-Functional-Enrichment.Rmd). First, I imported the gene ID-to-GO term database, and extracted the transcript IDs (gene names) to use as the gene universe:

```{r}
geneID2GO <- readMappings(file = "../Haws_08-GOterm-annotation/geneid2go-union1x.tab") #Loading the GO annotations and GeneIDs. Each line has one transcript ID and all associated GOterms
str(head(geneID2GO)) #Confirm file structure
```

Then, I imported my DML lists. I extracted the transcript IDs, then created a separate factor vector that indicated genes with DML in the gene name list. This factor would list genes containing DML as significant (1), and genes without DML would be not-significant (0):

```{r}
ploidyGenes <- ploidyDMLGOterms$transcript #Extract transcript ID
ploidyGeneList <- factor(as.integer(geneNames %in% ploidyGenes))
names(ploidyGeneList) <- ploidyGenes
```

I ran the enrichment for biological processes, cellular components, and molecular functions. For each category, I created a `topGO` object that specified the ontology I was interested in, my gene list, annotation, and gene2GO database. Then, I ran a Fisher's exact test to see if any processes were overrepresented:

```{r}
ploidyGOdataBP <- new("topGOdata", ontology = "BP", allGenes = ploidyGeneList,
                      annot = annFUN.gene2GO, gene2GO = geneID2GO) #Create biological process topGO object
ploidyGOdataBP #Get summary of object
```

Available genes have annotations, but feasible ones are linked to the GO hierarchy that topGO uses (I think...see [this link](https://avrilomics.blogspot.com/2015/07/using-topgo-to-test-for-go-term.html)).

```{r}
test.stat <- new("classicCount", testStatistic = GOFisherTest, name = "Fisher test")
resultFisher.ploidyBP <- getSigGroups(ploidyGOdataBP, test.stat)
resultFisher.ploidyBP
```

I found enriched GOterms related to biological processes for ploidy-DML, and biological processes and molecular functions for pH-DML! I didn't find any enriched GOterms for the interaction-DML, which makes sense considering how there was a smaller interaction effect to begin with.

**Table 1**. Enriched biological process and molecular function GOterms

<img width="889" alt="Screen Shot 2021-06-10 at 2 56 25 AM" src="https://user-images.githubusercontent.com/22335838/125146047-da012d00-e0d8-11eb-9db8-f9ea7e8188d1.png">

### Going forward

1. Update methods
7. Update results
5. Outline discussion
6. Write discussion
7. Write introduction
2. Conduct randomization test with `DSS`
2. Try [EpiDiverse/snp](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)


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
