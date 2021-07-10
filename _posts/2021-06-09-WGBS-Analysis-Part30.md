---
layout: post
comments: true
title: WGBS Analysis Part 30
tags: manchester gigas-broodstock topGO
---

## Finishing up analyses and figures!

The time has come to wrap this shit up (or at least...wrap it up enough for my defense). I'm going to perform necessary statistical tests, make figures, and conduct GOterm enrichment!

### Principal Components Analysis

I started by making figures associated with my `methylKit` output in [this R Markdown document](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.Rmd#L493). The first thing I wanted to do was make a [PCA to visualize global methylation patterns](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/06-methylKit/figures/all-sample-PCA.pdf). I used code from my *C. virginica* paper to do this.

![Screen Shot 2021-06-06 at 12 43 58 PM](https://user-images.githubusercontent.com/22335838/122095626-7a0db400-cdc2-11eb-8b7c-26e8e9c117e1.png)

**Figure 1**. PCA

Based on this figure, it's easy to see the lack of clear global methylation differences by treatment. In a revised version of this figure, I want to add different symbols that indicate maturation stage to see if that contributes to the commonalities in global methylation profiles.

### DML heatmaps

To visualize methylation differences between treatments at DML, I wanted to create a multipanel figure with 4 different heatmaps: 1) female-DML that don't overlap with SNPs, 2) female-DML that overlap with SNPs, 3) indeterminate-DML that don't overlap with SNPs, and 4) indeterminate-DML that overlap with SNPs. My hope was that breaking down the heatmap by these categories will allow me to look at hyper- and hypomethylation differences. For example, maybe all my SNP DML are hypermethylated, and all of my non-SNP DML are hypomethylated! Well...maybe not that extreme but I thought I should investigate.

In this [R Markdown document](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.Rmd#L540), I created four separate heatmaps using `heatmap.2`. Unfortunately, `heatmap.2` calls `plot.new()` each time it's run, so I can't make a multipanel plot in R. Instead, I created a multipanel with [this InDesign document](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/06-methylKit/figures/multipanel-heatmap.indd), then [saved it as a pdf](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/06-methylKit/figures/multipanel-heatmap.indd).

![Screen Shot 2021-06-06 at 4 50 06 PM](https://user-images.githubusercontent.com/22335838/122095609-767a2d00-cdc2-11eb-8645-c108e39e0edf.png)

**Figure 2**. Multipanel heatmap

So no drastic differences in methylation patterns based on DML identity. I'll revise this figure to only include one heatmap for female- or indeterminate-DML, and maybe add an asterisk to the rows associated with SNPs to see if they cluster together.

### Chromosomal distribution

The last thing I wanted to do with in my [`methylKit` R Markdown document](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.Rmd#L752) is look at the distribution of DML in *C. gigas* chromosomes. Essentially, I want to know if all the DML are located in on chromosome, or if they are distributed relatively evenly. To do this, I counted the number of DML per chromosome. I also normalized the number of DML in each chromosome by the number of CpGs present, as DML can't exist where there are no CpGs! I then created a [stacked barplot with the number of DML in each chromosome normalized by the number of CpGs, then added a secondary axis with the number of genes in each chromosome](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/06-methylKit/figures/DML-chr-distribution.pdf).

<img width="944" alt="Screen Shot 2021-06-06 at 11 47 57 PM" src="https://user-images.githubusercontent.com/22335838/122095613-77ab5a00-cdc2-11eb-9e15-94d855276d6a.png">

**Figure 3**. Chromosomal distribution of DML

The chromosomes with the most DML had the most genes, so that tracks.

### Distribution of CpGs in genome features

To characterize the general methylation landscape, I created [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/09-General-Methylation-Landscape.Rmd). I used the 5x unionBED methylation data to create a frequency distribution of CpGs and methylation status. I also used the unionBED overlaps with genome feature files to conduct a contingency test and create a stacked barplot. The distribution of highly methylated CpGs was significantly different than all 5x CpGs detected by WGBS.

<img width="944" alt="Screen Shot 2021-06-07 at 3 04 39 AM" src="https://user-images.githubusercontent.com/22335838/122095627-7aa64a80-cdc2-11eb-8e9a-e679b5bc437b.png">

**Figure 4**. [Frequency distribution and genomic location of CpGs](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/09-methylation-landscape/figures/meth-landscape-fig.pdf)

### Distribution of DML in genome features

Finally, I created a [stacked barplot to visualize the distribution of DML in the genome](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/10_DML-characterization/figures/DML-feature-overlaps.pdf)! Based on my contingency test results, the number of female-DML in CDS and downstream flanks differed significantly from highly methylated CpGs, and the distribution of indeterminate-DML also differed significantly from highly methylated CpGs with respect to upstream flanks, lncRNA, and intergenic regions, in addition to CDS and downstream flanks.

<img width="944" alt="Screen Shot 2021-06-07 at 4 05 59 AM" src="https://user-images.githubusercontent.com/22335838/122095632-7bd77780-cdc2-11eb-9161-679bb58d3c50.png">

**Figure 5**. Genomic location of DML

### `topGO` enrichment

Finally, I conducted a GOterm enrichment test using `topGO`, following the workflow I used for the [Hawaii data](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part21/) in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/11-GOterm-Annotation.ipynb). Unfortunately, I didn't find any enriched terms in my data! I returned to [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/11-GOterm-Annotation.ipynb) to add gene product information to my list of annotated genes with female-DML. To do this, I isolated the transcript ID and product information from the nucleotide sequence I used for `blastx`:

```
#Convert to tab-delimited file
!perl -e '$count=0; $len=0; while(<>) {s/\r?\n//; s/\t/ /g; if (s/^>//) { if ($. != 1) {print "\n"} s/ |$/\t/; $count++; $_ .= "\t";} else {s/ //g; $len += length($_)} print $_;} print "\n"; warn "\nConverted $count FASTA records in $. lines to tabular format\nTotal sequence length: $len\n\n";' \
../../genome-feature-files/ncbi-genomes-2021-05-04/GCF_902806645.1_cgigas_uk_roslin_v1_rna_from_genomic.fna \
> ../../genome-feature-files/ncbi-genomes-2021-05-04/GCF_902806645.1_cgigas_uk_roslin_v1_rna_from_genomic.tab

#Isolate annotation
!cut -f2 ../../genome-feature-files/ncbi-genomes-2021-05-04/GCF_902806645.1_cgigas_uk_roslin_v1_rna_from_genomic.tab \
| tr "[]" "\t" \
| cut -f6,8 \
| tr "=" "\t" \
| cut -f2 \
> cgigas_uk_roslin_v1_rna_from_genomic_annotation.tab

#Isolate transcript ID
!cut -f2 ../../genome-feature-files/ncbi-genomes-2021-05-04/GCF_902806645.1_cgigas_uk_roslin_v1_rna_from_genomic.tab \
| tr "[]" "\t" \
| cut -f6,8 \
| tr "=" "\t" \
| cut -f4 \
> cgigas_uk_roslin_v1_rna_from_genomic_transcript.tab

#Paste annotation and transcript together
#Extract lines with a transcript ID (XM)
#Sort
#Save output
!paste cgigas_uk_roslin_v1_rna_from_genomic_transcript.tab cgigas_uk_roslin_v1_rna_from_genomic_annotation.tab \
| grep "XM" \
| sort \
> cgigas_uk_roslin_v1_rna_from_genomic_annot.transcript.tab

#Join files to get product names for genes with DML
!join -1 1 -2 1 -t $'\t' \
DML-pH-75-Cov5-Fem.GeneIDs.geneOverlap.transcriptIDs.GOAnnot \
cgigas_uk_roslin_v1_rna_from_genomic_annot.transcript.tab \
| uniq \
| sort \
> DML-pH-75-Cov5-Fem.GeneIDs.geneOverlap.transcriptIDs.GOAnnot.productID
```

### Going forward

1. Run `methylKit` with 4 vs. 4
2. Revise figures
1. Perform randomization test
2. Update `mox` handbook with R information
4. Incorporate other epigenomic data
5. Identify overlaps between SNP data and other epigenomic data
7. Identify methylation islands and non-methylated regions
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
