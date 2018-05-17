---
layout: post
comments: true
title: Gonad Methylation Analysis Part 14
tags: virginica bismark MBDSeq
---

## A spoiled enchilada

I [aligned](https://yaaminiv.github.io/Gonad-Methylation-Part13/) all of my files, and proceeded to deduplication, sorting, and indexing in [this Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-05-04-Gonad-Methylation-Full-Samples-Bismark.ipynb). All of that was successful!

I was ready to use `bismark_methylation_extractor` but I ran into an error instead. To run `bismark_methylation_extractor`, I need to provide the path to my deduplicated — but unsorted — .bam files. The error file says that I need to use unsorted files, but I'm already directing the program to my unsorted files! I posted [this issue](https://github.com/RobertsLab/resources/issues/262) to figure it out. 

Since I'm replicating Steven's code, the only difference in my work and his is that I used the default --score_min option. However, he replicated my work in [this notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/sr_2018-05-04-Gonad-Methylation-Full-Samples-Bismark.ipynb) and [this notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/sr2_2018-05-04-Gonad-Methylation-Full-Samples-Bismark.ipynb), but did not get an error! Thinking everything was peachy, I reran my code and still got the same error!

![error](https://user-images.githubusercontent.com/22335838/40192079-2e4d2de2-59b8-11e8-91c8-dfcd1bd0ab99.png)

I am officially stumped and unable to continue these analyses for now. Back to the DNR paper!

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
