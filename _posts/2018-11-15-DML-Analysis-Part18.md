---
layout: post
comments: true
title: DML Analysis Part 18
tags: virginica MBDSeq DML bedtools
---

## Flanking analysis for mRNA coding regions

[In this notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb), I used `bedtools` (`flankBed` and `closestBed`) to conduct a flanking analysis. I intially thought that each method would give me the same result, but I now think that these analyses provide me with two slightly different results.

I will use `flankBed` to add 1000 bp regions to each mRNA coding region. I can then `intersect` flanks with various genomic feature files. This gives me a broad idea of what DML or DMR could potentially regulate expression of an mRNA coding region no more than 1000 bp away. I will use `closestBed` to find the closest non-overlapping DML or DMR to each mRNA coding region. This is zooming in a little closer to the mRNA coding region, but looking for DML or DMR that could be in a nearby promoter sequence that could regulate an mRNA coding region. All of the output I generated can be found in [this folder](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-14-Flanking-Analysis).

### `flankBed`

First, I used the following code to generate 1000 bp flanks up- and downstream of an mRNA coding region.

```
! {bedtoolsDirectory}flankBed \ #Path to flankBed
-i {mRNAList} \ #Path to mRNA GFF
-g 2018-11-14-Flanking-Analysis/2018-11-14-bedtools-Chromosome-Length.txt \ #Path to a list of start and stop positions for each chromosome. I pulled the data off of NCBI. The file can be found [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-14-Flanking-Analysis/2018-11-14-bedtools-Chromosome-Length.txt)
-b 1000 \ #Length of flank to add
> 2018-11-14-Flanking-Analysis/2018-11-14-mRNA-1000bp-Flanks.bed #Redirect output to a new file
```

Here's where I ran into some confusion. For the longest time, I thought my output was a repeat of my mRNA GFF, but with altered start and stop positions such that they incorporate the 1000 bp up- and downstream flanks. I posted [this issue](https://github.com/RobertsLab/resources/issues/488) to find a way to manipulate files and isolate up- and downstream flanks in different BEDfiles. When I used the code Sam suggested, I ended up with some funky results. Upon closer inspection of the two files...

<img width="783" alt="untitled" src="https://user-images.githubusercontent.com/22335838/48598845-dc0b9180-e919-11e8-9723-b9adec071fe7.png">

...I FOUND THAT `bedtools flank` ALREADY SEPARATES THE FLANKS OUT FOR YOU. I swear I think I spent atleast a few weeks operating under this wrong assumption. At least I'm now more proficient in some `bash` wizardry.

Although I did get just the flanks in a new document, I still wanted to separate up- and downstream flanks into separate files. This way, I could easily figure out where an overlapping region was with respect to the mRNA coding region in question. To do this, I used the following command:

```
awk '{ if (NR%2) print > "2018-11-14-Flanking-Analysis/2018-11-15-mRNA-Upstream-Flanks.bed"; \ #If the row number is odd, redirect the row to the upstream flank file
else print > "2018-11-14-Flanking-Analysis/2018-11-15-mRNA-Downstream-Flanks.bed" }' \ #If the row number is even, redirect it to the downstream flank file
2018-11-14-Flanking-Analysis/2018-11-14-mRNA-1000bp-Flanks.bed #Path to the input file of flanking regions
```

Once I separated up- and downstream flanks, I used `intersectBed` to find overlaps between the flanks and DML, DMR, and CG motifs. The CG motifs serve as a "background" for the other two. Upstream of mRNA coding regions, there were 95 overlaps for DML and 12 for DMR. Downstream of mRNA coding regions, there were 124 DML overlaps and 18 DMR overlaps.

### `closestBed`

I used the following code for `closestBed` to identify the closest nonoverlapping genomic feature to an mRNA coding region:

```
! {bedtoolsDirectory}closestBed \ #Path to program
-io \ #Ignore overlapping features
-a {mRNAList} \ #Path to mRNA GFF
-b {} \ #DMLList, #DMRList, or #CG motifs
-t all \ #In the case of a tie, report all matches
-D ref \ #Report distance to A in an extra column. Use negative distances to report upstream features with respect to the reference genome. B features with a lower (start, stop) are upstream
> 2018-11-14-Flanking-Analysis/2018-11-14-mRNA-Closest-NoOverlap-DMLs.txt #Redirect output to a new file
```

While I got an output file each time I ran `closestBed`, I also got the following error each run:

```
Error: Sorted input specified, but the file C_virginica-3.0_Gnomon_mRNA.gff3 has the following out of order record
NC_035780.1	Gnomon	mRNA	2413594	2416601	.	-	.	ID=rna199;Parent=gene122;Dbxref=GeneID:111129373,Genbank:XM_022475729.1;Name=XM_022475729.1;gbkey=mRNA;gene=LOC111129373;model_evidence=Supporting evidence includes similarity to: 2 Proteins;product=mucin-2-like;transcript_id=XM_022475729.1
```

I still got an output file, so I kept going...? May have to change that.

### Going forward

1. Update flanking analysis methods
2. Update flanking analysis results
3. Perform a gene enrichment for DML, DMR, and flanking analysis output

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
