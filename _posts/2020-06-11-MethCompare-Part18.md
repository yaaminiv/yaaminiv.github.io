---
layout: post
comments: true
title: MethCompare Part 18
tags: MethCompare bedtools
---

## Formatting individual sample information

In [this issue](https://github.com/hputnam/Meth_Compare/issues/68) we've discussed how to revise our CpG methylation status and genomic location statistical analyses. We know we want to compare proportoins while investigating if sequencing method affects the proportion of different methylation statuses or in genomic locations. I posted some suggestions, but in the meantime I thought I could obtain individual-level proportion data.

Thankfully for me, most of the pipeline was already set up! In [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x.ipynb) I counted CpGs for each methylation status and in various genomic features. The only things I needed to modify were [ensuring I used `bedtools -u`](https://yaaminiv.github.io/MethCompare-Part17/), adding code for upstream and downstream flank overlaps, and adding the path to the explicit intragenic region tracks. I took the output files (line counts) and used them in [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.Rmd) and used them to create summary tables:

***M. capitata***:

- [Methylation status table](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-CpG-Type.txt)
- [Methylation status exploratory figure](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-CpG-Type.pdf)
- [Genomic location counts](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap_union-Genomic-Location-Counts.txt)
- [Genomic location percents](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap_union-Genomic-Location-Percents.txt)

***P. acuta***:

- [Methylation status table](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-CpG-Type.txt)
- [Methylation status exploratory figure](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-CpG-Type.pdf)
- [Genomic location counts](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-Genomic-Location-Counts.txt)
- [Genomic location percents](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-Genomic-Location-Percents.txt)

### Going forward

1. [Conduct statistical analysis](https://github.com/hputnam/Meth_Compare/issues/68)
1. [Locate TE tracks](https://github.com/hputnam/Meth_Compare/issues/56)
2. [Characterize intersections between data and TE, and create summary tables]
4. Look into program for mCpG identification

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
