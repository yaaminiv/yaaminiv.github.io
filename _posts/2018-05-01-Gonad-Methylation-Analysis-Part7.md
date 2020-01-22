---
layout: post
comments: true
title: Gonad Methylation Analysis Part 7
tags: virginica methylKit macau MBDSeq
---

## M&M: methylKit and MACAU

Now that I've run samples through `bismark`, my next step is to analyze the methylation data produced. This can be done with `methylKit` or `MACAU` in R. Steven tasked with installing both programs and reading the manuals, so I did just that (kind of).

M1: [`methylKit`](http://bioconductor.org/packages/3.7/bioc/vignettes/methylKit/inst/doc/methylKit.html)
`methylKit` is made for reduced-representation bisulfite sequencing, but can apparently handle whole-genome bisulfite sequencing data (i.e. my data) if it's in the "right format." I installed the package on the Mac mini in [this R script](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-05-01-MethylKit/2018-05-01-MethylKit-Analysis.R). It was however, extremely confusing and difficult because `data.tables` (a dependency) wouldn't properly compile. I think I was able to figure it out by just entering "no" every time it asked me to do something, but I'll have to revisit it. 

The manual is detailed, but confusing. I don't think I'll get a feel for the order of operations until I actually start using it. I could definitely refer to [Steven's lab notebook post](https://sr320.github.io/MethylKittens/) to figure it out! 

M2: [`MACAU 2.O`](http://www.xzlab.org/software/macau/MACAU-R-Manual.pdf)

MACAU somehow makes more sense to me, even though it's all about modeling (thanks, Skalski). It fits a binomial mixed model to the data to identify differential methylation. However, it can only run on a linux machine. I'll probably run `methylKit` first on the Mac mini, then jump over to MACAU. 

One thing I need to figure out before starting is what the difference between these two programs are. I'll probably also look at the methods sections of methylation papers just to get a clearer understanding of what I'm doing.

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
