---
layout: post
comments: true
title: DML Analysis Part 24
tags: virginica MBDSeq DML
---

## Some information I've missed

I met with Steven on Tuesday, and he suggested I do a few things:

1. Figure out if the [mRNA genome feature file](http://eagle.fish.washington.edu/Cvirg_tracks/C_virginica-3.0_Gnomon_mRNA.gff3) overlaps with [introns](http://eagle.fish.washington.edu/Cvirg_tracks/C_virginica-3.0_intron.bed) and exons
2. Count the number of unique genes the gene background overlapped with and get Uniprot codes

### Feature file overlaps

TL;DR: Yes, the mRNA feature file includes introns and exons.

![screen shot 2019-02-27 at 2 33 18 pm](https://user-images.githubusercontent.com/22335838/53602368-60012680-3b63-11e9-923e-783066dd4206.png)

**Figure 1**. Various genome feature files in IGV.

I opened the tracks in IGV and found they overlapped. I have to consider this as I think about what the overlaps between DML and DMR and exon, intron, or mRNA coding regions actually mean. My guess is that I need to consider exon and intron overlaps as a subset of the mRNA overlaps. Unless the mRNA coding region file has information that isn't an intron or exon, I could just compare exon and intron overlaps instead of using mRNA overlaps.

### Unique genes from gene background-mRNA overlaps

I went back to my [R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-Gene-Enrichment-Analysis.Rmd) and subsetted unique Genbank IDs from the file with gene background-mRNA overlaps and Uniprot codes. I used the following code:

```
uniqueBackgroundmRNAblast <- subset(backgroundmRNAblast, !duplicated(backgroundmRNAblast$Genbank)) #Subset the unique Genbank IDs from backgroundmRNAUniprot and save as a new dataframe.
nrow(uniqueBackgroundmRNAblast$Genbank) #Count the number of unique genes
```

The gene background overlapped with 14,943 unique genes. I saved the subsetted information in [this file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-28-mRNA-Gene-Background-Uniprot-Unique.csv).

### Going forward

1. Describe gene products for all remaining DML and DMR overlaps
2. Compare genes with hypermethylated vs. hypomethylated loci and regions

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
