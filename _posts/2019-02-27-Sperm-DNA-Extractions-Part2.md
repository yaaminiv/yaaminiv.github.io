---
layout: post
comments: true
title: Sperm DNA Extractions Part 2
tags: virginica labwork sperm WGBS
---

## Isolating and quantifying *C. virginica* sperm DNA

[Yesterday](https://yaaminiv.github.io/Sperm-DNA-Extractions/) I prepared two *C. virginica* sperm samples for overnight lysis. Today I'll continue with the [E.Z.N.A. Mollusc Kit](https://github.com/RobertsLab/resources/blob/master/protocols/Commercial_Protocols/Omega_Mollusc-DNA-Kit-Combo-May-2013-D3373.pdf) to isolate DNA, then use the Qubit to quantify my yield. Hana, an undergraduate biology major, shadowed me while I did labwork! I'm hoping I can get her involved in helping with my labwork in the next few weeks.

### DNA Isolation

**Step 7**. Remove samples from the heat block and add 350 µL of choroform:isoamyl acholol (24:1) to each sample. Vortext the samples to mix. Set the heat block to 70ºC.

**Step 8**. Centrifuge 10,000 x g for 2 minutes at room temperature.
  - While centrifuging, I labelled two additional tubes for Step 9 with the sample name and "Aq" to designate that this was the aqueous phase (ex. 12s Aq)
  
**Step 9**. Transfer the upper aqueous phase to a clean 1.5 mL microcentrifuge tube. Take note of the quantity transferred. Avoid the milky interface containing contaminants and inhibitors.
  - The milky interface was really easy to spot! I forgot I had my phone with me so I didn't take any pictures, but I will next time.
  - Both samples had 350 µL of the upper aqueous phase
  
**Step 10**. Add MBL Buffer in the same amount as the volume of the aqueous phase transferred in Step 8. Add 10 µL of RNase A to each sample, then vortex at maximum speed for 15 seconds.
  - I added 350 µL MBL Buffer to each sample
  
**Step 11**: Incubate the samples at 70ºC for 10 minutes.
  - The original protocol specifies this should be done in a water bath, but Sam said it was fine if I just used a heat block.
  - The samples were left at room temperature for 13 minutes while I waited for the heat block to reach 70ºC
  
**Step 12**. Cool the sample to room temperature.
  - I let the samples sit in the tube rack for 10 minutes to reach room temperature.
  - While cooling, I pipetted 350 µL of the Elution Buffer into a 1.5 mL centrifuge tube, then placed it on the 70ºC heat block. I need the buffer at 70ºC for the final elution.
  - I also labelled HiBind DNA Mini Columns with sample names while waiting for samples to cool.
  
**Step 13**. Add one volume 100% ethanol. Vortex at maximum speed for 15 seconds.
  - I added 350 µL 100% ethanol to each sample.
  
**Step 14**. Insert a labelled HiBind DNA Mini column into a 2 mL Collection Tube. Transfer 750 µL of sample from Step 12 (including any precipitate) into the column.
  - I did not have any precipitate.
  
**Step 15**. Centrifuge at 10,000 x g for 1 minute. Discard the filtrate and place the column back in the collection tube. Repeat Step 14 and 15 with until al of the sample has been applied to the spin column.
  - I only needed to repeat these steps once more.
  
**Step 16**. Discard the collection tube. Place the spin column into a new collection tube and add 500 µL HBC Buffer to the column. Centrifuge at 10,000 x g for 30 seconds.

**Step 17**. Discard the filtrate and reuse the collection tube. Add 700 µL DNA Wash Buffer and centrifuge the samples at 10,000 x g for 1 minute. Repeat this step once more for a second DNA Wash Buffer wash step.

**Step 18**. Centrifuge the empty column at maximum speed for 2 minutes to dry the membrane.

**Step 19**. Place the spin column in a clean 1.5 mL microcentrifuge tube. Add 50-100 µL of the pre-heated Elution Buffer to the membrane and let it sit at room temperature for 5 minutes.
  - I added 50 µL of buffer to each sample to get a more concentrated sample.
  
**Step 20**. Centrifuge at 10,000 x g for 1 minute. Repeat Step 19 and 20 once more for a second elution step.
  - For the second elution, I used the eluate from the first elution instead of adding new elution buffer. I hoped this would increase my yield and concentration without changing my elution volume.

### Quantificiation

**Step 21**. Obtain dsDNA BR standards from the fridge.

**Step 22**. Prepare the master solution, using a 1:200 ratio of dye to dsDNA BR buffer. Each standard and sample needs 200 µL of solution.
  - I had two standards and two samples, so I needed 800 µL of solution.
  - I prepared 880 µL of solution, using 875.6 µL buffer and 4.4 µL dye (880 µL solution * 0.5 / 100 = 4.4 µL dye; 880 µL solution - 4.4 µL dye = 875.6 µL buffer).

**Step 23**. Pipet 200 µL master solution into each Qubit assay tube.

**Step 24**. Add 10 µL of the correct standard to the standard assay tube. Add 5 µL of sample to the sample tube. Vortex the tubes for 2-3 seconds, then incubate at room temperature for 2 minutes.

**Step 25**. Use Qubit to quantify yield

### Results

**Table 1**. Sample ID, concentration, and total DNA yield.

| **Sample** | **Concentration (ng/µL)** | **Total DNA Yield (ng in 45 µL total)** |
|:----------:|:-------------------------:|:---------------------------------------:|
|  L18A0012s |             125           |                   5625                  |
|  L18A0031s |            9.84           |                  442.8                  |

My two samples had very different concentrations! I think I was able to get enough DNA for whole genome bisulfite sequencing out of these samples. Since Sam isn't in today for me to confirm, I'll comment in [this issue](https://github.com/RobertsLab/resources/issues/587) and continue with labwork tomorrow.

### Going forward

1. Thursday: Pulverize remaining samples and start overnight incubation
2. Friday: Finish isolating DNA from all samples and quantify with the Qubit

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
