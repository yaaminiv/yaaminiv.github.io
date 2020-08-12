---
layout: post
comments: true
title: WGBS Samples Part 7
tags: manchester labwork gigas-broodstock WGBS
---

## Final quality check

After talking to Steven, we decided that I would [drop samples 7 and U3]() since they barely had any DNA and attempt to sequence the remaining eight samples. That would give me a balanced design with four samples per treatment. In case my two samples that have less than 500 ng of DNA cannot be sequenced, I will have four treatment samples and two control samples. Since this is still an exploratory study and I don't know if there will be any effect, it may be okay to re-extract DNA. Definitely makes my lab life easier too.

The sequencing facility will do library preparation, so I just wanted to check DNA quality for the samples I precipitated. I created 1:10 dilutions of each sample with 1 µL of DNA and 9 µL of DNAse free water. I ran these dilutions on the BioAnalyzer [with some *C. virginica* samples](https://yaaminiv.github.io/Sperm-DNA-MBD-Enrichment-Day6/).

![Capture](https://user-images.githubusercontent.com/22335838/90073597-5bc0be80-dcae-11ea-9120-33aa4091a0e2.PNG)

![Capture2](https://user-images.githubusercontent.com/22335838/90073604-5d8a8200-dcae-11ea-95f9-5a119d67d93f.PNG)

**Figures 1-2**. BioAnalyzer results (bottom row).

I still have a decent amont of high molecular weight DNA! Sample U2 has a lower peak than the others, so that will be something to consider for library preparation. To determine if these samples will even be considered for library preparation, I emailed Genewiz and Zymo. Both facilities told me they could try their best with these samples, but the Zymo rep sounded way more confident about yield.

### Going forward

1. Send DNA for WGBS
2. Extract RNA from gonad samples

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
