---
layout: post
comments: true
title: Sperm DNA MBD Enrichment Day 10
tags: virginica labwork sperm MethylMiner
---

## DNA elution and ethanol precipitation (again)

Today I took the DNA samples bound to MBD-Biotin I placed on the rotating mixer [yesterday](https://yaaminiv.github.io/Sperm-DNA-MBD-Enrichment-Day9/), obtained the bound DNA, and started the ethanol precipitation process.

### Step 0: Prepare for the day

- Labelled six sets of 10 1.7 mL centrifuge tubes. I made sure each set was a different color so it would be easier to tell the samples apart when working with many tubes.
  - Sample Number + Wash 0 (ex. 6 W0)
  - Sample Number + Wash A (ex. 6 WA)
  - Sample Number + Wash B (ex. 6 WB)
  - Sample Number + Elution 1 (ex. 6 E1)
  - Sample Number + Elution 2 (ex. 6 E2)
  - Sample Number + Elution 3 (ex. 6 E3)
- Got ice

### Step 1: Remove non-captured DNA

- Removed tubes from the rotating mixer at 4ºC
  - I removed the samples at 8 a.m.
  - Batch 1 ran from 1 p.m.-8:30 a.m., and Batch 2 ran from 12:30 p.m.-8 a.m. Perfect timing!
- For each tube:
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "W0" tube, and placed "W0" tube on ice
  - Added 200 µL 1X Bind/Wash buffer to "B" tube
- For each tube:
  - Placed tube on rotating mixer for 3 minutes
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "WA" tube, and placed "WA" tube on ice
  - Added 200 µL 1X Bind/Wash buffer to "B" tube
  - Repeated entire process once more
- For each tube:
  - Placed tube on rotating mixer for 3 minutes
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "WB" tube, and placed "WB" tube on ice
  - Added 200 µL 1X Bind/Wash buffer to "B" tube
  - Repeated entire process once more, *EXCEPT* I added 400 µL of High Salt Elution Buffer to the "B" tube after pipetting the supernatant from the "B" tube and into the "WB" tube on the second round. I got the High Salt Elution Buffer from the 4ºC fridge

### Step 2: Elute captured DNA

- For each tube:
  - Placed tube on rotating mixer for 3 minutes
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "E1" tube, and placed "E1" tube on ice
  - Added 400 µL High Salt Elution Buffer to "B" tube
- For each tube:
  - Placed tube on rotating mixer for 3 minutes
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "E2" tube, and placed "E2" tube on ice
  - Added 400 µL High Salt Elution Buffer to "B" tube
- For each tube:
  - Placed tube on rotating mixer for 3 minutes
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "E3" tube, and placed "E3" tube on ice
  - Discarded "B" tubes
  
### Step 3: Ethanol precipitation

- Placed non-captured DNA and wash tube (W0, WA, and WB) in the -20ºC freezer in FTR 213
- To each elution tube (E1, E2, and E3)
  - Added 1 µL glycogen (from -20ºC freezer)
  - Added 1/10th sample volume of pH 5.2 3 M sodium acetate (made by Sam in March 2007) to each sample
    - Sample volume was 400 µL, so I added 40 µL
  - Added 2 sample volumes of 100% ethanol (200 proof) to each sample
    - Added 800 µL to all tubes
- Gently mixed all tubes by pipetting gently
- Placed tubes in the -20ºC freezer
  - Batch 1 was in the freezer from 8/13 to 8/18. I will keep Batch 2 in the freezer from 8/19 to 8/24 to avoid any batch effects.

### Going forward

1. Complete ethanol precipitation
2. Check methylated DNA concentrations
3. Send samples for sequencing

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
