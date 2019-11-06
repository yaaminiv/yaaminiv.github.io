---
layout: post
comments: true
title: DML Analysis Part 42
tags: virginica MBDSeq
---

## Addressing collaborator feedback

I got some edits from Katie and Alan, so I figured I'd start to address that feedback. I'm also at the point where I don't think I'll make that Oct. 1 deadline, so I can take my time to make my paper as good as it can be.

### CpG location figures

Now seems like a good time to tackle making better figures to describe DML or CpG locations in the *C. virginica* genome. The first thing I did was update overlap counts [in this spreadsheet](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Overlap-Proportions.csv). In [this R Markdown script](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Proportion-Test.Rmd), I followed Katie and Steven's suggestions to make stacked barplots. I started by making figures to compare all CpG categories (found [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-12-02-Gene-Enrichment-Analysis)).

![Screen Shot 2019-09-30 at 2 32 40 PM](https://user-images.githubusercontent.com/22335838/68252391-42316e80-ffda-11e9-833f-687f71e53876.png)

**Figure 2**. Stacked barplot depicting location of various CpG categories (all CpGs in *C. virginica* genome, MBD-Enriched, methylated, sparsely methylated, and unmethylated) in specific genomic features (exons, introns, transposable elements, putative promoters, other).

I got feedback that this was too much information for a figure. I condensed the barplots to All CpGs and methylated CpGs. The rest of the information in the original figure was still in-text.

![Screen Shot 2019-09-30 at 3 16 27 PM](https://user-images.githubusercontent.com/22335838/68252572-bd932000-ffda-11e9-9037-e137e6365c50.png)

**Figure 3**. Stacked barplot depicting location of all CpGs and methylated CpGs in specific genomic features (exons, introns, transposable elements, putative promoters, other).

I also made a similar figure to compare MBD-enriched CpGs and DML.

![Screen Shot 2019-09-30 at 5 06 34 PM](https://user-images.githubusercontent.com/22335838/68252733-1e225d00-ffdb-11e9-870c-14f824dbd548.png)

**Figure 4**. Stacked barplot depicting location of CpG loci with 5x coverage and DML in specific genomic features (exons, introns, transposable elements, putative promoters, other).

### Genes with multiple DML

Based on Katie's feedback, I decided to look at trends in genes with multiple DML. I mentioned that there were some genes with multiple DML in the text, but I didn't quantify how many DML genes had, or whether they were hyper- or hypomethylated. I examined the trends [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-Gene-Enrichment-Analysis.Rmd).

Before looking into gene patterns, I thought I should go one step back and look at the distribution of DML in chromosomes. I created a barplot showing how many DML were found in each chromosome. I wanted to see if this tracked the number of genes in each chromosome, so I spent some (re: maybe too much) time learning how to add a second y-axis to a plot. I saved some useful code [here](https://genefish.wordpress.com/2019/10/01/adding-lines-to-barplots-2-y-axes/). Essentially, I needed to save my original barplot as a new object, then add the second plot on top.

<img width="686" alt="Screen Shot 2019-09-30 at 11 05 47 PM" src="https://user-images.githubusercontent.com/22335838/68336822-0eb31a80-0094-11ea-9542-d84aae148487.png">

**Figure 5**. Barplot with number of DML and number of genes for each chromosome.

The interesting thing about this plot is that the number of DML track the number of genes, which lends evidence to our idea that DML influence gene activity! While it isn't in this plot, the chromosome with the most genes is also not the largest chromosome, so it's not just a chromosome size thing.

Then I dug into the number of DML in genes. The first thing I did was  calculate the mean, median, and maximum number of DML in a gene. Most genes had only 1 DML, but there was a gene with 5 DML. For each gene with more than 1 DML, I wanted to see if the DML were all in the same direction (i.e. all hypermethylated vs. all hypomethylated). I found that not all DML went the same direction in the same gene! I created tables with the gene ID, number of DML, DML location, methylation difference, p-value, q-value, and annotation:

- [Genes with 1 DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-10-01-Genes-with-1-DML.csv)
- [Genes with 2 DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-10-01-Genes-with-2-DML.csv)
- [Genes with 3 DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-10-01-Genes-with-3-DML.csv)
- [Genes with 4 DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-10-01-Genes-with-4-DML.csv)
- [Genes with 5 DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-10-01-Genes-with-5-DML.csv)

In Excel, I collated DML information for each gene, and counted how many DML were hypermethylated vs. hypomethylated:

- [Genes with 2 DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-10-01-Genes-with-2-DML-withCounts.csv)
- [Genes with 3 DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-10-01-Genes-with-3-DML-withCounts.csv)
- [Genes with 4 DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-10-01-Genes-with-4-DML-withCounts.csv)
- [Genes with 5 DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-10-01-Genes-with-5-DML-withCounts.csv)

I combined the chromosome information with the number of genes in each DML to create my pride and glory, a beautiful multipanel plot.

![Screen Shot 2019-10-03 at 7 25 54 PM](https://user-images.githubusercontent.com/22335838/68337381-35be1c00-0095-11ea-841a-49187b30ca69.png)

**Figure 6**. Number of DML in chromosomes, number of DML in genes, and number of DML in genes with more than 1 DML. There is only 1 gene with 5 DML, with 4 hypermethylated DML and 1 hypomethylated DML.

There's no clear pattern of hyper/hypomethylation breakdowns in genes with multiple DML. Interesting find, but I have no idea what that could mean.

### Scaled DML distributions

One thing Liew et al. did was look at percent methylation in each exon and intron type. While that seems really interesting, I realized I couldn't do this for my data because 1) the *C. virginica* GFF formatting makes it hard to parse through and determine which exon/intron are which and 2) not all genes have the same genomic architecture! After brainstorming with Shelly, I decided to scale each gene from 0% to 100% and see wehre DML occur. This is useful to see if methylation occurs in any consistent location for each gene.

To start, I calculated the length of each gene. I then calculated the absolute position of the DML in the gene, and finally, the scaled position:

```
DMLGeneAnnotationNomRNA$geneLength <- DMLGeneAnnotationNomRNA$gene.end - DMLGeneAnnotationNomRNA$gene.start #Calculate gene length

DMLGeneAnnotationNomRNA$absPosition <- DMLGeneAnnotationNomRNA$start - DMLGeneAnnotationNomRNA$gene.start #Calculate the absolute position of the DML in the gene

DMLGeneAnnotationNomRNA$scaledPosition <- DMLGeneAnnotationNomRNA$absPosition / DMLGeneAnnotationNomRNA$geneLength #Calculate the scaled position of the DML in the gene
```

Next, I separated out hyper- and hypomethylated DML. My goal was to make a mirror plot, with information for hypermethylated DML on top and hypomethylated DML on the bottom. I found [this code](https://genefish.wordpress.com/2019/10/09/creating-plots-mirrored-along-the-y-axis/), and used it as a starting point for my second pride and joy.

<img width="698" alt="Screen Shot 2019-10-09 at 4 10 30 PM" src="https://user-images.githubusercontent.com/22335838/68338463-57200780-0097-11ea-88ab-4ac3dd6868d0.png">

**Figure 7**. Distribution of hyper- and hypomethylated DML across a theoretical gene.

Again, no clear patterns! Hyper- and hypomethylated DML seem to occur across the gene and aren't concentrated in any one area.

Now time to wrap my brain around more text revisions.

### Going forward

2. Update methods and results
3. Update paper repository
4. Revise the discussion
5. Revise the introduction
7. Revise my abstract
8. Address any new edits and clean up the text
9. Post the paper on bioRXiv
10. Submit to the Special Issue

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
