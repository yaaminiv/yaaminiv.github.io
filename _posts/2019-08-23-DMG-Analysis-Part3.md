---
layout: post
comments: true
title: DMG Analysis Part 3
tags: virginica MBDSeq DMG GO-MWU
---

## Enrichment and description of differentially methylated genes

I talked to Shelly about [my non-significant results](https://yaaminiv.github.io/DMG-Analysis-Part2/). She said I could use uncorrected p-values in my results and argue that this is an exploratory study, just like she did with her crab metabolomics paper. I returned to [my R Markdown script](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2019-08-14-Differentially-Methylated-Genes) to do just that.

### Methylation differences and annotation

I wanted to [use GO-MWU again](https://yaaminiv.github.io/DML-Analysis-Part39/). I knew I needed methylation difference information between treatments to correct p-values if I wanted to use signed log p-values. To do this, I subsetted methylation information for ambient and treatment samples separately, then calculated median percent methylation by gene:

```{r}
ambientSamples <- subset(fullPercentMethExpanded, subset = fullPercentMethExpanded$treatment == "Ambient") #Subset ambient samples
ambientSamplesPercentMeth <- aggregate(percentMeth ~ geneID, data = ambientSamples, FUN = median) #Calculate median methylation by gene for ambient samples
```

```{r}
treatmentSamples <- subset(fullPercentMethExpanded, subset = fullPercentMethExpanded$treatment == "Treatment") #Subset treatment samples
treatmentSamplesPercentMeth <- aggregate(percentMeth ~ geneID, data = treatmentSamples, FUN = median) #Calculate median methylation by gene for treatment samples
```

Returning to the Kruskal-Wallis statistical output, I subsetted genes with uncorrected significant p-values — my DMG.  I then added annotation information to the DMG, including Uniprot Accession codes, GO terms, and methylation differences. I repeated this process with DMG with DML. There are 223 DMG and 6 DMG with DML! I saved the annotated DMG [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-DMG-KW-Test-Results-Uncorrected-Sig.csv) and annotated DMG with DML [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-DMG-KW-Test-Results-DMLOnly-Uncorrected-Sig.csv).

### Gene enrichment with GO-MWU

Gene enrichment time! I generated a [GO annotation table](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-Genes-GO-Annotations-Table.tab) and [table of significance measures](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-Genes-Table-of-Significance-Measures.csv) using signed log p-values from Kruskal-Wallis tests. All genes were used; not just those that are DMG. I noticed that half of the genes in the GO annotation table did not have associated GOterms. Unsurprisingly, there were no significantly enriched parent GOslim terms for biological processes, cellular components, and molecular function categories. I counted the frequency of parental GOslim terms for all genes tested and for DMG:

**Biological processes**:
- [All genes](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-Genes-Parent-GOSlim-Frequency-BP.csv)
- [DMG](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-DMG-Parent-GOSlim-Frequency-BP.csv)

**Cellular components**:
- [All genes](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-Genes-Parent-GOSlim-Frequency-CC.csv)
- [DMG](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-DMG-Parent-GOSlim-Frequency-CC.csv)

**Molecular function**:
- [All genes](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-Genes-Parent-GOSlim-Frequency-MF.csv)
- [DMG](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-DMG-Parent-GOSlim-Frequency-MF.csv)

Alright, now it's time to write (and maybe make some new figures who knows).

### Going forward

2. Update methods and results
3. Finalize figures
2. Update paper repository
3. Outline the discussion
4. Write the discussion
5. Write the introduction
6. Revise my abstract
7. Share the draft with collaborators and get feedback
8. Post the paper on bioRXiv
9. Prepare the manuscript for publication

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
