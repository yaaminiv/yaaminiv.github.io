---
layout: post
comments: true
title: Gonad Methylation Analysis Part 9
tags: virginica bismark MBDSeq
---

## The whole enchilada

In [this Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-05-04-Gonad-Methylation-Full-Samples-Bismark.ipynb), I ran the `bismark` alignment on the full range of my samples. The .txt file output can be found [here](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-05-04-Bismark-Full-Samples). The .bam files produced were several GBs! I saved them in [this OWL folder](http://owl.fish.washington.edu/spartina/Virginica-MBD/2018-05-04-Bismark-Full-Samples/). I also uploaded all .bam output from my file subsets [here](http://owl.fish.washington.edu/spartina/Virginica-MBD/2018-04-27-Bismark-Subset/).

My next step is to run these samples through `methylKit` in R. I'm going to test this first with the subset data I have, then start plugging along on these large files.

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
