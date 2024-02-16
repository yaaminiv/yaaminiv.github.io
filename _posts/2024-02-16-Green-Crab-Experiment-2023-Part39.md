---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 38
tags: green-crab-wc chelex PCR gel
---

## PCR, and gel for TTR crabs

Vanessa was out today, but I [ran the samples I extracted last week](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part38/) through PCR and gels!

### Methods

**PCR**:

1. IF STARTING HERE: Use RNAse AWAY to clean bench space, pipets, tip boxes, etc. Put pipets, tip boxes, and tube racks with open clean tubes into the UV Crosslinker.
2. Label either an 8-strip PCR tube or a 96-well plate with sample ID and "P" (ex. 3P) and a 1.5 mL eppendorf tube "PCR MM." Be sure to label an extra tube for the PCR blank.
3. Calculate the amount of reagents needed for PCR master mix for samples, a PCR blank, and two extra reactions.
  - GoTaq: 12.5 µL/sample
  - SMC F primer: 2.5 µL/sample
  - SMC R primer: 2.5 µL/sample
  - DEPC H<sub>2</sub>O: 5.5 µL/sample
4. Preheat the thermocycler with the TD65_48L PCR protocol
5. Get ice and thaw PCR reagents and DNA at room temperature. Briefly vortex, spin down, and move onto ice as soon as reagents thaw.
6. *IF NEEDED*, make new SMC F and R primer aliquots. Take the 100 µM stock bottles (blue lids) and dilute into a new, labelled 1.5 mL eppendorf tube in a 1:10 dilution (ex. 10 µL primer stock, 90 µL NF water).
6. Make master mix based on calculations in step 3 in the labelled PCR MM tube. Vortex, spin down, and keep on ice.
7. Aliquot 23 µL of PCR MM from step 6 in each tube for the samples, extraction blank, and PCR blank.
8. Once the DNA thaws, briefly vortex DNA and spin down. Place on an ice block tube holder.
9. Add 2 µL of DNA into each sample tube. Add 2 µL DEPC water for the PCR blank.
10. Seal wells completely. Use tube/well caps, foil plate seal, or Microseal A film to seal plates. Use roller to push down seal and use the plate sealer tool to ensure all wells are completely sealed.
11. Vortex the sealed tubes and briefly spin down.
12. Place in the thermocycler, close lid, and run TD65_48L PCR protocol.
13. When protocol is complete, place PCR product in the 4ºC fridge until ready to load a gel.

**Gel**:

1. Obtain PCR product, ladder, and TriTrak from the 4ºC fridge and move into gel room.
2. Microwave pre-made gel mix in the microwave for 3 minutes in 1.5 minute intervals. Be sure to swirl the bottle to mix the gel liquid.
3. Allow the bottle with gel mix to cool for a few minutes. While cooling, tape off sides of the gel tray.
3. After bottle has cooled enough to the touch but the gel mix is still in liquid form, pour the mix into the taped gel tray slowly to avoid bubbles. Use combs to make wells by slowly inserting them into the gel to avoid bubbles.
4. Allow gel to harden for at least two hours.
5. Once gel is hardened, place in gel box with 1x TAE buffer. If an extra gel was made, slide and place in a Ziploc bag with TAE buffer. Place that Ziploc bag in the same drawer as the pre-made gel mix, away from the light.
6. Obtain a piece of parafilm and pipet 1 µL of TriTrak dye for each sample, extraction blank, and PCR blank. On the parafilm, the dye will bead up. Place the dots far enough apart to avoid contamination.
7. Take 6 µL of PCR product and mix with a 1 µL dot of TriTrak dye by pipetting up and down at least 10 times. Then, pipet up 6 µL and load gel. Repeat for each sample until halfway through samples. Then, add 2.5 µL of diluted ladder + dye mix, and continue with remaining samples.
8. Run gel for 30 minutes.
9. After running gel, remove gel tray and image.

### Notes

- Samples extracted: 202, 200, 122, 119, 105, 69, 48, 40, 9, 181, 114
- PCR Master Mix calculations
  - 12 samples + 1 PCR blank + 1 extra = 14 aliquots
  - GoTaq: 12.5 µL x 15 = 137.5 µL
  - SMC F: 2.5 µL x 15 = 27.5 µL
  - SMC R: 2.5 µL x 15 = 27.5 µL
  - NF H<sub>2</sub>: 5.5 µL x 15 = 60.5 µL

### Results

<img width="627" alt="Screenshot 2024-02-16 at 1 55 28 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/fab0a0ee-d75a-4120-9d8e-bae575cd1aa5">

**Figure 1**. Gel image for 202, 200, 122, 119, 105, 69, 48, ladder, 40, 9, 181, 114, extraction blank, and PCR blank.

Still looking good! Lack of primer-dimer is not an issue (just means our PCR worked well). 181 and 114 may need re-extraction, but I think it makes sense to re-extract samples at the very end for any faint bands.

### Going forward

1. Work with Vanessa to continue with Chelex extractions, PCRs, and gels for TTR crabs
4. Re-extract and PCR samples with faint gel bands
4. Genotype remaining samples
4. Examine HOBO data from 2023 experiment
5. Determine best statistical approach for analyzing performance data
5. Demographic data analysis for 2023 paper
6. Update methods and results of 2023 paper

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
