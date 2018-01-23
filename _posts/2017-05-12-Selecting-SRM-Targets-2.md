---
layout: post
comments: true
title: Selecting SRM Targets Part 2
tags: DNR DIA SRM
---

## Record scratch. Freeze frame.

Hi. I bet you're wondering how I got here. Yaamini, I thought you said you selected targets yesterday? Your [lab notebook says so](https://yaaminiv.github.io/Selecting-SRM-Targets/)! Well, I was a fool and tried calculating a bunch of coefficients of variance. And like Mr. T, I pity the fool.

At about 3 a.m., I realized that this [coefficient of variance method](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-05-11-Selecting-SRM-Targets.ipynb) wasn't statistically sound! Better late than never, right? I spoke with Emma this morning and she reminded me of MSStats. She used it before for DDA analysis to identify differentially expressed proteins (i.e. potential targets). I'm going to use MSStats to identify potential targets. Then, I'll merge that list with my [table of protein information](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_Preliminary_Analyses_20170321/all-proteins-go-terms/Proteins-GO-terms.tabular) (ex. GO terms, functions, processes, etc.) to learn more about them.

A huge shoutout to all of the stats classes I've ever taken for making me realize I wasn't doing things properly before it was too late!

In [this notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-05-12-Selecting-SRM-Targets-with-MSstats.ipynb), I detail the MSStats process, from adding it as a tool in Skyline to running analyses in R. I'm basing my analyses off of information in the [MSstats manual](https://bioconductor.org/packages/release/bioc/vignettes/MSstats/inst/doc/MSstats-manual.pdf).

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
