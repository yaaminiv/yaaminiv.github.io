---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 28
tags: hawaii gigas-ploidy
---

## Parameterizing `methylKit`

Steven ran [a `methylKit` parameterization R script for me on klone](https://github.com/RobertsLab/resources/issues/2472). I'm (finally) following up on some of the comments and output!

### Evaluating `methylKit` output

The first thing I wanted to do was determine which `meth.diff` and `min.per.group` parameters provided the best balance between a conservative DML estimate and providing DML to analyze. Steven said the output from the script could be found in [this folder](https://gannet.fish.washington.edu/v1_web/owlshell/bu-github/project-oyster-oa/analyses/Haws_04.2-methylKit/DML/). However, there was only output for the ploidy DML for `min.per.group` 9 through 12! I'm missing information for `min.per.group = 8`, and I'm not sure why there isn't any pH output. I [posted a comment](https://github.com/RobertsLab/resources/issues/2472#issuecomment-5345440905) in the same issue.

### Going forward

1. Continue parameter testing for DML identification
2. Revise `methylKit` methods and results
4. `methylKIt` randomization test
1. ATAC-Seq data integration
2. `KOG-MWU` for *Crassostrea* methylation comparison
5. Revise discussion
7. Revise introduction
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)

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
