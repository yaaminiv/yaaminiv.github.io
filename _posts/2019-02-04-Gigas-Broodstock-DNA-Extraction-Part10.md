---
layout: post
comments: true
title: Gigas Broodstock DNA Extraction Part 10
tags: manchester labwork gigas-broodstock WGBS
---

## Preparing samples for WGBS

We're planning on sending samples for whole genome bisulfite sequencing, and Steven asked me prepare two pooled samples with  [*C. gigas* broodstock DNA I extracted](yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part9.md). These samples needed to have at least 500 ng of DNA and a concentration of 20 ng/µL. I decided to make 2 pooled samples: one for the low pH group and one for the ambient pH group. For each pooled sample, I used two stage 0 oyster samples (sexually undifferentiated) with the highest yield. This way, I could hopefully use the samples for sequencing again, and I'd have a baseline for DNA methylation without sex effects. Since each pooled sample needed 500 ng, I obtained 250 ng of DNA from each individual sample.

### Low pH sample (WGBS-YRVL 2/4)

**9-T2**: 15.9 ng/µL, 795 ng total in 50 µL. I used 15.8 µL of this sample.  
**10-T3**: 18.3 ng/µL, 915 ng total in 50 µL. I used 13.7 µL of this sample.

The total volume of this pooled sample was 29.5 µL, and the concentration was 17 ng/µL. This was too dilute for sequencing, so Shelly helped me do an [isopropanol precipitation](https://www.qiagen.com/us/resources/faq?id=5cf3006a-c63f-48d6-89d9-9ae14329c230&lang=en) to concentrate the sample, modified from [her protocol](https://genefish.wordpress.com/2019/01/31/shellys-notebook-tues-jan-30-2019/). I added 20.65 µL of isopropanol to the sample (70% of original sample's volume) and 2.95 µL 3M sodium acetate (10% of original sample's volume). While I added the reagents to the sample, Shelly placed the centrifuge in the 4ºC fridge. We spun the sample for 30 minutes at 12000 rpm at 4ºC. The DNA was pelleted out and was white because of the salts. I removed the supernatant and saved it in a different tube, just in case (YRVL sup 2/4). I then added 75% ethanol and spun the sample and ethanol for 10 minutes at 12000 rpm and 4ºC. After the ethanol wash, I once again remove and saved the supernatant (YRVL EW 2/4). I kept the sample on a heat block at 37ºC for 2 mintues to dry the sample. While the sample was on the heat block, I also warmed the elution buffer from the E.Z. DNA kit. I then added 15 µL of the elution buffer to my sample and used the Qubit to measure the concentration. My final sample concentration was 20.8 ng/µL.

### Ambient pH sample (WGBS-YRVA 2/4)

**11-T4**: 15.7 ng/µL, 785 ng total in 50 µL. I used 16.0 µL of this sample.  
**UK-01**: 63 ng/µL, 3150 ng total in 50 µL. I used 4.0 µL of this sample.

My final sample concentration was 25 ng/µL in 20 µL. I did not need to modify this sample.

I saved both samples in the fridge so we can send them off for sequencing when UW operations aren't affected by snow.

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
