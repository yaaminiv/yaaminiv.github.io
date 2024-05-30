---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 4
tags: green-crab-cold restriction-digest
---

## Testing new primers

I got my primers back from IDT! I took DNA samples from [my May 14 extraction](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part52/) to use with our old SMC primers and my new primers. The goal is to examine product size and quality before proceeding.

**PCR**:

1. IF STARTING HERE: Use RNAse AWAY to clean bench space, pipets, tip boxes, etc.
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
7. Take 6 µL of PCR product and mix with a 1 µL dot of TriTrak dye by pipetting up and down at least 10 times. Then, pipet up 6 µL and load gel. Repeat for each sample until halfway through samples. Then, add 3 µL of diluted ladder + dye mix, and continue with remaining samples.
8. Run gel for 30 minutes.
9. After running gel, remove gel tray and image.

### Notes

- Samples: 172, 151, 170, 8, 47. I ran the same samples with both primer sets
- PCR Master Mix calculations
  - GoTaq: 12.5 µL x 8 = 100
  - Forward primer: 20 (from 4/16 and new batch made 5/30 for SMC, new batch for new primers made 5/30)
  - Reverse primer: 2.5 µL x 8 = 20 µL (from 4/16 and new batch made 5/30 for SMC, new batch for new primers made 5/30)
  - NF H<sub>2</sub>: 5.5 µL x 8 = 44 µL (from 4/22)

### Results

<img width="1087" alt="Screenshot 2024-05-30 at 3 01 15 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/a41f54f4-ad52-4953-b54f-dc629c1b246d">

**Figure 1**. Gel image for 171, 151, 170, 8, 47, PCR blank (with SMC primers), ladder, 171, 151, 170, 8, 47, PCR blank (with new primers)

Good news: no surprise intron-exon boundary! My product length is around 700 bp, which is what I expected. There is some laddering occurring, possibly because I used the TD65_48L protocol which isn't specific to this primer set. Carolyn suggested I try a gradient melting temperatures with one of the samples that laddered in order to determine the correct parameters for PCR for this primer set. But, seeing how the product is the length I want, I can look into restriction enzymes!

### Going forward

1. Check temperature of tanks
3. Identify candidate restriction enzymes for new primers
2. Tailor PCR protocol for new primers
3. Develop restriction band digest for genotyping
4. Develop heart rate protocol

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
