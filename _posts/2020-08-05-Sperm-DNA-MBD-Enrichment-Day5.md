---
layout: post
comments: true
title: Sperm DNA MBD Enrichment Day 4
tags: virginica labwork sperm
---

## Continuing shearing and quality checks

We got BioAnalyzer chips on Friday, so it's time to run more chips! I took the dilutions I [made previously (1:100 for sample 57, 1:20 for all other samples)](https://yaaminiv.github.io/Sperm-DNA-MBD-Enrichment-Day4/) and loaded them onto the chip. My run (thankfully) was successful!

![Capture](https://user-images.githubusercontent.com/22335838/89451482-2564cb80-d711-11ea-939c-16e7e7d1c592.PNG)

![Capture2](https://user-images.githubusercontent.com/22335838/89451485-25fd6200-d711-11ea-9691-19b6ee2a02a3.PNG)

**Figures 1-2**. Electropherogram and gel for resheared samples

Most of my samples have peaks around 300-400 bp, which is good for the MethylMiner kit. Samples 6 and 63 have peaks around 1000 bp, so I ran them for five cycles (30 seconds on, 30 seconds off). I wasn't sure about sample 12, which has a significant amount of DNA that's sheared correctly but also some DNA that's longer. I posted [this issue](https://github.com/RobertsLab/resources/issues/983) to get Sam's opinion about reshearing. I figured I'd hold off on running samples 63 and 6 on the BioAnalyzer until 1) I confirm if I need to shear more samples and 2) I conduct an [isopropanol precipitation with *C. gigas* gonad DNA samples](https://yaaminiv.github.io/WGBS-Samples-Part4/) so I can conserve BioAnalyzer chips.

### Going forward

1. Finish shearing and quality checks
2. Dilute samples to 25 ng/µL
3. Prepare reagents for MethylMiner kit (1X Buffer) and label tubes
4. Process *C. virginica* sperm samples in two batches with MethylMiner kit
5. Complete ethanol precipitation with all *C. virginica* sperm samples

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
