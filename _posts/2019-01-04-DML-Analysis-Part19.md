---
layout: post
comments: true
title: DML Analysis Part 19
tags: virginica MBDSeq bedtools
---

## Overlaps with `methylKit` gene background

The next big step in the *C. virginica* project is to conduct a gene enrichment (if necessary...we'll get to this later). I started [this notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-12-02-Gene-Enrichment-Analysis.ipynb) with that intention. Before I could get into gene enrichment or description, I needed to characterize overlaps between the gene background used in `methylKit` and various genome feature files. I started this small analysis in December, but didn't follow through because I was focusing on my MEPS resubmission.

The gene background from `methylKit` is formed using `unite`. I took this background and saved it as a BEDfile [in this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd). I used `intersectBed` to find the overlaps between the gene background, exons, introns, mRNA coding regions, and transposable elements. Finally, I calculated the overlap proportions. My next step is to use these overlap proportions, and [overlap proportions from DML and DMR to conduct a proportion test](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb).

### Going forward

1. Conduct a proportion test with gene background and DML/DMR overlaps with genome feature files
2. See how `min_cov`, alignment stringency, or SNPs affect clustering
3. Determine if a formal gene enrichment is necessary
4. If necessary, select the most appropriate gene enrichment method
5. Describe functions of most interesting genes with DML and DMR

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
