---
layout: post
comments: true
title: Full Skyline Preliminary Analysis
---

## I was gifted 3,000 extra proteins

When [error checking](https://yaaminiv.github.io/Skyline-Error-Checking-Round2/) the new Skyline document from the revised "desearlinated" .blib, I noticed that I had 9,047 proteins instead of about 6,000. Where did these 3,000 extra proteins come from? What do they do? To answer this question, I ran the new Skyline output through the [same preliminary pipeline](https://yaaminiv.github.io/Preliminary-Data-Analysis/) I used for my [NSA poster](https://yaaminiv.github.io/NSA-Poster/).

[Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/2017-06-13-Full-Skyline-Preliminary-Analysis.ipynb)

Videos of my narrating my findings quietly because I'm at my parents' house and they're asleep:

[Part 1](https://youtu.be/5dhA2xk3mKE)

[Part 2]()

Most interesting finding: miRNA inhibition of translation is a significant GO term for biological processes that's overrepresented across all of my samples!

![REVIGO](https://camo.githubusercontent.com/a8be34223564f2c250e177447f8832a1a8d511df/68747470733a2f2f757365722d696d616765732e67697468756275736572636f6e74656e742e636f6d2f32323333353833382f32373131363135342d36336530613261362d353038342d313165372d396338382d3261616632323336303833302e706e67)

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

