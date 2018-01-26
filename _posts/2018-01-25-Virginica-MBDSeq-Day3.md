---
layout: post
comments: true
title: Virginica MBDSeq Day 3
tags: virginica labwork
---

## Rinse and repeat

Today I got really good at pipetting supernatant from tubes as they were on the magnetic rack, then adding liquid to the beads and placing the tubes on the rotating mixer. Maybe it's because that's all I really did...........

### Step 0: Prepare for the day

- Made 10 mL of the 1X Bind/Wash buffer using the same ratio as [yesterday](https://yaaminiv.github.io/Virginica-MBDSeq-Day3/).
  - I used the same bottle of buffer I used yesterday and mixed everything. Then I realized there probably wasn't a full 2 mL in that bottle since the pipet tip had lots of bubbles. I discarded the buffer I made, and remade it using 5X Bind/Wash Buffer from a new bottle. I labelled this bottle with the date and the volume I took from it.
- Labelled six sets of 10 1.7 mL centrifuge tubes!
  - Sample Number + Wash 0 (ex. 31 W0)
  - Sample Number + Wash A (ex. 31 WA)
  - Sample Number + Wash B (ex. 31 WB)
  - Sample Number + Elution 1 (ex. 31 E1)
  - Sample Number + Elution 2 (ex. 31 E2)
  - Sample Number + Elution 3 (ex. 31 E3)
- Got ice
  - Sidenote: the new ice machine is so fancy

### Step 1: Remove non-captured DNA

- Removed tubes from the rotating mixer at 4ºC
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
    - During this step, I spilled some of sample 106 E1 as I was pipetting it from the "B" tube to the "E1" tube
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
  
I finished this stage at 11:05, and took a break until 12:05 to eat lunch. I covered the samples with extra ice to keep them cool.

### Step 3: Ethanol precipitation

Kaitlyn helped me with this step, which was nice since I had 60 tubes!

- Obtained glycogen from -20ºC freezer
- Added 1 µL glycogen to each tube (all W0, WA, WB, E1, E2 and E3 tubes)
  - I had sets W0, WB, and E2. Kaitlyn had the others
- Added 1/10th sample volume of pH 5.2 3 M sodium acetate (made by Sam in March 2007) to each sample
  - I measured the contents of the W0 tubes with a pipet, since those volumes were larger than the others (every other tube had a volume of 400 µL). The W0 samples were 700 µL
  - Added 70 µL of sodium acetate to W0 tubes, and 40 µL to all others (except for 31 WB, which I accidentally added 70 µL to instead of 40 µL)
- Added 2 sample volumes of 100% ethanol (200 proof) to each sample
  - Added 800 µL to all sets except W0, which needed 1400 µL. The W0 tubes were extremely full, so I had to transfer each W0 tube to a corresponding 2.0 mL centrifuge tube. While I was doing this, Kaitlyn took care of sets WB and E2
- Kaitlyn vortexed all sample centrifuge tubes while I put reagents away
- Placed all tubes in a box, then placed box in the -20ºC freezer

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
