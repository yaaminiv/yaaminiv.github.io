---
layout: post
comments: true
title: Gonad Methylation Analysis Part 16
tags: virginica bismark MBDSeq
---

## A new enchilada

(P.S. I think I really want enchiladas now)

After [fixing the `bismark_methylation_extractor` issue](https://yaaminiv.github.io/Gonad-Methylation-Analysis-Part15/), Steven suggested I duplicate my notebook and rerun the analysis on a subset of the data. I created [this notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-05-22-Gonad-Methylation-Subset.ipynb) and started rerunning `bismark` to align the sequences to the prepared genome. I then deduplicated, sorted, and indexed the .bam files, and extracted methylation calls successfully! I also completed the HTML and Summary Report steps. All outputs from this notebook can be found [in this folder](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-05-22-Bismark-Subset).

The next step is to duplicate the notebook I created today, remove the -u argument, and run the commands on the full dataset. I created [this notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-05-22-Gonad-Methylation-Full-Samples.ipynb) and started to run the alignment. I'll check on it in a few days!

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
