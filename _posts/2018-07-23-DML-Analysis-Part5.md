---
layout: post
comments: true
title: DML Analysis Part 5
tags: virginica MBDSeq DML gene-enrichment
---

## Blasting for Uniprot codes

While I was gone, my [`blastx`](https://yaaminiv.github.io/DML-Analysis-Part2/) finished running!

![untitled](https://user-images.githubusercontent.com/22335838/43119340-e4f51ee6-8eca-11e8-907a-b31e74bc5f4c.png)

Now that I have these Uniprot codes, I can theoretically get Entrez Gene IDs. I also emailed Mike Riffle, who made the gene enrichment program for Emmas geoduck paper and Rhonda's data. Hopefully, he can make one for *C. virginica* that I can use instead of DAVID and `topGO`. In the meantime, Steven suggested I work on the gonad methylation paper, so that's what I'll do.

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
