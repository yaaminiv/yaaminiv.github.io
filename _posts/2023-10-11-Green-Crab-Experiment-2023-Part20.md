---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 19
tags: green-crab-wc chelex PCR gel
---

## Re-trying extractions, PCR, and gel

Okay, I've still got a contamination issue. So let's start from the beginning and see if I can run through the full protocol with more cleaning, new reagents, and working on the cleaner molecular bench.


### Methods

**Chelex Extraction**:

1. Use RNAse AWAY to clean bench space, pipets, tip boxes, etc.
2. Obtain samples in ethanol from the -80ºC freezer.
2. Obtain two sets of either an 8-strip PCR tube set or a 96-well plate. Label one set with sample ID, and another with sample ID and "S" (ex. 3 and 3S). Label another tube "Chelex."
3. Preheat the thermal cycler (protocol CHELEX) or set up a thermal block to 100ºC
4. Prepare a 10% Chelex solution (ex. 0.1 g Chelex beads in 1 mL of DEPC/nuclease-free water). Vortex thoroughly (10-15 seconds) and spin down briefly (5 secconds)
5. Add 70 µL of Chelex solution to each tube. Vortex Chelex solution for 10-15 seconds in between each sample tube since the Chelex beads settle quickly
6. Obtain and set up a flaming station and two pairs of tweezers. Ethanol and flame tweezers, then place on a clean kim wipe.
7. For each sample, use one tweezer to remove the leg joint from the sample tube with ethanol, and another pair of tweezers to remove the tissue from the leg. Be sure to avoid any exoskeletal pieces, as the chiton in the shell can inhibit the Chelex reaction. Place the tissue on a clean kim wipe and press to dab ethanol from the sample (which can also impede the Chelex reaction). After placing the blotted tissue in the sample tube, ethanol and flame the tweezers and **use RNAse AWAY to clean bench**.
8. Repeat step 7, **ensuring clean kim wipe is used to blot each sample after cleaning the bench with RNAse AWAY**.
9. If doing extractions in a plate, seal wells with caps.
10. Vortex samples for 10-15 seconds, and spin samples briefly (5-10 seconds).
11. Place samples in the thermal cycler and run the CHELEX protocol, or on a heat block for 20 minutes at 100ºC.
12. If doing extraction sin a plate, quickly spin down samples, remove caps, and add 50 µL DEPC water to each sample to assist with evaporation issues. Recover plate with a foil seal.
13. Vortex samples for 10-15 seconds, then spin down for 2 minutes.
14. Pipet ~50 µL supernatant into new, labelled tubes or a plate (labelled "S"). Avoid the Chelex beads when pipetting.
15. Place the supernatant with DNA in the dirty -20ºC freezer until ready for PCR.

**PCR**:

1. Use RNAse AWAY to clean bench space, pipets, tip boxes, etc.
2. Label either an 8-strip PCR tube or a 96-well plate with sample ID and "P" (ex. 3P) and a 1.5 mL eppendorf tube "PCR MM." Be sure to label an extra tube for the PCR blank.
3. Calculate the amount of reagents needed for PCR master mix for samples, a PCR blank, and two extra reactions.
  - GoTaq: 12.5 µL/sample
  - SMC F primer: 2.5 µL/sample
  - SMC R primer: 2.5 µL/sample
  - DEPC H<sub>2</sub>O: 5.5 µL/sample
4. Preheat the thermocycler with the TD65_48L PCR protocol
5. Get ice and thaw PCR reagents and DNA at room temperature. Briefly vortex, spin down, and move onto ice as soon as reagents thaw.
6. Make new SMC F and R primer aliquots. Take the 100 µM stock bottles (blue lids) and dilute into a new, labelled 1.5 mL eppendorf tube in a 1:10 dilution (ex. 10 µL primer stock, 90 µL NF water).
6. Make master mix based on calculations in step 3 in the labelled PCR MM tube. Vortex, spin down, and keep on ice.
7. Aliquot 23 µL of PCR MM from step 6 in each tube for the samples, extraction blank, and PCR blank.
8. Once the DNA thaws, briefly vortex DNA and spin down. Place on an ice block tube holder.
9. Add 2 µL of DNA into each sample tube. Add 2 µL DEPC water for the PCR blank.
10. Seal wells completely. Use tube/well caps, foil plate seal, or Microseal A film to seal plates. Use roller to push down seal and use the plate sealer tool to ensure all wells are completely sealed.
11. Vortex the sealed tubes and briefly spin down.
12. Place in the thermocycler, close lid, and run TD65_48L PCR protocol.
13. When protocol is complete, place PCR product in the 4ºC fridge until ready to load a gel.

**Gel**:

1. Obtain PCR product and TriTrak from the 4ºC fridge and move into gel room.
2. Microwave pre-made gel mix in the microwave for 3 minutes in 1.5 minute intervals. Be sure to swirl the bottle to mix the gel liquid.
3. Allow the bottle with gel mix to cool for a few minutes. While cooling, tape off sides of the gel tray.
3. After bottle has cooled enough to the touch but the gel mix is still in liquid form, pour the mix into the taped gel tray slowly to avoid bubbles. Use combs to make wells by slowly inserting them into the gel to avoid bubbles.
4. Allow gel to harden.
5. Once gel is hardened, place in gel box with 1x TAE buffer.
6. Obtain a piece of parafilm and pipet 1 µL of TriTrak dye for each sample, extraction blank, and PCR blank. On the parafilm, the dye will bead up. Place the dots far enough apart to avoid contamination.
7. Take 6 µL of PCR product and mix with a 1 µL dot of TriTrak dye by pipetting up and down at least 10 times. Then, pipet up 6 µL and load gel. Repeat for each sample until halfway through samples. Then, add 6 µL of ladder + dye mix, and continue with remaining samples.
8. Run gel for 30 minutes.
9. After running gel, remove gel tray and image.

### Notes

I ran out of the GoTaq master mix I used for [my non-contaminated PCR](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part19/), and I didn't want to use the master mix in Julia's box that I used for [my contaminated PCR](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part18/) in case that was the issue. So, I took some master mix from a barnacle box. There should be more master mix aliquots tomorrow.

### Results

<img width="656" alt="Screenshot 2023-10-11 at 2 47 19 PM" src="https://github.com/RobertsLab/resources/assets/22335838/cd3712f8-7d85-44f3-a459-b4b9cea7bf96">

**Figure 1**. Gel image for samples 3, 5, 10, 31, 32, 52, and 60 after re-trying extractions and PCR with clean reagents.

The extraction blank is still showing a band, but that band is MUCH ligher than the other samples. This means that the extra cleaning is definitely helping! Carolyn said that I could sequence these samples plus the blanks in a pinch. If the blank is showing a genotype, or if all of the samples are showing the same genotype, then that lowers our confidence in these samples. But at the very least, I can move on to another set of respirometry crabs for extractions.

We also talked about other potential contamination sources. We've eliminated the PCR reagents as a source of contamination. For PCR and Chelex extractions, I am using 10 µL and 200 µL pipets. Since those are common pipets and the PCR blank is clean, we know it's not those pipets. The only things unique to the Chelex extractions are the 1000 µL pipet and the Chelex beads themselves. Carolyn said she will take the 1000 µL pipet apart, clean it with RNAase AWAY, and stick it in the UV transilluminator to get rid of any potential contamination. I saved the Chelex solution I made last time. Additionally, we have another bottle of Chelex beads that I could use for extractions. I'm going to use the second bottle of Chelex beads for the next set of crabs, but also do PCR with a blank using beads from the second bottle, and another using the Chelex solution I used for this reaction. This will allow us to rule out the Chelex solution as a contamination source. Finally, Carolyn suggested spraying my dissection tools with RNAse AWAY after I ethanol and flame.

But hey, progress.

### Going forward

1. Determine contamination source for Chelex extractions
2. Continue with Chelex extractions, PCRs, and gels
2. Treatment-wise TTR analysis
3. Treatment-wise respirometry analysis
4. Get existing genotype data from Julia
5. Incorporate genotype into TTR and respirometry analyses
6. Prepare talk for PICES
8. Update methods and results of 2023 paper

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
