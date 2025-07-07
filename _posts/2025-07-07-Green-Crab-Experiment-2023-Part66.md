---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 66
tags: green-crab-wc RNA-library-preparation
---

## Continuing first round of library preparation

Just chugging along with library prep!

- Protocol can be found [here](https://docs.google.com/document/d/1KUGF7xg5rOeEQ883Pr_nJX1BmIbkS9pryi89AFXG9qM/edit?tab=t.0). Any updates that I think are necessary from my notes are then added to the protocol.
- All primer and cDNA concentration information can be found in [this spreadsheet](https://docs.google.com/spreadsheets/d/1B1tyeCI7F_T-l41144m6k_MEVhhU-XCHAEkr6PHoTpw/edit?gid=1215190646#gid=1215190646).

### 2025-07-07

#### Notes

- Samples: 5-123, 5-002, 25-170, 25-116, 5-008, 5-005, 5-061, 25-168
- Tagging primers: i7-7, i5-1 through i5-8
- When I was taking samples out of the -80 I LOST A SAMPLE. Literally dropped it as I was trying to pick up another sample. I searched high and low in the RNA room, but sample 5-064 is no where to be found. This sample was a CT from 5ºC sampled at week 6. Honestly, not too stressed about it since I don't need the genotype and there are 5 other samples from that timepoint. I refuse to re-extract at this point.
- Added a bit more than 5 µL of the fragmented RNA since samples 1-4 weren't pipetting well with the multichannel (I must not be rocking well)
- For the first bead dry, samples were dried for 2-2.5 minutes. The centrifuge really separated the pellets out, and the tops were starting to lighten and crack. The pellets themselves weren't as dry as desired but I didn't want to have the beads crack any more.
- Sample 5 was a little frozen after adding the end prep reagents and mixing
- Sample 3 was very foamy after mixing all of the PCR reagents
- Dried all samples for 2.5 minutes, except for sample 6 which I dried for 3 minutes during the second bead drying step
- Qubit S1 = 55.61, S2 = 25316.48

#### Results

I have really nice yields today!!

While I was working on library prep, I was chatting with Sara to figure out what to do about my libraries with really low concentrations. Given that I don't have BioAnalyzer reports for my libraries, Sara suggested I re-prep the libraries from scratch instead of trying to re-amplify them. So far, I need to re-prep the following libraries with concentrations < 5 ng/µL:

- i7-1: i5-1 through i5-8
- i7-2: i5-1, i5-2, i5-6
- i7-3: i5-1 through i5-8
- i7-5: i5-3, i5-4, i5-5, i5-6, i5-7
- i7-12: i5-1

I will have i7-11 i5-3 through i5-8 and i7-12 i5-6 through i5-8 leftover once I finish my first round of everything. This means I can redo all of my i7-1 and i7-12_i5-1 with these leftover indices. Today, I also noticed that there's an extra 5 µL of i7-7 after I finished all my samples. That means that I  have ~1 sample worth of all my finished i7 indices. However, I'll still need my i7-2, i7-3, and i7-5 indices for more than one sample, as well as extra i5-1 through i5-8. Since I need so many indices, I need to buy more tagging primers! I emailed NEB tech support to see if there is a way to get just what indices I need, instead of ordering the full 96 sample kit again.

I will also need more reagents to complete these libraries. I have 87 samples total, but I prepped 6 with reagents from the sample kit. That gives me 81 samples + 15 re-preps = 96 total samples that I would need reagents for. I mayyyy be able to eek out okay with what i have in the kit, but I asked Carolyn to order a sample PolyA module and library prep kit just in case, which will give me reagents for 6 more samples. I also asked Sara if the kit usually has extra reagents for a sample or two.

I did spill Tris and the reconstitution buffer. I should be fine on Tris, but I need to triple check my reconstitution buffer volume to ensure I have enough for the re-preps (or at the very least, to finish out this kit: 5 x 8 = 40 samples).

## 2025-07-08

### Notes

- Checked if I have enough reconstitution buffer for 40 samples (40 x 40 = 160 µL). I pulled out 1000 µL of reconstitution buffer, so I should be fine?

### Results

## 2025-07-09

### Notes

### Results

## 2025-07-10

### Notes

### Results

### Going forward

1. Prep all RNA libraries: i7-8, i7-9, i7-10, i7-11
2. Re-prep specific libraries: i7-1, i7-3, any library with concentration < 5 ng/µL
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
