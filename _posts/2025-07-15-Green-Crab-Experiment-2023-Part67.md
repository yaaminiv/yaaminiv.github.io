---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 67
tags: green-crab-wc RNA-library-preparation
---

## Re-prepping low-yield libraries

- Protocol can be found [here](https://docs.google.com/document/d/1KUGF7xg5rOeEQ883Pr_nJX1BmIbkS9pryi89AFXG9qM/edit?tab=t.0). Any updates that I think are necessary from my notes are then added to the protocol.
- All primer and cDNA concentration information can be found in [this spreadsheet](https://docs.google.com/spreadsheets/d/1B1tyeCI7F_T-l41144m6k_MEVhhU-XCHAEkr6PHoTpw/edit?gid=1215190646#gid=1215190646).

### 2025-07-15

Today I'm redoing all of my [i7-3 libraries since they had yields < 5 ng/µL](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part65/). Figured it would be easier to start with one more round of 8 samples before piece-mealing things!

#### Notes

- Samples: 5-181, 15-033, 25-046, 25-050, 25-054, 25-115, 5-003, 5-131
- Tagging primers: i7-3, i5-1 through i5-8
- When diluting the RNA, I first added the sample volume amount of water to the empty tubes! I realized I needed to add more water, so I added the remaining necessary volume before adding the sample.
- Added 413 µL of RNA Binding Buffer since I couldn't add 412.5 µL
- There was less than 5 µL of fragmented library in sample 8. I added ~3-4 µL of Fragmentation Master Mix to make up the volume.
- Libraries sat for 1.5-2 hours in the thermocycler at 4ºC while we were filed by Farming Falmouth before I could finish up the cDNA preparation!
- Beads were dried for 1-2 minutes during the first bead drying step due to separation.
- Combined my i7-3 and i5 indices with the new kit! I had roughly 10-20 µL of index transfered to the new kit. I spilled some i5-2 while transferring, but I will have more than enough to finish out my samples.
- Got more reconstitution buffer from NEB!
- Samples were dried for 1.5-2 minutes during the second bead drying step. Sample 6 was dried for ~3.5 minutes since it was still super glossy.
- Tossed old i7-3 libraries out
- Qubit S1 = 55.73, S2 = 24589.09

#### Results

Results look great!

...I need to see if I can make it with enough beads. I think I'll be okay with the enzyme, since there's usually a little extra in each tube so I'll be able to pipet out the correct volume.

When chatting with Carolyn about pooling, she said I should probably send over pools with 4-8 samples per pool, and pool equal volumes of samples that have roughly the same qubit yields. This way, the facility can run BioAnalyzer/TapeStation and adjusts volumes as necessary for the final pool to run on the lane. I'll have ~11 pools and need to redo my Genewiz quote to match this! Once I have all my yields, I can organize which samples go into which pools.

## 2025-07-16

### Notes

- Samples: 15-154, 25-109
  - Tagging primers: i7-2, i5-1 through i5-2
- Samples: 15-161, 15-092, 25-202, 5-004
  - Tagging primers: i7-5, i5-3, i5-4, i5-7, and i5-8
- When making the 80% EtOH, I accidentally added 860 µL instead of 840 µL of water! Since I added 20 µL of water, I added 80 µL of EtOH to ensure my final mix was 80%
- Prepared 0.1 extra sample for dT25 beads. It worked out well!
- First bead dry was ~1.5 minutes
- Finished the enzyme mix from the 96 sample kit. Opened the enzyme mix from the sample kit for two samples. I should have exactly 4 samples left.
- Combined the old i7-2 and i7-5 indices with the new ones
- Second bead dry was ~1.5-2 minutes, since the beads were more separate than usual and the edges were starting to lighten
- When getting Qubit samples, the little drops for samples 1 and 5 were really annoying to pipet! I had to respin so they weren't on the sides where the beads were, and set them back on the magnetic stand for a few seconds. Thankfully none of the beads were leaching back into the eluate.
- When setting up the Qubit, I accidentally put sample 5 in the sample 6 Qubit tube! I made the correction to the tube labels immediately.
- Qubit S1 = 57.11, S2 = 23945.71

### Results

Yields are a little less than the past few days but very solid!

## 2025-07-22

### Notes

- Samples: 25-058, 25-120, 15-091, 5-014
  - Tagging primers: i7-2, i5-1 through i5-2
- I dried beads for ~1.5 minutes for the first and second bead drying steps
- I had JUST enough Enzyme Mix for all my samples!
- When doing the ethanol washes during the phased bead clean up, I somehow did not have enough ethanol for samples 1 and 2 during the second wash. They got between 150-200 µL of EtOH for the second wash step. Maybe because I was prepping such small quantities I was dealing with evaporation...?
- Qubit S1 = 55.88, S2 = 25253.11

### Results

Yields looked good! I'm not sure why 25-058 has a much lower yield than the other three, but it's still passable and that's what matters.

Now I just need to pool and send my libraries for sequencing!

### Going forward

1. Pool RNA libraries
3. Send RNA for sequencing!
4. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper
6. Start methods and results of 2023 paper

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
