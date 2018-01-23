---
layout: post
comments: true
title: PECAN Update 5
---

## It works!

Kind of. [Using the digested proteome](https://yaaminiv.github.io/PECAN-Update-4/) solved my problems of not having [valid MS2 data](https://genefish.wordpress.com/2017/03/04/pecan-on-roadrunner-isnt-working-correctly/). However, Sean reported that we've hit a new snag. It seems like the memory request argument isn't a global memory request, but specifies memory for each instance of `pecanpie`. Basically, the first instance takes up all of the memory and the following instances time out. Sean's suggestion is to rerun `pecanpie` and tell it to only look at three isolation windows concurrently as it makes its way down the isolation scheme.

In the meantime, I'm going to draft poster layouts and modify the [DIA wiki](https://github.com/RobertsLab/resources/blob/master/protocols/DIA-data-Analyses.md) and our [DNR Paper](https://docs.google.com/document/d/1giP16iXWPE7oDSNI7fyLV3p_1jqsXuuxlH7cJQAwhLM/edit#heading=h.7vvlns7jaib). Seriously hoping things work before I go out of town Wednesday!

![screen shot 2017-03-10 at 1 32 19 pm](https://cloud.githubusercontent.com/assets/22335838/23813952/29ebc15e-0596-11e7-9f2d-8d40d9849dd5.png)

**Figure 1**. #MOOD

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
