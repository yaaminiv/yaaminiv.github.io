---
layout: post
comments: true
title: WGBS Analysis Part 2
tags: manchester gigas-broodstock WGBS mox
---

## Fixing my `bismark` script for Mox

I [previously submitted my `bismark` job](https://yaaminiv.github.io/WGBS-Analysis/) on Mox, but it remained in the queue. Steven pointed out that my wall time overlapped with routine Mox maintenance, and that I should reduce it so the job finishes (or is terminated) by 9 a.m. on Tuesday. Once I reduced it, I ran into two problems:

- `path_to_bowtie` was not recognized
- Wildcards were not recognized in my `find | xargs` command

I posted [this issue](https://github.com/RobertsLab/resources/issues/742) to figure out why this happened. Looking at other `bismark` scripts Steven posted, I figured out that I could get the script to run if I modified the wildcard and chagned from `bismark 0.21.0` to `bismark 0.19.0`. I saved my modified script [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/scripts/2019-09-03-Bismark.sh) and started the job on Mox.

Unfortunately, I couldn't get one sample finished before maintenance started. Once maintenance is over, I'll restart my job with a longer wall time. I'm a bit nervous about getting data before PCSGA...we'll see what happens and if I need to present on *C. virginica* stuff instead.

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
