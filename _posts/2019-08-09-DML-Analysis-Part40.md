---
layout: post
comments: true
title: DML Analysis Part 40
tags: virginica MBDSeq GO-MWU
---

## Revised gene description using GO-MWU information

I [previously used](https://yaaminiv.github.io/DML-Analysis-Part39/) GO-MWU GOslim terms to count the number of genes with DML in different GO categories, but looking over the results I wasn't convinced with my methods. I decided to revise what I did and get more accurate counts.

In [this R Markdown script](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-Gene-Enrichment-with-GO-MWU.Rmd), I first imported [annotated DML-gene overlaps](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Gene-Annotation.csv). I subsetted the Genbank IDs, p-values, and methylation differences and merged that information with GO-MWU GOslim terms. Just in case there were any entries without matching GO-MWU information, I decided to keep all rows in the DML-gene annotation subset. I also retained unqiue lines after merging:

```{r}
DMLGenbank <- data.frame("Genbank" = DMLGeneAnnotation$Genbank,
                         "pvalue" = DMLGeneAnnotation$pvalue,
                         "meth.diff" = DMLGeneAnnotation$meth.diff) #Create a new dataframe with Genbank IDs and pvalues for each DML. Include meth.diff as well
head(DMLGenbank) #Confirm dataframe creation
```

```{r}
parentGOBPDML <- unique(merge(x = DMLGenbank, y = parentGOBP, by = "Genbank", all.x = TRUE)) #Merge parent GOslim terms and DML information by Genbank ID. Keep only unique entries. This will allow me to retain multiple DML per gene, but remove any merging artifacts. Keep rows that don't have matching GOterms from GO-MWU just in case.
head(parentGOBPDML) #Confirm merge. There are several entries without any matching information from GO-MWU.
```

I noticed that there were DML-gene overlaps without matching GO-MWU information, so I decided to count how many entries were missing information:

```{r}
sum(is.na(parentGOBPDML$name)) #Count how many genes with DML do not have GO-MWU entries.
```

There were 1,958 DML that did not have biological process parent gene ontology terms and 1,959 DML that did not have molecular function parent gene ontology terms from GO-MWU for corresponding genes! That seems a bit concerning, but I do remember that there were lots of DML in genes with uncharacterized transcripts. I counted the number of genes with DML in BP and MF divisions, and further split up genes with hypermethylated and hypomethylated DML to make some tables. There was an even split between uncharacterized genes with hyper- and hypomethylated DML.

**Table 1**. Biological process functions of genes with DML. Corresponding transcripts were annotated using BLASTx, then paired with gene ontology terms from the Uniprot Swiss-Prot Database. GO-MWU clustered gene ontology by parent terms, presented below. There were 1,958 DML that did not have biological process parent gene ontology terms from GO-MWU for corresponding genes, and no gene ontology categories were significantly enriched (FDR > 10%).

|               **Biological Process**              | **Hypermethylated DML in Annotated Genes** | **Hypermethylated DML in Annotated Genes** |
|:-------------------------------------------------:|:------------------------------------------:|:------------------------------------------:|
|         Anatomical structure morphogenesis        |                      4                     |                      1                     |
|          Cell cycle G2/M phase transition         |                      2                     |                      1                     |
|            Cell cycle phase transition            |                      2                     |                      1                     |
|                 Cell morphogenesis                |                      2                     |                      0                     |
|            Cellular component assembly            |                      1                     |                      1                     |
|           Cellular developmental process          |                      2                     |                      0                     |
|                  Cilium movement                  |                      0                     |                      2                     |
|             Cytoskeleton organization             |                      1                     |                      0                     |
|               DNA metabolic process               |                      1                     |                      3                     |
|                    Localization                   |                      2                     |                      0                     |
|             Microtubule-based process             |                      1                     |                      2                     |
|             Mitotic cell cycle process            |                      2                     |                      1                     |
|           Morphogenesis of an epithelium          |                      1                     |                      1                     |
|     Movement of cell or sub cellular component    |                      0                     |                      1                     |
|               mRNA metabolic process              |                      1                     |                      0                     |
|          Multicellular organismal process         |                      2                     |                      0                     |
| Negative regulation of cellular metabolic process |                      4                     |                      1                     |
|               Organelle organization              |                      2                     |                      0                     |
|          Regulation of biological quality         |                      1                     |                      0                     |
|  Regulation of cellular protein metabolic process |                      1                     |                      0                     |
|                Reproductive process               |                      1                     |                      0                     |
|                Response to stimulus               |                      1                     |                      1                     |
|         Ribonucleoprotein complex assembly        |                      0                     |                      1                     |
|               RNA metabolic process               |                      1                     |                      0                     |
|                   RNA processing                  |                      1                     |                      0                     |
|        RNA splicing via transesterification       |                      1                     |                      0                     |
|                  Uncharacterized                  |                     979                    |                     979                    |

**Table 2**. Molecular functions of genes with DML. Corresponding transcripts were annotated using BLASTx, then paired with gene ontology terms from the Uniprot Swiss-Prot Database. GO-MWU clustered gene ontology by parent terms, presented below. There were 1,959 DML that did not have molecular function parent gene ontology terms from GO-MWU for corresponding genes, and no gene ontology categories were significantly enriched (FDR > 10%).

|           **Molecular function**           | **Hypermethylated DML in Annotated Genes** | **Hypomethylated DML in Annotated Genes** |
|:------------------------------------------:|:------------------------------------------:|:-----------------------------------------:|
|  Anion transmembrane transporter activity  |                      1                     |                     0                     |
|             Calcium ion binding            |                      0                     |                     1                     |
|               Enzyme binding               |                      1                     |                     6                     |
|      Guanyl-nucleotide exchange factor     |                      1                     |                     0                     |
|                Lipid binding               |                      1                     |                     0                     |
|            Magnesium ion binding           |                      1                     |                     0                     |
|              Metal ion binding             |                      1                     |                     0                     |
|        Molecular function regulator        |                      1                     |                     0                     |
|                mRNA binding                |                      1                     |                     1                     |
|           Oxidoreductase activity          |                      2                     |                     4                     |
|             Phospolipid binding            |                      1                     |                     0                     |
|     Protein-containing complex binding     |                      1                     |                     0                     |
| Purine ribonucleotide triphosphate binding |                      1                     |                     1                     |
|                 RNA binding                |                      1                     |                     1                     |
|           Signal receptor binding          |                      1                     |                     0                     |
|           Small molecule binding           |                      1                     |                     1                     |
|     Structural constituent of ribosome     |                      1                     |                     0                     |
|        Structural molecule activity        |                      1                     |                     0                     |
|     Transcription coactivator activity     |                      0                     |                     2                     |
|     Transcription coregulator activity     |                      0                     |                     3                     |
|      Transcription regulator activity      |                      0                     |                     3                     |
|            Transferase activity            |                      0                     |                     3                     |
|            Transporter activity            |                      1                     |                     0                     |
|               Uncharacterized              |                     986                    |                    973                    |

Updated output files can be found at these links:

**Biological processes**:

- [CondensedDML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDML-Parent-GOSlim-Frequency-BP.csv)
- [Hypermethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHyper-Parent-GOSlim-Frequency-BP.csv)
- [Hypomethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHypo-Parent-GOSlim-Frequency-BP.csv)

**Cellular components**:

- [CondensedDML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDML-Parent-GOSlim-Frequency-CC.csv)
- [Hypermethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHyper-Parent-GOSlim-Frequency-CC.csv)
- [Hypomethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHypo-Parent-GOSlim-Frequency-CC.csv)

**Molecular function**:

- [CondensedDML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDML-Parent-GOSlim-Frequency-MF.csv)
- [Hypermethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHyper-Parent-GOSlim-Frequency-MF.csv)
- [Hypomethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHypo-Parent-GOSlim-Frequency-MF.csv)

Alright, NOW I think I'm done looking at DML.

### Going forward

1. Work through gene-level analysis
2. Update methods and results
3. Update paper repository
4. Outline the discussion
5. Write the discussion
6. Write the introduction
7. Revise my abstract
8. Share the draft with collaborators and get feedback
9. Post the paper on bioRXiv
10. Prepare the manuscript for publication

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
