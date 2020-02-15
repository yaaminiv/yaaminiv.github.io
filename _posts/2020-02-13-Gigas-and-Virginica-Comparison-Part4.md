---
layout: post
comments: true
title: Gigas and Virginica Comparison Part 4
tags: manchester gigas-broodstock virginica methylation-islands bedtools
---

## Reframing my questions and making a poster

I have (almost) all analyses completed, so it's time to make the poster! I showed Steven the analyses I did thus far, which helped me frame the sub-questions they correspond to.

### Where in the genome are MI?

I [calculated how much individual features overlapped with MI](https://yaaminiv.github.io/Gigas-and-Virginica-Comparison-Part2/), but I don't have any visual representation of the genomic location of MI. Steven suggested I create a stacked barplot similar to those I've made in the past.

#### *C. virginica*

To start, I returned to [my Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb). Since `intersectBed` output can change depending on which file you list first, I characterized two different kinds of overlaps:

- MI first: Where are MI in the genome?
- Features first: How much of a single feature overlaps with MI (see below)?

All output is saved in [this folder](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-11-01-DML-and-DMR-Analysis). The naming conventions match which file was argument `-a` and which was `-b` (ex. MI-Exon means that MI was listed as `-a`). For two different features, it didn't make sense to look at both kinds of overlaps. Understanding which MI are in intergenic regions is important, but I don't know what it would tell me if there is an intergenic region inside my MI. Also, `bedtools` wouldn't let me switch the inputs to even answer that question. Knowing which DML are in MI is important, but since they are single loci doing the opposite command will lead to the same output.

**Table 1**. Characterizing two different kinds of overlaps between MI and various genome features in *C. virginica*

|      **Feature**      	| **MI Location in Feature** 	| **Individual Feature Overlaps with MI** 	|
|:---------------------:	|:--------------------------:	|:---------------------------------------:	|
|         Exons         	|            22705           	|                  240133                 	|
|         Intron        	|            28730           	|                  92472                  	|
|         Genes         	|            30773           	|                  15009                  	|
|          mRNA         	|            29805           	|                  29483                  	|
| Transposable Elements 	|            25085           	|                  107926                 	|
|   Putative Promoters  	|            4217            	|                   8846                  	|
|         Other         	|            1154            	|                   N/A                   	|
|          DML          	|             N/A            	|                   537                   	|

#### *C. gigas*

I rinsed and repeated with *C. gigas* MI in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb).

**Table 2**. Characterizing two different kinds of overlaps between MI and various genome features in *C. gigas*

|      **Feature**      	| **MI Location in Feature** 	| **Individual Feature Overlaps with MI** 	|
|:---------------------:	|:--------------------------:	|:---------------------------------------:	|
|         Exons         	|            16135           	|                  49607                  	|
|         Intron        	|            18322           	|                  51980                  	|
|         Genes         	|            19278           	|                   8761                  	|
| Transposable Elements 	|            2988            	|                   5868                  	|
|   Putative Promoters  	|            2498            	|                   2746                  	|
|         Other         	|            3084            	|                   N/A                   	|
|          DML          	|             N/A            	|                   384                   	|

#### Stacked barplot

I updated [this file with overlap information](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/2020-02-12-Combined-Overlap-Proportions.csv) to include MI locations. I imported the revised file into [this R Markdown document](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/2019-09-15-Proportion-Test.Rmd) to analyze the distributions. The MI locations in various genomic features are significantly different between the two species (χ<sup>2</sup> = 21.026, df = 4, p-value = 0.0003129).

<img width="824" alt="Screen Shot 2020-02-13 at 10 04 36 PM" src="https://user-images.githubusercontent.com/22335838/74505761-d437f580-4eac-11ea-9a00-7dfcd210ed1a.png">

**Figure 1**. Location of MI in *C. virginica* and *C. gigas* genomes.

### Are genes in MI conserved?

Which genes are in MI for both species? I started working on that question in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb). My plan:

1. Annotate Gene-MI overlaps for each species with Uniprot Accession codes
2. Match Uniprot Accesion codes!

Annotating the *C. gigas* overlaps with Uniprot Accession codes was easy, but *C. virginica* was a bit more complicated. I identified overlaps using a BEDfile that didn't have gene information! Additionally, I have several files with *C. virginica* Uniprot Accession codes and matching Genbank IDs, but those are found in mRNA.

To fix the first issue, I went back to [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb) and used the `gff` gene track to find the overlaps instead. Easy!

To fix the second issue, I matched gene IDs in the gene-MI overlap file with Genbank IDs in the mRNA track to then match to Uniprot Accesion codes (confusing I know). When it came down to merging Uniprot Accession codes, I found that there were no common genes in MI between the two species! Wild.

### How much of a single feature overlaps with MI?

#### Fixing `bedtools` output

Earlier, I [tried finding overlaps between *C. gigas* MI and genome feature tracks](https://yaaminiv.github.io/Gigas-and-Virginica-Comparison-Part2/) in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb). My output wasn't correctly tab-delimited, so I posted [this issue](https://github.com/RobertsLab/resources/issues/840). After trying various slight code modifications, Sam noticed I wasn't using the most updated `bedtools` version. I followed the instructuions [here](https://bedtools.readthedocs.io/en/latest/content/installation.html) to download version 2.29.1, then reran my code. Updating the version was enough to fix my output issue! Turns out the only thing it needed to fix was the actual output, so the overlap counts I have don't need to be revised.

#### Revising the figure

I returned to [this R Markdown file](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2020-02-11-Characterizing-CpG-Methylation/2020-02-11-Characterizing-Methylation-Islands.Rmd) to see if the distributions were different and fix my MI overlap boxplots. I found that the distribution of individual genome features in MI is significantly different between species (χ<sup>2</sup> = 23.599, df = 2, p-value = 7.507e-06). I  modified the y-axis label in the figure since it wasn't accurate and added the revised *C. gigas* intron data.

<img width="715" alt="Screen Shot 2020-02-13 at 4 11 27 PM" src="https://user-images.githubusercontent.com/22335838/74489869-8951ba00-4e7b-11ea-9c26-7fc44b6bff4c.png">

**Figure 2**. Final boxplots showing genome feature overlaps with methylation islands.

### What biological processes are represented in DML compared to the full genome?

#### *C. virginica*

Since I already made a back-to-back GOSlim plot for the paper submission, all I needed to do was recolor it to match the poster color scheme. I returned to [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-Gene-Enrichment-with-GO-MWU.Rmd) and did just that. Instead of grouping GOSlim terms by colors, I thought I'd just use different colors for the full genome and DML.

<img width="800" alt="Screen Shot 2020-02-14 at 10 52 57 AM" src="https://user-images.githubusercontent.com/22335838/74558980-29f5b780-4f18-11ea-8415-9606dbf6faf2.png">

**Figure 3**. GOSlim terms in *C. virginica* genome and DML

#### *C. gigas*

For WSN, I examined GOSlim terms for genes with DML...OR SO I THOUGHT. When I went back to [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb), I found that I annotated genes containing DML with Uniprot Accession codes, but I didn't actually subset my GOSlim list so that it only included those genes when making my figure in [this R Markdown file](https://yaaminiv.github.io/WGBS-Analysis-Part8/). Since my goal is to make a back-to-back GOSlim plot for genes and genes with DML, I did that subsetting.

<img width="806" alt="Screen Shot 2020-02-14 at 10 46 41 AM" src="https://user-images.githubusercontent.com/22335838/74558521-4a714200-4f17-11ea-860e-f797c63b46f3.png">

**Figure 4**. GOSlim terms in *C. gigas* genome and DML

Looking at both figures, there are two things that stand out:

1. Both genomes have remarkably similar distributions of GOSlim terms
2. DML processes are also similar, but there are potentially more developmental process GOSlim terms associated with *C. gigas* DML-gene overlaps.

While these figures are good, they take up too much space on the poster! Steven suggested I divde the percent of genes with DML with the percent of genes for each GOSlim term to get relative proportions. I could then plot the *C. virginica* and *C. gigas* data back-to-back.

I made the plot using the same code I used for the other plots, but for some reason it adds "0" to the x-axis label 3 times...unsure why that's happening but I can fix it later.

<img width="811" alt="Screen Shot 2020-02-15 at 10 45 28 AM" src="https://user-images.githubusercontent.com/22335838/74593458-49f0ae00-4fe0-11ea-970b-e633896357da.png">

**Figure 5**. Relative proportion of GOSlim terms for genes with DML vs. all genes for *C. virginica* and *C. gigas*

### Final poster

I tried my hand ata the #betterposter format! It was difficult to figure out what the main takeaway should be since there were a lot of interesting components, but we settled on some summary statistics from MI and DML as well as the GOSlim plot. Here's hoping people will be interested at Ocean sciences next week!

<img width="769" alt="Screen Shot 2020-02-15 at 1 37 30 PM" src="https://user-images.githubusercontent.com/22335838/74595602-554fd380-4ff8-11ea-9f0e-817b12407858.png">

### Going forward

4. Send for printing!

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
