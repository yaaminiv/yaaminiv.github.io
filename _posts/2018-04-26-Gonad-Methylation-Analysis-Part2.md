---
layout: post
comments: true
title: Gonad Methylation Analysis Part 2
tags: virginica bismark MBDSeq
---

## Yaamini vs. Computers

[Plan](https://yaaminiv.github.io/Gonad-Methylation-Analysis/) in hand, I went to Roadrunner to start working on my pipeline. Before that, I created a [project-virginica-oa](https://github.com/RobertsLab/project-virginica-oa) repository, and a [notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-04-26-Gonad-Methylation-FastQC.ipynb) to detail my pipeline.

However, Roadrunner wasn't working! Sam had to reboot it a few times. After rebooting, I was unable to open Jupyter Notebook. Sam couldn't even run any anaconda commands! He rebooted again, but the screen wouldn't display anything. My struggles are documented in [this issue](https://github.com/RobertsLab/resources/issues/232). Steven pointed out that I should transition to the Mac mini in the conference room, so that's exactly what I did.

But the problems didn't stop there! In my notebook, I was unable to change the my working directory. In [this issue](https://github.com/RobertsLab/resources/issues/233), Sam and Steven pointed out that I need to use exclamation points before commands, and that I shouldn't comment in the code box itself. By the time I solved all my computer problems, I had to rush to the graduate student town hall. I ensured that I could connect to the Mac mini using the Mac Screen Sharing app, then left. Before I left SAFS, I checked once more that my computer could connect to the Mc mini so I could work from home tonight.

When I got home, my computer could no longer connect to the Mac mini! My guess is that the screen turned off, or someone else tried accessing the computer, preventing me from logging in. So much for starting on my analysis today. I'll focus on other things and return to this task (hopefully uninterrupted) this weekend.

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
