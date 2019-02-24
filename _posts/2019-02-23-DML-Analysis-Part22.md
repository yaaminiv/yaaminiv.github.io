---
layout: post
comments: true
title: DML Analysis Part 22
tags: virginica MBDSeq gene-enrichment
---

## Gene enrichment for mRNA overlaps

Back in analysis mode! I decided to tackle a gene enrichment for the any feature files that overlaped with mRNA coding regions. The reasons why I only chose files with mRNA coding region overlaps are because learning which coding regions are enriched is most interesting to me, and because I needed Genbank IDs to match overlap results with my [`blastx` output](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2018-09-11-Transcript-Uniprot-blastx-codeIsolated.txt). [I did a gene enrichment before](https://yaaminiv.github.io/DML-Analysis-Part7/), but this was using the wrong background. For this analysis, I used the [a previously generated `blastx` output](https://yaaminiv.github.io/DML-Analysis-Part6/) and [the gene background from `methylKit`](https://yaaminiv.github.io/DML-Analysis-Part19/). I merged documents and isolated Uniprot codes in [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-Gene-Enrichment-Analysis.Rmd).

I followed the instructions from my previous gene enrichment using [DAVID](https://david.ncifcrf.gov/summary.jsp). I downloaded the functional annotation table, functional annotation clustering, and GOterm information (biological processes, cellular components, and molecular functions) from DAVID for each analysis. The output from all my analyses can be found in [this master folder](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-DAVID-Output). I've gone into detail about some of the GOterm results below, but there were barely any significantly enriched terms after correcting for multiple comparisons. There are a handful of enriched GOterms that are significant without correction that could be interesting to describe.

### Overlaps

I performed a gene enrichment from the [DMR-mRNA](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-07-DMR-mRNA.txt) and [DML-mRNA](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-07-DML-mRNA-Unfolded.txt) overlaps to see which genes were overrepresented in differentially methylated loci and regions between ambient and treatment samples.

#### DMR

Specific results can be found [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-DAVID-Output/2019-02-22-DMR-mRNA-Overlaps).

Based on corrected p-values, there was only one significantly enriched GOterm for this analysis: [cilium morphogenesis](http://www.informatics.jax.org/vocab/gene_ontology/GO:0060271)! That could be interesting seeing how impacts on cilia could affect cellular structure. The only other GOterm with less than a 10% FDR was [cellular projection organization](http://www.informatics.jax.org/vocab/gene_ontology/GO:0030030), which may also be involved with cilia, flagella, and sperm motility.

#### DML

Specific results can be found [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-DAVID-Output/2019-02-22-DML-mRNA-Overlaps).

There were no significantly enriched GOterms when I looked at DMLs instead of DMRs. For cellular components, cytoplasm had a FDR less than 10%. The molecular function ubiquitin-protein transferase activity also had a FDR less than 10%.

### Upstream Flanks

I previously conducted a [flanking analysis](https://yaaminiv.github.io/DML-Analysis-Part18/) to identify 100 bp flanks upstream and downstream of mRNA coding regions. I then intersected these flanks with DMR and DML. Understanding what processes are enriched in the flanking regions can provide insight into regulatory mechanisms.

For the upstream flanks, I used [this DMR overlap file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-14-Flanking-Analysis/2018-11-15-mRNA-100bp-UpstreamFlanks-DMR.txt) and [this DML overlap file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-14-Flanking-Analysis/2018-11-15-mRNA-100bp-UpstreamFlanks-DML.txt).

There were no significantly enriched GOterms for the intersection of upstream flanks with DMRs or DMLs after correcting for multiple comparisons.

### Downstream Flanks

For the downstream flanks, I used [this DMR overlap file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-14-Flanking-Analysis/2018-11-15-mRNA-100bp-DownstreamFlanks-DMR.txt) and [this DML overlap file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-14-Flanking-Analysis/2018-11-15-mRNA-100bp-DownstreamFlanks-DML.txt).

There were no significantly enriched GOterms for the intersection of downstreams flanks with DMRs or DMLs after correcting for multiple comparisons.

### Closest Non-Overlapping

When I conduced my flanking analysis, I also identified the closest non-overlapping DMR and DML to each mRNA coding region. Again, understanding what processes are enriched in these non-overlapping elements may provide information about regulatory mechanisms or related gene functions. I used [this file for DMRs](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-14-Flanking-Analysis/2018-11-14-mRNA-Closest-NoOverlap-DMRs.txt) and [this file for DMLs](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-14-Flanking-Analysis/2018-11-14-mRNA-Closest-NoOverlap-DMLs.txt).

There were also no significantly enriched GOterms for the closest non-overlapping DMRs or DMLs.

### Going forward

1. Determine if this is the best gene enrichment approach
2. Find a way to do a gene enrichment with exon, intron, and transposable element overlaps
3. Describe functions of most interesting genes with DML and DMR

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
