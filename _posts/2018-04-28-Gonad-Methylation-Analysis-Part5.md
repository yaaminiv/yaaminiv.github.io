---
layout: post
comments: true
title: Gonad Methylation Analysis Part 5
tags: virginica bismark MBDSeq
---

## When the stars (and sequences) align

I successfully completed my [genome preparation](https://yaaminiv.github.io/Gonad-Methylation-Analysis-Part4/) in Bismark yesterday, so I started on my sequence alignment. Based on the Bismark manual and [Steven's lab notebook post](https://sr320.github.io/The-Bismark-boat/), I used the following parameters in my [Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-04-27-Gonad-Methylation-Bismark.ipynb):

1. Path to Bismark
2. -u 10,000: Allows me to subset the first 10,000 reads from each input file
3. --nondirectional: See [this issue](https://github.com/RobertsLab/resources/issues/216)
4. --score_min L,0,-1.2: This was a parameter Steven used to allow for mismatches in alignment
5. --genome + path to the folder with the .fa genome and bisulfite genome directories Bismark prepared earlier
6. Path to sequence files for alignment

The Bismark run was completed for each file, but I did end up with a `gunzip` error for each of my files:

![untitled](https://user-images.githubusercontent.com/22335838/39400223-3d6fdff8-4ae1-11e8-902c-d208527f442b.png)

I'm not entirely sure what this means, so I [opened a new issue](https://github.com/RobertsLab/resources/issues/236). Once I fix this `gunzip` problem, I can move on to the methylation extraction and start to analyze all of the .bam files this alignment step produced!

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
