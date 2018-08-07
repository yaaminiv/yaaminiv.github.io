---
layout: post
comments: true
title: Gigas Broodstock DNA Extraction Part 3
tags: manchester labwork gigas-broodstock
---

## Protocol test: Round 2

Here we go again! 

### Step 0: Prepare for extractions

- Preheat the thermomixer to 56ºC
- Clean scalpel in 10% bleach solution

### Step 1: Cut a tissue sample from the parrafin block

- I used Graham's scale, which was way more sensitive
- Tared the scale with a piece of weigh paper
- Used a scooping thing to cut and scoop out parrafin-embedded tissue
  - Today I carved out waaay less tissue than I did yesterday! I think I definitely had too much tissue yesterday, which could have caused the lower yield
- Set the tissue on the weigh paper
- Weighed 0.0201 g of tissue

### Step 2: Cut the block into smaller pieces and place them into a 2 mL round-bottomed processing tube

- Following the protocol's suggestions, I cut the block into much smaller pieces than I did yesterday

### Step 3: Add 1 mL xylene to the sample. Vortex vigorously for 20 s, and incubate for 3 min on the benchtop

### Step 4: Centrifuge at maximum speed for 3 minutes (but do not exceed 20,000 x g)

- Maximum speed on the centrifuge was 13,000 RPM x g

### Step 5: Remove the supernatant by pipetting. Do not remove any of the pellet

### Step 6: Add 1 mL of ethanol (96%-100%, purity grade p.a.) to the pellet, and mix by vortexing for 20 s.

### Step 7-8: Repeat Steps 4-5

### Step 9: Open the tube and incubate at room temperature for 10 minutes, or until all residual alcohol has evaporated

- I incubated the sample for 25 minutes, just like yesterday

### Step 10: Resuspend the pellet in 180 µL Buffer TD1.

### Step 11: Add one stainless steel bead to each 2 mL processing tube, and place the tubes in the TissueLyser Adapter Set 2 x 24.

- Added about 50 µL of glass beads (1 small spatula full) into the 2 mL sample tube

### Step 12: Operate the TissueLyser for 20 s at 15 Hz

- Instead, I vortexed the sample and glass beads at maximum speed for 25 s

### Step 13: Carefully pipet lysates into new 1.5 mL microcentrifuge-safelock tubes

- The protocol suggested that I vortex the lysates vigorously to improve yield, so I vortexed the lysates for 20 s

### Step 14: Add 20 µL proteinkinase K, and mix by pulse-vortexing for 15s

### Step 15: Incubate for 1 hour at 56ºC using a shaker-incubater at 1400 rpm.

- I used the Genome Sciences thermomixer, set to 56ºC and 1400 rpm, for one hour.
- After the incubation ended, I increased the temperature to 80ºC to prepare for Step 18

### Step 16: Briefly centrifuge the microcentrifuge-safelock tube to remove drops from the inside of the lid

- I centrifuged the tube for 15 s at 13,000 RPM x g
- There was no gelatinous pellet after incubation and centrifuging, so Proteinkinase K digestion did not go horribly

### Step 17: Add 4 µL RNase A (100 mg/mL), mix by vortexing, and incubate for 2 min at room temperature

- I added RNase A
- Vortexed for 20 s at maximum speed
- Incubated at room temperature for 2 minutes
- Incubated at room temperature an extra 4.25 min while I was waiting for the thermomixer to reach 80ºC

### Step 18: Incubate for 60 min at 80ºC at 1400 rpm

### Step 19: Repeat Step 16

### Step 20: Add 200 µL Buffer TD2, and mix by pulse-vortexing for 15 s

### Step 21: Add 200 µL ethanol (96-100%) to the sample, and mix thoroughly by vortexing

- I mixed for 35 s using a vortex at maximum speed

### Step 22: Repeat Step 16

### Step 23: Pipet the sample, including any precipitate that may have formed, into the PAXgene DNA spin column placed in a 2 mL processing tube, and centrifuge for 1 min at 6000 x g. Plae the spin column in a new 2 mL procesing tube, and discard the old processing tube containing flow-through.

### Step 24: Pipet 500 µL Buffer TD3 into the PAXgene DNA spin column, and centrifuge for 1 min at 6,000 x g. Place the spin column in a new 2 mL processing tube, and discard the old processing tube containing flow-through.

### Step 25: Pipet 500 µL Buffer TD4 into the PAXgene DNA spin column, and centrifuge for 1 min at 6,000 x g. Place the spin column in a new 2 mL processing tube, and discard the old processing tube containing flow-through.

### Step 26: Centrifuge for 3 min at maximum speed (but do not exceed 20,000 x g) to dry the membrane completely.

### Step 27: Discard the processing tube containing flow-through. Place PAXgene DNA spin column in a 1.5 mL microcentrifuge tube, and pipet 50-200 µL Buffer TD5 directly onto the PAXgene DNA spin column membrane. Centrifuge for 1 min at maximum speed (but do not exceed 20,000 x g) to elute the DNA.

- I pipetted 50 µL on the spin column membrane and let it incubate for 5 minutes, then centrifuged for one minute at 13,000 RPM x g

### Step 28: Assess DNA yield with the Qubit

- Obtain dsDNA BR standards from fridge
- Prepare the master solution, using a 1:200 ratio of dsDNA BR buffer to dye. The master solution is used for the two standards and the samples
  - Each sample needs 200 µL of master solution. Since I only had three tubes (2 standards, 1 sample), I needed 600 µL of solution
  - I prepared 660 µL of solution, using 656.7 µL buffer and 3.3 µL dye. 660 µL solution * 0.5 / 100 = 3.3 µL dye
- Pipet 200 µL master solution into each Qubit assay tube
- Add 10 µL of the correct standard to the standard assay tube
- Add 5 µL of sample to the sample tube
- Vortex the tubes for 2-3 seconds
- Incubate tubes at room temperature for 2 minutes
- Use Quibit to quantify yield
  - It's pretty easy to follow the instructions on the machine! You select your assay (dsDNA BR), read standards, then load in the samples

### Results

Somehow I ended up with a lower yield than last time...1.16 ng/µL. I need 6000 ng total per sample to do MBD-Seq, which means 130 ng/µL. I need 100x more DNA!
  
### Going forward

I posted another comment in [this issue](https://github.com/RobertsLab/resources/issues/277) asking for guidance. One possible solution could be to increase the time I vortex with glass beads. I'll try Thursday?

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
