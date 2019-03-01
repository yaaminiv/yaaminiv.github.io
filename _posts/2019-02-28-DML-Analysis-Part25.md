---
layout: post
comments: true
title: DML Analysis Part 25
tags: virginica MBDSeq gene-enrichment
---

## DML-mRNA gene product information

I repeated my [DMR-mRNA overlap product description procedure](https://yaaminiv.github.io/DML-Analysis-Part23/) with the DML-mRNA overlaps. I then pored over my [product summary file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2019-02-28-DML-mRNA-Product-Summary.txt). I slightly regret manually processing the file in Excel instead of finding a nifty way to do it in bash because it took me over an hour...HOWEVER I learned some interesting things.

There were some gene products, like multidrug resistance-associated protein and heat shock 75kDa, that were prominent in the DML file but not present in the DMR overlaps. I think this has to do with the fact that the same coding region can have loci that are both hyper- and hypomethylated. Since DMRs are looking at methylation differences across 100 bp, some of these opposite effects "cancel out" and are not significantly differentially methylated. 

Some coding regions had mulitple DMLs that were all hyper- or hypomethylated, but they had different methylation differences. It's possible for there to be several DMLs in a coding region, but spread out such that it does not constitute a DMR. [Since the mRNA coding regions include exons and introns](https://yaaminiv.github.io/DML-Analysis-Part24/), it may be valuable to consider exons and introns separately in all downstream analyses.

There were several gene products related to human cancers, which didn't make any sense. I did not annotate the genome myself, but it would be worth learning how genome annotation happens just so I can understand these annotations (or know to ignore them).

### Going forward

1. Repeat this process with exon, intron, transposable element overlaps
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
