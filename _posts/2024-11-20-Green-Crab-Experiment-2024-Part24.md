---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 24
tags: green-crab-cold extractions PCR restriction-digest gel
---

## Returning to genotyping troubleshooting (Part 2)

We're still troubleshooting! [Last week](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part23/) it seemed like the next step was to try a new Chelex, and potentially DNEasy extractions. Here's my plan for the week:

- Extract samples using new Chelex sample
- Run Qubit with extracted samples
- Run PCR product on a gel
- If PCR product looks good, run a restriction digest
- If the new Chelex PCR product does not look good, try a DNEasy extraction then PCR

### Methods

**Chelex Extraction**:

1. Use RNAse AWAY to clean bench space, pipets, tip boxes, etc.
2. Obtain samples in ethanol from the -80ºC freezer.
2. Obtain two sets of either an 8-strip PCR tube set or a 96-well plate. Label one set with sample ID, and another with sample ID and "S" (ex. 3 and 3S). Label an eppendorf tube "Chelex."
3. Preheat the thermal cycler (protocol CHELEX) or set up a thermal block to 100ºC
4. Prepare a 10% Chelex solution (ex. 0.1 g Chelex beads in 1 mL of DEPC/nuclease-free water). Vortex thoroughly (10-15 seconds) and spin down briefly (5 secconds)
5. Add 70 µL of Chelex solution to each tube. Vortex Chelex solution for 10-15 seconds in between each sample tube since the Chelex beads settle quickly
6. Obtain and set up a flaming station and two pairs of tweezers. Ethanol and flame tweezers, then place on a clean kim wipe.
7. For each sample, use one tweezer to remove the leg joint from the sample tube with ethanol, and another pair of tweezers to remove the tissue from the leg. Be sure to avoid any exoskeletal pieces, as the chiton in the shell can inhibit the Chelex reaction. Place the tissue on a clean kim wipe and press to dab ethanol from the sample (which can also impede the Chelex reaction). After placing the blotted tissue in the sample tube, ethanol, flame, and toss the kim wipe. **Use RNAse AWAY to clean bench**.
8. Repeat step 7, **ensuring a clean kim wipe is used to blot each sample after cleaning the bench with RNAse AWAY**.
9. If doing extractions in a plate, seal wells with caps.
10. Vortex samples for 10-15 seconds, and spin samples briefly (5-10 seconds).
11. Place samples in the thermal cycler and run the CHELEX protocol, or on a heat block for 20 minutes at 100ºC.
12. If doing extraction sin a plate, quickly spin down samples, remove caps, and add 50 µL DEPC water to each sample to assist with evaporation issues. Recover plate with a foil seal.
13. Vortex samples for 10-15 seconds, then spin down for 2 minutes. Let tubes sit an additional minute or two, if possible, for Chelex beads to settle and make pipetting easier.
14. Pipet ~50 µL supernatant into new, labelled tubes or a plate (labelled "S"). Avoid the Chelex beads when pipetting.
15. Place the supernatant with DNA in the dirty -20ºC freezer until ready for PCR.

**DNEasy Extraction**:

1. Set the benchtop incubator to 56°C.
2. Wipe down the bench surface, pipettes, and racks with RNase Away.
3. Take out and label one 1.5 mL tube for each sample.
4. Add 180 µL Buffer ATL to each sample tube.
5. One at a time, cut a small amount of tissue from each sample (~1mm x 1mm) and place in the corresponding labeled 1.5mL tube. Make sure to ethanol and flame tools in between samples. If the specimen is stored in ethanol, make sure to blot off all ethanol before adding tissue to the labeled tube with Buffer ATL.
6. Add 20 µL Proteinase K to each sample. Mix samples by vortexing and quick-spin tube(s).
7. Place tube(s) in incubator at 56°C until tissue is completely lysed. Ideally, the incubation will be overnight. If opting for an incubation that isn't overnight, set an initial timer for 30 minutes and vortex tube(s) every 10 minutes. If tissue appears to not be fully dissolved after 30 minutes, continue to incubate samples and vortex every 15 minutes until tissue has dissolved.
8. After incubation and complete lysis, vortex sample(s) for 15 seconds and quick-spin tube(s).
9. Add 200 µL Buffer AL to each sample. Mix thoroughly by vortexing and quick-spin tube(s).
10. Incubate sample(s) at 56°C for 10 minutes.
11. While samples are incubating, take out and label one new 1.5 mL tube for each sample.
12. Add 200 µL 200 Proof Ethanol to each sample. Mix thoroughly by vortexing and quick-spin tube(s).
13. Pipet the entire sample volume (600 µL) into a labeled DNeasy Mini Spin column placed in a 2 mL collection tube (supplied). 14. Centrifuge for 1 minute at ≥6000 xg (8000 rpm). Discard the flow-through and collection tube.
15. Place the spin column in a new 2 mL collection tube (supplied). Add 500µl Buffer AW1 to each sample. Centrifuge for 1 minute at ≥6000 xg (8000 rpm). Discard the flow-through and collection tube.
16. Place the spin column in a new 2 mL collection tube (supplied). Add 500 µL Buffer AW2 to each sample and centrifuge for 3 minutes at 20,000 xg (14,000 rpm). Discard the flow-through and collection tube.
17. Transfer each spin column to the correct labeled 1.5 mL tube (not supplied).
18. Elute the DNA by adding 60 µL Buffer AE directly to the center of each spin column membrane. Dispense Buffer AE with pipette tip as close to the membrane as possible, without touching the membrane with the tip. It is strongly recommended to use a new tip for each sample.
19. Incubate for 1 minute at room temperature (15°C - 25°C). Centrifuge for 1 minute at ≥ 6000 xg (8000 rpm).
20. Take the eluted 60 µL and add it back to the membrane. Incubate at room temperature for 1 minute then centrifuge for 1 minute at ≥6000 xg (8000 rpm).


**PCR**:

1. IF STARTING HERE: Use RNAse AWAY to clean bench space, pipets, tip boxes, etc.
2. Label either an 8-strip PCR tube or a 96-well plate with sample ID and "P" (ex. 3P) and a 1.5 mL eppendorf tube "PCR MM." Be sure to label an extra tube for the PCR blank. Label two tubes for a known CC and a known TT crab.
3. Calculate the amount of reagents needed for PCR master mix for samples, a PCR blank, and two extra reactions.
  - GoTaq: 12.5 µL/sample
  - F primer: 2.5 µL/sample
  - R primer: 2.5 µL/sample
  - DEPC H<sub>2</sub>O: 5.5 µL/sample
4. Preheat the thermocycler with the SMC_60RD PCR protocol
5. Get ice and thaw PCR reagents and DNA at room temperature. Briefly vortex, spin down, and move onto ice as soon as reagents thaw.
6. *IF NEEDED*, make new F and R primer aliquots. Take the 100 µM stock bottles (blue lids) and dilute into a new, labelled 1.5 mL eppendorf tube in a 1:10 dilution (ex. 10 µL primer stock, 90 µL NF water).
6. Make master mix based on calculations in step 3 in the labelled PCR MM tube. Vortex, spin down, and keep on ice.
7. Aliquot 23 µL of PCR MM from step 6 in each tube for the samples, extraction blank, and PCR blank.
8. Once the DNA thaws, briefly vortex DNA and spin down. Place on an ice block tube holder.
9. Add 2 µL of DNA into each sample tube. Add 2 µL DEPC water for the PCR blank.
10. Seal wells completely. Use tube/well caps, foil plate seal, or Microseal A film to seal plates. Use roller to push down seal and use the plate sealer tool to ensure all wells are completely sealed.
11. Vortex the sealed tubes and briefly spin down.
12. Place in the thermocycler, close lid, and run the SMC_60RD PCR protocol.
13. When protocol is complete, either place product in the 4ºC or proceed to the restriction digest.

**Restriction Digest**:

1. Preheat the thermocycler with the SMCRD_IN incubation protocol
2. Obtain Alul enzyme from the -20ºC. VERY gently vortex and place on ice.
3. Add 0.5 µL enzyme to each PCR reaction. Either pipet up and down to mix or VERY gently and briefly vortex
4. Briefly spin down
5. Place in thermocycler, close lid, and run the SMCRD_IN protocol
6. When incubation is finished, either place product in the 4ºC or proceed to gel imaging.

**Gel**:

1. Obtain product for gel, ladder, and TriTrak from the 4ºC fridge and move into gel room.
2. Microwave pre-made gel mix in the microwave for 3 minutes in 1.5 minute intervals. Be sure to swirl the bottle to mix the gel liquid.
3. Allow the bottle with gel mix to cool for a few minutes. While cooling, tape off sides of the gel tray.
3. After bottle has cooled enough to the touch but the gel mix is still in liquid form, pour the mix into the taped gel tray slowly to avoid bubbles. Use combs to make wells by slowly inserting them into the gel to avoid bubbles.
4. Allow gel to harden for at least two hours.
5. Once gel is hardened, place in gel box with 1x TAE buffer. If an extra gel was made, slide and place in a Ziploc bag with TAE buffer. Place that Ziploc bag in the same drawer as the pre-made gel mix, away from the light.
6. Obtain a piece of parafilm and pipet 1 µL of TriTrak dye for each sample, extraction blank, and PCR blank. On the parafilm, the dye will bead up. Place the dots far enough apart to avoid contamination.
7. Take 10 µL of DNA, 6 µL of PCR product, or 10 µL of restriction digest product and mix with a 1 µL dot of TriTrak dye by pipetting up and down at least 10 times. Then, pipet up 6 µL and load gel. Repeat for each sample until halfway through samples. Then, add 3 µL of diluted ladder + dye mix, and continue with remaining samples.
8. Run gel for 40 minutes.
9. After running gel, remove gel tray and image.

### 2024-11-20

#### Notes

- Got a new Chelex 100 mesh size sample!
- Samples for extraction: 5, 19, 74, 3, 115, 99, 118
- Used 5 µL of DNA for a DNA gel
- Used 6 µL of PCR product for the PCR gel
- PCR Master Mix calculations
  - GoTaq: 12.5 µL x 11 = 137.5 µL
  - F: 2.5 µL x 11 = 37.5 µL
  - R: 2.5 µL x 11 = 37.5 µL
  - NF H<sub>2</sub>: 5.5 µL x 11 = 60.5 µL
- Used mix of Carolyn and my primers
- For DNEasy, samples were incubated started at 2 p.m. overnight

#### Results

**Table 1**. DNA concentrations from the Qubit 1x dsDNA kit. S1 = 52.16, S2 = 25091.01

| **Sample** | **Concentration (ng/µL)** |
|:----------:|:-------------------------:|
|      5     |            6.25           |
|     19     |            6.69           |
|     74     |            3.53           |
|      3     |            2.66           |
|     115    |            4.80           |
|     99     |            1.69           |
|     118    |            2.32           |

The concentrations with the new Chelex are higher than the concentrations for the same samples with the old Chelex! The DNA gel was way too overexposed when I tried to image it! Sara thought this might be because there was too much GelRed in the agarose mix, so that was a bust. From the little we saw though, the DNA didn't look great. I decided to just run a PCR gel.

![Screenshot 2024-11-21 at 10 52 56 AM](https://github.com/user-attachments/assets/09904163-8db3-4bbf-a568-a386fa84659c)

**Figure 1**. Gel with PCR product

Welp, this doesn't look much better than the other Chelex we have. At this point, Carolyn suggested I try the DNEasy protocol, since it would be a much cleaner extraction process. There's a chance that there are too many inhibitors in the Chelex output. Why that would be an issue now and not when I first developed this protocol baffles me, but I went ahead and extracted the same samples using the DNEasy protocol. I set up the samples around 2 p.m. for an overnight incubation at 56ºC.

### 2024-11-21

Today I finished up the DNEasy protocol, ran PCR, and ran the PCR product on a gel to see if that improved my PCR amplification!

#### Notes

- Stopped overnight incubation at 8:30 a.m.
- Used 60 µL of room temperature elution buffer
- Took the eluted DNA and passed it through the spin column a second time after a one minute incubation to improve yield
- PCR Master Mix calculations
  - GoTaq: 12.5 µL x 11 = 137.5 µL
  - F: 2.5 µL x 11 = 37.5 µL
  - R: 2.5 µL x 11 = 37.5 µL
  - NF H<sub>2</sub>: 5.5 µL x 11 = 60.5 µL
- Used only my old primer for the F primer, and a mix of my old and a newly made primer for the R primer. Made a new F primer while I was at it.
- Made new ladder: 5 µL Gene Ruler, 5 µL dye, 15 µL NF H<sub>2</sub>. Seems like dilutions should always be at least 3 parts water: 1 part ladder
- After gel imaging, I added 10 µL to the ruler, so the final quantities are 5 µL ruler, 5 µL dye, 25 µL NF H<sub>2</sub>.
- Added 5 µL of Alul to all sample tubes. Since the extraction and PCR blanks were blank from the PCR, I didn't want to waste restriction enzyme on them.

#### Results

![Screenshot 2024-11-21 at 3 45 55 PM](https://github.com/user-attachments/assets/5bb09d2a-3350-4621-9764-8dd6a3e77062)

**Figure 2**. PCR product gel from DNEasy extractions

WE HAVE BANDS!! They're not as bright as I would have expected, but I think that's because the ladder is extremely bright in comparison. I added an additional 10 µL to the ladder to attempt to dilute it out more. The next time I run the ladder I may add 2.5 µL instead just to see if that helps even out the brightness. Based on the bands, I started a restriction digest for the PCR product, which I will image tomorrow.

### 2024-11-22

Today I would run the gel with restriction digest product. Based on that, I could identify my next steps.

#### Notes

- Ran 10 µL of digest on the gel at 110 V for 40 minutes
- After the first restriction digest gel, I added 10 µL to the ruler, so the final quantities are 5 µL ruler, 5 µL dye, 35 µL NF H<sub>2</sub>.
- Redid PCR and the restriction digest to run 20 µL of product on the gel
- When redoing the PCR, I used the new F and R primers I made 11/21.
- When running the second gel, I accidentally mixed 10 µL of 74 with 10 µL of 3 in the third well. I only have 10 µL of 3 in the fourth well.
- Samples for extraction: 1, 6, 8, 11, 21, 25, 27, 100, 110, 113, 114

#### Results

<img width="625" alt="Screenshot 2024-11-22 at 11 54 32 AM" src="https://github.com/user-attachments/assets/4f2d3737-b6dd-444f-a4ee-77f9e7c4457b">

**Figure 3**. Restriction digest product gel from DNEasy extractions

These bands still aren't bright enough for me to genotype, except for 115! Samples 3, 99, and 118 are my unknowns, and they aren't clear enough for me to genotype. I figured I'd run the gel with 20 µL of product. Carolyn also suggested I run the product out at a lower voltage for much longer on a gel without a second row of wells to separate the bands a bit more.

<img width="545" alt="Screenshot 2024-11-22 at 4 07 22 PM" src="https://github.com/user-attachments/assets/6eeafbc5-9353-46d4-b29c-84df816f1a20">

**Figure 4**. Restriction digest product gel with 20 µL of product

I FINALLY HAVE CLEAR BANDS! Interestingly, the band for 3 is bright enough to read even though I only ran 3 µL of this new PCR product. My guess is that that running the gel low and slow allowed for less smearing so the band is crisper and can be read with 10 µL of product instead of 20 µL. I also ran this PCR with primers made on 11/21. Carolyn mentioned that some primers are less stable, so the next time I make a 10 µM working stock of primers to use elution buffer instead of water for increased stability. In any case, I have a workable protocol!

Instead of immediately scaling up to 23 samples and a blank (aka the maximum I could run in a centrifuge), I decided to try extracting 11 more samples that had no or unclear gel bands in my previous trials this summer. I set up the extraction for overnight incubation so I could finish the extraction and run PCR on Monday.

### Going forward

1. Troubleshoot genotyping
2. Clarify methods for average TTR analysis (reach out to Andy, Nic, or Megan?)
2. Individual-level TTR data analysis
3. Determine methods for comparing population responses
3. Troubleshoot lipid assay protocol
5. Conduct lipid assay for crabs of interest

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
