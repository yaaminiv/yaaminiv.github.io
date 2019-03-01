---
layout: post
comments: true
title: Sperm DNA Extractions Part 4
tags: virginica labwork sperm WGBS
---

## Finishing *C. virginica* sperm DNA extractions

Last day of sperm DNA extractions! Kaitlyn helped me isolate DNA from the [samples I incubated overnight](https://yaaminiv.github.io/Sperm-DNA-Extractions-Part3/) so we sped through the protocol.

### DNA Isolation

**Step 9**. Remove samples from the heat block and add 350 µL of choroform:isoamyl acholol (24:1) to each sample. Vortex the samples to mix. Set the heat block to 70ºC.
  - I removed the samples at 8:50 a.m., so the incubation ran for about 22 hours.

**Step 10**. Centrifuge 10,000 x g for 2 minutes at room temperature.
  - While centrifuging, I labelled additional tubes for Step 11 with the sample name and "Aq" to designate that this was the aqueous phase (ex. 48s Aq)
  
**Step 11**. Transfer the upper aqueous phase to a clean 1.5 mL microcentrifuge tube. Take note of the quantity transferred. Avoid the milky interface containing contaminants and inhibitors.
  - This was tricky for 57s and 63s. I sucked up some of the milky interface as I was pipetting and tried to not transfer it to the microcentrifuge tube, but some got in there.
  - Volume upper aqueous phase transferred:
    - 300 µL: 9s, 48s
    - 350 µL: 6s, 59s
    - 400 µL: 7s, 13s, 57s, 63s
    - 450 µL: 23s
  
**Step 12**. Add MBL Buffer in the same amount as the volume of the aqueous phase transferred in Step 11. Add 10 µL of RNase A to each sample, then vortex at maximum speed for 15 seconds.
  - Volume MBL Buffer added:
    - 300 µL: 9s, 48s
    - 350 µL: 6s, 59s
    - 400 µL: 7s, 13s, 57s, 63s
    - 450 µL: 23s
  - I FORGOT TO ADD RNASE TO MY SAMPLES. I didn't realize this until it was too late, so I hoped it wouldn't interfere with my DNA yields.
  
**Step 13**: Incubate the samples at 70ºC for 10 minutes on a heat block.

**Step 14**. Cool the sample to room temperature.
  - I let the samples sit in the tube rack for 10 minutes to reach room temperature.
  - I also labelled HiBind DNA Mini Columns with sample names while waiting for samples to cool.
  
**Step 15**. Add one volume 100% ethanol. Vortex at maximum speed for 15 seconds.
  - Volume MBL Buffer added:
    - 300 µL: 9s, 48s
    - 350 µL: 6s, 59s
    - 400 µL: 7s, 13s, 57s, 63s
    - 450 µL: 23s
  
**Step 16**. Insert a labelled HiBind DNA Mini column into a 2 mL Collection Tube. Transfer 750 µL of sample from Step 12 (including any precipitate) into the column.
   - There was no precipitate in any of my samples.
  
**Step 17**. Centrifuge at 10,000 x g for 1 minute. Discard the filtrate and place the column back in the collection tube. Using the same column, repeat Step 16 with until all of the sample has been applied to the spin column. 
  - I repeated Step 16 once more for all samples.
  
**Step 18**. Discard the collection tube. Place the spin column into a new collection tube and add 500 µL HBC Buffer to the column. Centrifuge at 10,000 x g for 30 seconds.

**Step 19**. Discard the filtrate and reuse the collection tube. Add 700 µL DNA Wash Buffer and centrifuge the samples at 10,000 x g for 1 minute. Repeat this step once more for a second DNA Wash Buffer wash step.

**Step 20**. Centrifuge the empty column at maximum speed for 2 minutes to dry the membrane.

**Step 21**. Place the spin column in a clean 1.5 mL microcentrifuge tube. Add 50-100 µL of the pre-heated Elution Buffer to the membrane and let it sit at room temperature for 5 minutes.
  - I added 50 µL of buffer to each sample to get a more concentrated sample.
  
**Step 22**. Centrifuge at 10,000 x g for 1 minute. Repeat Step 19 and 20 once more for a second elution step.
  - For the second elution, I used the eluate from the first elution instead of adding new elution buffer. I hoped this would increase my yield and concentration without changing my elution volume.

### Quantificiation

**Step 23**. Obtain dsDNA BR standards from the fridge.

**Step 24**. Prepare the master solution, using a 1:200 ratio of dye to dsDNA BR buffer. Each standard and sample needs 200 µL of solution.
  - I had two standards and nine samples, so I needed 2200 µL of solution.
  - Kaitlyn prepared 2220 µL of solution, using 2208.9 µL buffer and 11.1 µL dye (2220 µL solution * 0.5 / 100 = 11.1 µL dye; 2220 µL solution - 11.1 µL dye = 2208.9 µL buffer).

**Step 25**. Pipet 200 µL master solution into each Qubit assay tube.

**Step 26**. Add 10 µL of the correct standard to the standard assay tube. Add 5 µL of sample to the sample tube. Vortex the tubes for 2-3 seconds, then incubate at room temperature for 2 minutes.

**Step 27**. Use Qubit to quantify yield
  - When I tried quantifying 7s and 23s, it said the yield was too high and I had to dilute my sample! Kaitlyn and I prepared two more assay tubes using only 1 µL of sample and got a viable Qubit reading. The final volumne of these samples is 44 µL instead of 45 µL.

### Results

**Table 1**. Sample ID, concentration, and total DNA yield. Standard 1 read at 237.36, and Standard 2 at 22789.76.

| **Sample ID** | **DNA Concentration (ng/µL)** | **Final Volume (µL)** | **Total DNA Yield (ng)** |
|:-------------:|:-----------------------------:|:---------------------:|:------------------------:|
|   L18A0006s   |              113              |           45          |           5085           |
|   L18A0007s   |              576              |           44          |           25344          |
|   L18A0009s   |              552              |           45          |           24840          |
|   L18A0013s   |              208              |           45          |           9360           |
|   L18A0023s   |              416              |           44          |           18304          |
|   L18A0048s   |              35.6             |           45          |           1602           |
|   L18A0057s   |              312              |           45          |           14040          |
|   L18A0059s   |              95.2             |           45          |           4284           |
|   L18A0063s   |              173              |           45          |           7785           |

### Going forward

1. Prepare these samples for whole genome bisulfite sequencing!

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
