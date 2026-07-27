---
layout: post
comments: true
title: Citrate Synthase Assay Development
tags: citrate-synthase
---

## Developing a citrate synthase assay protocol

Time to do something I haven't done in a while: learn a new lab protocol from scratch! Mikaela, Clara, and I are working on developing a citrate synthase assay protocol for the Abcam colormetric kit. Here's what's been done so far:

- Found my [old Github issue](https://github.com/RobertsLab/resources/discussions/1652) asking Olivia about the assay
  - In this issue, Olivia provided her protocol and some helpful hints
- Jasmine modified Olivia's protocol for clarity and put together a Bradford Assay protocol based on a tutorial from Pascale
- Mikaela and Clara created a kinetic measurement protocol on the plate reader based on the modified protocol

### 2026-07-27

Today our goals were to:

1. Create protein lysates
2. Conduct a Bradford Assay

When grabbing the reagents from the fridge, I noticed a discrepancy in our protocol. The kit reagents very clearly need to be stored at 4ºC, but the modified protocol said that reagents should be stored in the -20ºC. Additionally, there were different reagents listed in the protocol vs. what we received! I did some digging for the [actual kit protocol from Abcam](https://content.abcam.com/content/dam/abcam/product/documents/119/ab119692/citrate-synthase-activity-assay-kit-protocol-book-v3c-ab119692). We followed the directions from the manufacturer's protocol with some changes that Mikaela, Clara, and I will work on changing in the protocol itself:

**Protein lysate preparation**:

1. Remove flash-frozen tissue from the -80ºC and place immediately on ice
2. Cut and measure 10-20 mg of tissue per sample and place in a new tube
3. Add 350 µL of the Extraction Buffer into the tube with the issue. Homogenize the tissue using a pestle
4. Incubate homogenates on ice for 20 minutes.
5. Centrifuge homogenates for 20 minutes at 4ºC and 16,000 rcf
6. Transfer the supernatant into a new tube. Place in the -80ºC for 6 months or measure immediately in a Bradford assay

**Notes**:

- Each sample has roughly ~20-30 mg of tissue, so we aimed for 10 mg of tissue for this run to have something to work with in case we needed to redo anything.
- HOMOGENIZING TISSUE CONTINUES TO BE MY LEAST FAVORITE THING IN THE WORLD. Olivia's protocol said to use a p1000 to homogenize, so I figured that the sample could be pipet homogenized or mashed effectively with a pipet tip. This was.....not the case. I ended up getting some reusable glass pestles which worked a lot better
- The glass pestles displaced a lot of volume in the 1.5 mL tubes, so we transferred the tissue and buffer to a 5 mL tube to have more room for homogenization. Next time, we will only add 100 µL of buffer for homogenization, then top off with 250 µL of buffer to avoid using too many tubes.
- It took us 20-30 minutes between sectioning tissue to making homogenates. Samples were on ice or in the Extraction Buffer at all times, but I'm worried about how much protein is left in those samples.

We didn't get to the Bradford Assay today, but we will do it with Pascale tomorrow.

### Going forward

1. Quantify protein content with Bradford assay
2. Finalize protein lysate-making methods
3. Try controls and samples in full assay protocol
4. Troubleshoot protocol as needed

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
