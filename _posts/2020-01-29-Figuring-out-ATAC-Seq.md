---
layout: post
comments: true
title: Figuring Out ATAC-Seq
tags: ATAC-Seq
---

## Looking at labwork and sequencing options

Previously, I [hashed out](https://yaaminiv.github.io/Gigas-Labwork-Plans/) a plan that involved doing ATAC-Seq with *C. gigas* tissues. Shelly helped me get some answers on what ATAC-Seq entails from some experts, and put those resources in [this issue](https://github.com/RobertsLab/resources/issues/830). The consensus is that:

- frozen tissue is difficult to work with
- the protocol will need to be optimized several times for different tissue types
- it would be easier to optimize a protocol with live tissue

I shadowed Mac today when she worked on her embryo trial! Since there's a potential that I can do scATAC-Seq, I would need to learn how to dissociate cells. In talking to her, it seems like I'll need to isolate nuclei no matter what I end up doing, so that's a better method to look into first. Mac send me a nuclei isolation protocol that I'll review and figure out how to translate to frozen *C. gigas* tissues.

### Going forward

2. Determine what kits to use for RNA and DNA extractions and order necessary materials
2. Test RNA extraction protocol with tissue in histology blocks
3. Start processing frozen tissues
3. Extract DNA and RNA from larvae
4. Identify an ATAC-Seq protocol and start testing it
2. Figure out what to do with [*C. virginica* sperm](https://yaaminiv.github.io/Sperm-Extractions-Part5/) and other potential samples


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
