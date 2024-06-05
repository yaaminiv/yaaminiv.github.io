---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 8
tags: green-crab-cold restriction-digest PCR
---

## Testing the restriction digest

Sibel very graciously let me borrow the ToxLab Alul enzyme. This is a Promega enzyme. Digging into [Promega's protocol](https://www.promega.com/products/cloning-and-dna-markers/restriction-enzymes/alui/), it seems like most restriction enzymes can be added to the uncleaned PCR product then incubated! Alul wasn't one of the enzymes tested, so I messaged a Promega rep to ensure that I can actually get this to work. He seemed confident (even with my very expired loaner enzyme) that this could work, and even connected me with a sales rep so I could get a sample of the Alul enzyme in case the older enzyme was not sufficient.

### 2024-06-04

**PCR**:

1. IF STARTING HERE: Use RNAse AWAY to clean bench space, pipets, tip boxes, etc.
2. Label either an 8-strip PCR tube or a 96-well plate with sample ID and "P" (ex. 3P) and a 1.5 mL eppendorf tube "PCR MM." Be sure to label an extra tube for the PCR blank.
3. Calculate the amount of reagents needed for PCR master mix for samples, a PCR blank, and two extra reactions.
  - GoTaq: 12.5 µL/sample
  - F primer: 2.5 µL/sample
  - R primer: 2.5 µL/sample
  - DEPC H<sub>2</sub>O: 5.5 µL/sample
4. Preheat the thermocycler with the PCR protocol
5. Get ice and thaw PCR reagents and DNA at room temperature. Briefly vortex, spin down, and move onto ice as soon as reagents thaw.
6. *IF NEEDED*, make new F and R primer aliquots. Take the 100 µM stock bottles (blue lids) and dilute into a new, labelled 1.5 mL eppendorf tube in a 1:10 dilution (ex. 10 µL primer stock, 90 µL NF water).
6. Make master mix based on calculations in step 3 in the labelled PCR MM tube. Vortex, spin down, and keep on ice.
7. Aliquot 23 µL of PCR MM from step 6 in each tube for the samples, extraction blank, and PCR blank.
8. Once the DNA thaws, briefly vortex DNA and spin down. Place on an ice block tube holder.
9. Add 2 µL of DNA into each sample tube. Add 2 µL DEPC water for the PCR blank.
10. Seal wells completely. Use tube/well caps, foil plate seal, or Microseal A film to seal plates. Use roller to push down seal and use the plate sealer tool to ensure all wells are completely sealed.
11. Vortex the sealed tubes and briefly spin down.
12. Place in the thermocycler, close lid, and run PCR protocol.
13. When protocol is complete, immediately proceed to the restriction digest.

**Restriction Digest**:

1. Obtain Alul enzyme from the -20ºC. VERY gently vortex and place on ice.
2. Add 0.5 µL enzyme to each PCR reaction. Either pipet up and down to mix or VERY gently and briefly vortex
3. Briefly spin down
4. Place in thermocycler, close lid, and incubate at 37ºC for 2 hours
5. When incubation is finished, proceed to gel imaging.

**Gel**:

1. Obtain PCR + restrictoin digest product, ladder, and TriTrak from the 4ºC fridge and move into gel room.
2. Microwave pre-made gel mix in the microwave for 3 minutes in 1.5 minute intervals. Be sure to swirl the bottle to mix the gel liquid.
3. Allow the bottle with gel mix to cool for a few minutes. While cooling, tape off sides of the gel tray.
3. After bottle has cooled enough to the touch but the gel mix is still in liquid form, pour the mix into the taped gel tray slowly to avoid bubbles. Use combs to make wells by slowly inserting them into the gel to avoid bubbles.
4. Allow gel to harden for at least two hours.
5. Once gel is hardened, place in gel box with 1x TAE buffer. If an extra gel was made, slide and place in a Ziploc bag with TAE buffer. Place that Ziploc bag in the same drawer as the pre-made gel mix, away from the light.
6. Obtain a piece of parafilm and pipet 1 µL of TriTrak dye for each sample, extraction blank, and PCR blank. On the parafilm, the dye will bead up. Place the dots far enough apart to avoid contamination.
7. Take 6 µL of PCR product and mix with a 1 µL dot of TriTrak dye by pipetting up and down at least 10 times. Then, pipet up 6 µL and load gel. Repeat for each sample until halfway through samples. Then, add 3 µL of diluted ladder + dye mix, and continue with remaining samples.
8. Run gel for 30 minutes.
9. After running gel, remove gel tray and image.

### Notes

- Samples: 75, 188, 72, 151, 15, 194. Each sample was run in duplicate
- PCR Master Mix calculations
  - GoTaq: 12.5 µL x 15 = 187.15 (from 6/3)
  - Forward primer: 2.5 µL x 15 = 37.5 µL (from 6/3 and 6/4)
  - Reverse primer: 2.5 µL x 15 = 37.5 µL (from 6/3 and 6/4)
  - NF H<sub>2</sub>: 5.5 µL x 15 = 82.5 µL (from 4/16 and 6/4)

### Results

![Screenshot 2024-06-04 at 4 28 48 PM](https://github.com/yaaminiv/yaaminiv.github.io/assets/22335838/ee96481d-43ae-4dc1-a812-5549cdc1cdb5)

**Figure 1**. Gel image for 75, 188, 72, 151, ladder, 15, 194, and PCR blank

IT WORKED! All samples have a 213 band from the [second restriction enzyme cut site](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part6/). The CC homozygotes can be distinguished by a band complex at 301/389, which just looks like one band. The TT homozygotes have a band at 477 bp. And the heterozygotes have both the ~300 bp and 477 bp bands! As expected, the 88 bp band is indistinguishable from the primer-dimer junk. Carolyn suggested I run the gel out a bit longer to better separate the 300 and 477 bp bands from eachother. She also suggested I play around with restriction digest time. The digest looks very clean without any incomplete digestion after the two hour incubation. However, it's possible that I could get clean digestion with a shorter time, so I'll give that a try.

### 2024-06-05

### Notes

- Incubated for 30 min or 1 hour, and ran gel out immediately after each incubation for 35 minutes
- Samples: 75, 188, 72, 151, 15, 194
- PCR Master Mix calculations
  - GoTaq: 12.5 µL x 14 = 175 (from 6/3)
  - Forward primer: 2.5 µL x 14 = 35 µL (from 6/3 and 6/4)
  - Reverse primer: 2.5 µL x 14 = 35 µL (from 6/3 and 6/4)
  - NF H<sub>2</sub>: 5.5 µL x 14 = 77 µL (from 4/16 and 6/4)

### Results

![Screenshot 2024-06-05 at 3 52 39 PM](https://github.com/yaaminiv/yaaminiv.github.io/assets/22335838/2ca9556e-5712-4b26-a9ff-d46b3135b199)

**Figure 2**. Gel image for 75, 188, 72, 151, 15, 194 after 30 minutes, ladder, then, 75, 188, 72, 151, 15, and 194 after 1 hour

The 30 minute digestion looks a bit dicey, but the 60 minute digestion should be good moving forward! It doesn't look that much different than the 2 hour digestion. I have a working protocol!

### Going forward

1. Obtain MA crabs!
2. Genotype MA crabs
2. Develop heart rate protocol

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
