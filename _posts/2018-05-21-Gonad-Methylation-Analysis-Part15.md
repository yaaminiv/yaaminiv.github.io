---
layout: post
comments: true
title: Gonad Methylation Analysis Part 15
tags: virginica bismark MBDSeq
---

## A revised enchilada

I [tried rerunning `bismark_methylation_extractor`](https://yaaminiv.github.io/Gonad-Methylation-Analysis-Part14/) before the weekend and still got an error. Steven suggested I run the analysis on [these files](https://github.com/RobertsLab/project-virginica-oa/tree/master/data/dignore) instead. The files were created by Steven when he tried (and failed) to replicate my error in [this notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/sr2_2018-05-04-Gonad-Methylation-Full-Samples-Bismark.ipynb).

The command worked! I'm not sure what the difference is between Steven's files and my files. Understanding that difference will be important for reproducibility.

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
