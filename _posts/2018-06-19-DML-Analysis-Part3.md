---
layout: post
comments: true
title: DML Analysis Part 3
tags: virginica MBDSeq DML bedtools
---

## Quantifying proportion of overlaps

Now that I know the [DMLs and CGs intersect with various feature tracks](https://yaaminiv.github.io/DML-Analysis/), I need to quantify the overlap proportions. I answsered the following questions in this [Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-06-11-DML-Analysis.ipynb):

1. What proportion of total overlaps does a certain feature track represent?
2. Out the total number of CG motifs, how many overlaped with a feature track?
3. Out the total number of DMLs, how many overlaped with a feature track?

It was simple enough to conceptualize and carry out! Just needed to count the number of DMLs or CG motifs, then count the number overlaps before summing and dividing in various ways. See my [previous notebook post](https://yaaminiv.github.io/DML-Analysis/) for code that counts the lines in BEDfiles and overlaps.

Here's the answer to the first question:

- Proportion exon overlap with total CG overlaps: 67.55% (0.6754709569888477)
- Proportion intron overlap with total CG overlaps: 26.06% (0.2606253947864305)
- Proportion mRNA overlap with total CG overlaps: 6.39% (0.06390364822472172)

Most of our CG overlaps come from exons, which code for proteins. Interesting!

Here are the answsers to the second and third questions:

| **Feature** | **% CG Overlaps** | **% DML Overlaps** |
|:-----------:|:-----------------:|:------------------:|
|     Exon    |        4.40       |        66.05       |
|    Intron   |        1.70       |        27.82       |
|     mRNA    |        0.42       |        92.04       |

Those numbers are different, so there's definitely some sort of enrichment going on! Maybe when I figure out that [gene set enrichment](https://yaaminiv.github.io/DML-Analysis-Part2/) everything will make sense.

That's a wrap on the gonad methylation stuff until I'm back in town!

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
