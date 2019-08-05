---
layout: post
comments: true
title: DML Analysis Part 38
tags: virginica MBDSeq DAVID gene-enrichment
---

## Gene enrichment and description

Now that I have [annotated DML tables](https://yaaminiv.github.io/DML-Analysis-Part37/), I want to try a gene enrichment and find a good way to describe the functions of the genes in my annotations. Ideally, I can use the Uniprot Accession codes to get GOterms to do gene enrichment and to describe functions.

### Obtaining GOterms

Turns out [I had this exact issue last year](https://github.com/RobertsLab/resources/issues/292) (have I really not progressed…? *shudders*). After some back-and-forth with Sam and Shelly, I realized I could download the Uniprot-SwissProt databse with additional Gene Ontology columns. I went to [this website](https://www.uniprot.org/uniprot/?query=reviewed:yes) and added columns of interest. I then downloaded the database as a tab-delimited file. I initially tried downloading it as a FASTA, but Sam pointed out that I needed to download it as a .txt file if I wanted to maintain the additional columns. My file had the following columns:

- Entry (Uniprot-Accession)
- Entry Name (Uniprot-ID)
- Status (reviewed)
- Protein names
- Gene names
- Organism
- Length
- Gene ontology IDs
- Gene ontology (GO)
- Gene ontology (biological processes)
- Gene ontology (cellular component)
- Gene ontology (molecular function)

I then imported the file in [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-Gene-Enrichment-Analysis.Rmd). I skimmed some of the columns off, so my final annotation tables (found in [this folder](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-12-02-Gene-Enrichment-Analysis)) now include GO-ID, GO-BP, GO-CC, and GO-MF.

### Functional description

Now that I had GOTerms assigned to genes, I could try grouping GOterms together to describe genes. For each Uniprot Accession code, I have three different GOterm categories: biological processes, cellular component, and molecular function. For my [DML-exon](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Exon-Annotation.csv) and [DML-intron](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Intron-Annotation.csv) annotations, I isolated the first three GO-BP codes for each Uniprot accession code with an e-value no larger than 10<sup>-10</sup>. I used `count` in the `dplyr` package to create summary tables, found here:

- [DML-exons: 746 total GOterms, 261 categories](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Exon-Annotation-GO-BP-Counts.csv)
- [DML-exons (hypermethylated): 172; 151](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Exon-Annotation-Hypermethylated-GO-BP-Counts.csv)
- [DML-exons (hypomethylated): 574; 139](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Exon-Annotation-Hypomethylated-GO-BP-Counts.csv)
- [DML-introns: 86; 154](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Intron-Annotation-GO-BP-Counts.csv)
- [DML-introns (hypermethylated): 48; 78](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Intron-Annotation-Hypermethylated-GO-BP-Counts.csv)
- [DML-introns (hypomethylated): 38; 80](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Intron-Annotation-Hypomethylated-GO-BP-Counts.csv)

It’s really good information, but I’m not sure how to include such long tables in a paper. I think I’ll need to map the GOterms to parent (or grandparent) GOterms similar to what Shelly did. I’ll review her lab notebook to see how she did that.

### Gene enrichment with DAVID

I know one method of obtaining GOterms from Uniprot Accession codes it to use [DAVID](https://david.ncifcrf.gov/summary.jsp). Before I could do this, I needed to match Uniprot Accession codes to my gene background file. I returned to [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb) to use `intersectBed` and characterize DML background and mRNA overlaps. In my [R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-Gene-Enrichment-Analysis.Rmd), I matched Uniprot Accession codes to [DMLBackground-mRNA overlaps](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2019-06-20-DMLBackground-mRNA.txt).

I then performed a gene enrichment with DAVID and put the output [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DAVID-Output). No surprise: nothing was significantly enriched.

### A new approach

During our NSF E20 meeting, Steven suggested I focus on enrichment instead of description. Katie pointed out a different gene enrichment tool, [GO-MWU](https://github.com/z0on/GO_MWU). I'll tackle this next.

### Going forward

1. Perform gene enrichment with GO-MWU
2. Work through gene-level analysis
3. Update methods and results
4. Update paper repository
5. Outline the discussion
6. Write the discussion
7. Write the introduction
8. Revise my abstract
9. Share the draft with collaborators and get feedback
10. Post the paper on bioRXiv
11. Prepare the manuscript for publication

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
