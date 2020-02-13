---
layout: post
comments: true
title: Gigas and Virginica Comparison Part 3
tags: manchester gigas-broodstock virginica DML GOslim
---

## *C. gigas* vs. *C. virginica*: DML

Now the good stuff: looking at DML in response to ocean acidification in *Crassostrea spp.* gonad tissue!

### DML quantity and locations

When I was [first analyzing my *C. gigas* DML](https://yaaminiv.github.io/WGBS-Analysis-Part6/), I was pretty sure the numbers weren't identical. I decided to look at the number of DML and DML-genome feature track overlaps.

**Table 1**. Comparison of DML number and overlaps with various genome feature files. *C. gigas* DML were created with 10x data and 2 pooled samples, and *C. virginica* DML were created with 5x data and 10 individual samples.

|             **Feature**             	| ** _C. virginica_ ** 	| ** _C. gigas_ ** 	|
|:-----------------------------------:	|:--------------------:	|:----------------:	|
|               Quantity              	|          598         	|        628       	|
|         Overlaps with Exons         	|      368 (61.5%)     	|    157 (25.0%)   	|
|        Overlaps with Introns        	|      192 (32.1%)     	|    285 (45.4%)   	|
|         Overlaps with Genes         	|      560 (93.6%)     	|    442 (70.4%)   	|
| Overlaps with Transposable Elements 	|       57 (9.5%)      	|     8 (1.3%)     	|
|   Overlaps with Putative Promoters  	|       42 (7.0%)      	|     24 (3.8%)    	|
|     Overlaps with Other Regions     	|       21 (3.5%)      	|    165 (26.3%)   	|

At first glance, there aren't that many more *C. gigas* DML than there are *C. virginica* DML. However, they're distributed differently! In *C. gigas*, only 70.4% of DML are in genes as opposed to 93.6% in *C. virginica*. Within genes, *C. gigas* DML are more heavily weighted towards introns, while *C. virginica* DML are primarily in exons. In this [R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/2019-09-15-Proportion-Test.Rmd), I conducted a proportion test to see if [the DML distribution bewteen species](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/2020-02-12-Combined-Overlap-Proportions.csv) truly were different. Based on a chi-squared test, the distributions of DML across genomic features are significantly between speceis (χ<sup>2</sup>  = 38.516, df = 4, P-value = 8.767e-08).

<img width="824" alt="Screen Shot 2020-02-12 at 11 24 06 PM" src="https://user-images.githubusercontent.com/22335838/74410796-c6239f80-4dee-11ea-824f-9accea725bed.png">

**Figure 1**. Distribution of *C. gigas* and *C. virginica* DML in various genome features.

### Comparing GOSlim functions of common genes with DML

Even though there are differences in where DML are located in their respective gneomes, there may be some genes in both species that have DML in response to ocean acidification. The first thing I needed to do was identify if there were any overlaps. I decided to try matching the annotated gene products associated with each DML list in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb). Late-night work delirium coupled with caffeine lead me to believe the best way to do this would be to look for partial matches in the gene product columns. I tried come `awk` code I found online, but I couldn't get it to work consistently. Depending on which list I used as my base, I got two different numbers of overlapping gene products! Annoyed, I posted [this issue](https://github.com/RobertsLab/resources/issues/838). Sam rightly pointed out that the best way to go about this would be to look at Uniprot Accession instead since they have one-to-one matches with proteins. While I briefly toyed with the idea of trying to use `diff`, I went back to Old Faithful: `join`:

```
#Join the 1st column in the first file with the 1st column in the second file
#The files are tab delimited, and the output should also be tab delimited (-t $'\t')
#Check head of output
#Count number of overlaps
!join -1 1 -2 1 -t $'\t' \
2019-06-20-Cv-DML-Gene-Annotation-sorted-reduced.tab \
DML-Gene-annot-reduced.tab  \
> 2020-02-11-Common-DML-Cv-Cg.txt
```

The output, found [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/2020-02-11-Common-DML-Cv-Cg.txt), has Uniprot Accession, Genbank ID, and CGI ID. The inputs were reduced versions of [annotated *C virginica*](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/2019-06-20-Cv-DML-Gene-Annotation-sorted-reduced.tab) and [annotated *C. gigas* ](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/DML-Gene-annot-reduced.tab) I made with `awk`. I created an expanded version of this common DML [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/2020-02-11-Common-DML-Cv-Cg.txt) that has product annotations from both DML sets. Even though I figured the products would be identical since they are associated with Uniprot Accession, I still wanted one column from each dataset.

Once I had the list of overlapping genes with DML, I eliminated duplicate entries, going from 117 to 81 overlaps. I then used CGI ID to [match these entries with GOSlim terms](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/commonBlastquery-GOslim.tab). I extracted unique biological process entries that did not include "other biological processes" distinctions and saved the output [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/commonBlastquery-GOslim-BP.sorted.unique.noOther). In total I had 53 unique GOSlim terms representing 81 common genes with DML between *C. gigas* and *C. virginica*! I don't know what I was expecting, but that really doesn't seem like much. Interesting.

In [this R Markdown file](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/2020-02-12-Comparing-Common-Genes-wit-DML.Rmd), I made some figures to characterize the GOSlim terms in common genes and in the original datasets. I'm really glad I settled on this figure format during WSN becuase it has come in handy.

<img width="823" alt="Screen Shot 2020-02-13 at 12 52 53 AM" src="https://user-images.githubusercontent.com/22335838/74417194-2c162400-4dfb-11ea-9734-ae8bb36bd00b.png">

**Figure 2**. GOSlim terms associated with common genes containing DML in *C. gigas* and *C. virginica*.

<img width="823" alt="Screen Shot 2020-02-13 at 1 31 15 AM" src="https://user-images.githubusercontent.com/22335838/74420580-882f7700-4e00-11ea-9305-546c3f204d18.png">

**Figure 3**. GOSlim terms associated with *C. virginica* genes with DML

<img width="825" alt="Screen Shot 2020-02-13 at 1 31 42 AM" src="https://user-images.githubusercontent.com/22335838/74420617-97aec000-4e00-11ea-843f-0f5bd671e12b.png">

**Figure 4**. GOSlim terms associated with *C. gigas* genes with DML

It's interesting that signal transduction, death, cell adhesion, and cell-cell signaling — processes represented in the *C. gigas* and *C. virginica* DML datasets — are not present in GOterms of common genes. Maybe this is due to [differences in the experimental set-up](https://yaaminiv.github.io/Gigas-and-Virginica-Comparison/) or of family and/or species-specific stress tolerances.

### Going forward

3. Draft poster for ASLO and get feedback
4. Finalize ASLO poster and send for printing by Thursday to get the ASLO discount!

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
