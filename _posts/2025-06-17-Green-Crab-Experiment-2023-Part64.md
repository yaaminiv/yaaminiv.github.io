---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 64
tags: green-crab-wc RNA-library-preparation
---

## Continuing RNA library preparation

I'm back from 6 weeks of lovely vacation and library preparation is one of the few things in between me and a return to freedom.

- Protocol can be found [here](https://docs.google.com/document/d/1KUGF7xg5rOeEQ883Pr_nJX1BmIbkS9pryi89AFXG9qM/edit?tab=t.0). Any updates that I think are necessary from my notes are then added to the protocol.
- All primer and cDNA concentration information can be found in [this spreadsheet](https://docs.google.com/spreadsheets/d/1B1tyeCI7F_T-l41144m6k_MEVhhU-XCHAEkr6PHoTpw/edit?gid=1215190646#gid=1215190646).

### 2025-06-17

#### Notes

- Samples: 25-056, 25-194, 25-049, 25-203, 15-152, 25-167, 25-120, 15-091
- Tagging primers: i7-1, i5-1 through i5-8
- I don't know if I mixed up the wash buffer and the binding buffer when before the second denaturing, but I tossed my aliquots just in case and manually added in the volumes with a single use pipet
- Made a new wash buffer aliquot, will need to make a new RNA binding buffer aliquot tomorrow
- At the end of cDNA purification, I had less than 20 µL eluted for samples 1 and 8. I added TE buffer to make up the volume before proceeding.
- Forgot to keep NEBNext UltraExpress Ligation Master Mix in the freezer until ready to use, so I made a note in the protocol to not forget next time.
- Used 13 PCR cycles for PCR enrichment of adaptor ligated DNA based on yields from [my previous attempt](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part63/)
- I thought I accidentally kept the dilution buffer out and so I put it back way. I realized that I thought it was the TE and that's why I kept it out...to thaw........so I had to wait at the end of the PCR for the TE buffer to thaw so I could aliquot it out so I could then proceed with the cleanup.
- Removed 1.3 µL after phased bead cleanup for Qubit dsDNA HS assay.
- Qubit S1 = 52.69, S2 = 25789.96

#### Results

Welp my yields were SHIT! After talking to Sara, we decided that there were two potential issues:

1. The mixup between the binding and wash buffers
2. I wasn't letting the beads dry enough! That could lead to some ethanol carryover impeding the yields. Beads were drying for less than two minutes, but Sara said with this protocol she errs on the side of three minutes, especially in the summer when it is humid. the beads should be perfectly matte, which can be gauged by moving the rack back and forth.

## 2025-06-18

Today I am only going to do part of the protocol so I have some time to ICB revisions! I'll finish the protocol tomorrow and see how my yields are.

### Notes

- Samples: 25-056, 25-194, 25-049, 25-203, 15-152, 25-167, 25-120, 15-091
- Made only half of the ethanol for each sample (700 µL): 4480 µL EtOH, 1120 µL H<sub>2</sub>O
- Accidentally vortexed the dT25 bead stock (should only be inverted)
- Some beads were disturbed when I was removing excess supernatant prior to adding the 1X Fragmentation Master Mix to each sample
- Used a used pipet tip to get Strand Specificity Reagent for sample 3...but I noticed before I got any liquid! I don't think the tip touched the tube or any liquid.

## 2025-06-19

### Notes

- Samples: 25-056, 25-194, 25-049, 25-203, 15-152, 25-167, 25-120, 15-091
- Tagging primers: i7-2, i5-1 through i5-8
- Lost some of samples 6-8 while adding tagging primers. My glove got stuck in one of the tube caps when I closed it so I accidentally flung the strip tubes on the counter.
- Accidentally disturbed beans when trying to remove the TE and Reconstitution Buffer
- Dried beads for 3 minutes
- Qubit: S1 = 59.23, S2 = 26608.07

### Results

Yields this time were better! They were similar to my [first library prep run](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part63/). Sample 5 did have a much higher yield though, which was awesome. I sent my yields to Sara to see if I should increase the PCR cycles, or if I should just use the second repeat PCR protocol on these samples.

### Going forward

1. Prep all RNA libraries
2. Re-prep specific libraries if needed
3. Increase yields on specific libraries
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
