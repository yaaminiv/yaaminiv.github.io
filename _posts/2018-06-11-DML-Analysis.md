---
layout: post
comments: true
title: DML Analysis
tags: virginica MBDSeq DML bedtools
---

## Finding overlaps between DMLs and other things

In this [Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-06-11-DML-Analysis.ipynb), I used `intersectBed` from `bedtools` to find overlaps between [DMLs](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-05-29-MethylKit-Full-Samples/2018-05-30-DML-Locations.bed) I identified in `methylKit`! I found the Genome Feature Tracks in the [Genomic Resources wiki page](https://github.com/RobertsLab/resources/wiki/Genomic-Resources). There were four feature tracks I could use:

1. Exons: The coding regions
2. Introns: Regions that are removed
3. mRNA: The things that make proteins!
4. CG motifs: Regions with CGs where methylation can occur

I used the following command to count the number of overlaps between my DMLs and the various feature tracks:

`````
! /Users/Shared/bioinformatics/bedtools2/bin/intersectBed \ #Path to program
-u \ #Just report the fact that one overlap was found in a region
-a ../2018-05-29-MethylKit-Full-Samples/2018-05-30-DML-Locations.bed \ #Path to DML bed file
-b {} \ #Path to feature track
| wc -l #Count lines
`````

I then used a similar code to write out the overlaps in a .txt document:

`````
! /Users/Shared/bioinformatics/bedtools2/bin/intersectBed \ #Path to program
-wb \ #Write the entry information from File B
-a ../2018-05-29-MethylKit-Full-Samples/2018-05-30-DML-Locations.bed \ #Path to DML bed file
-b {} \ #Path to feature track
> 2018-06-11-DML-{}.txt #New .txt file created
`````
I also found and counted overlaps between CG motifs and exons, introns, and mRNAs! The reason I did this was because if most of the CG motifs are in the exon regions, then we'd expect to find most of the methylation in the exons. All of my .txt files that weren't too big for Github can be found in [this folder](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-06-11-DML-Analysis).

One thing I noticed is that I found [more overlaps than Steven](https://github.com/sr320/nb-2018/blob/master/C_virginica/21-Bedtools.ipynb) did for all categories. My guess is that the default `score_min` option I used lead to more overlaps. It will be interesting to look into the differences. Another important thing to mention is that there are more exon overlaps than mRNA overlaps. This does not make sense to me, since exons are what code for mRNA. I'll [post an issue](https://github.com/RobertsLab/resources/issues/288) with this question.

I think my next step has to do with gene ontology, but I'm not entirely sure what that entails. Guess I'll [post another issue](https://github.com/RobertsLab/resources/issues/289).

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
