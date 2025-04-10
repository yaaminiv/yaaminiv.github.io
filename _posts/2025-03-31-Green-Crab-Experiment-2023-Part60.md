---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 60
tags: green-crab-wc RNA-extractions
---

## RNA extractions

It's time to extract RNA! For now I have pulled out all of the 2023 boxes I could find in the -80ºC and put them into a few racks. I still want to organize the samples such that all RNA samples are consolidated in a few boxes, and all metabolomics samples are also consolidated. For now, I can start the extractions.

### Methods

**Prepare for extractions**:
1. Thoroughly clean RNA room, bench, and hood. Clean any equipment that will be used.
2. Remove samples from the -20ºC and allow to thaw on the bench.
3. Label 1 low-adhesion tube for each sample and an additional tube for the extraction blank.
4. Pre-cool the centrifuge to 4ºC.

**Tissue processing**:

1. Remove crab heart from RNALater tube using an unfiltered pipet tip and place into a cleaned weigh boat. Cut a subsection of the tissue using a clean scalpel. Blot RNALater from tissue on clean kim wipes. Use the pipet tip to place the subsection into the labelled tube. Repeat for all samples.
2. Break up tissue samples using a disposable pestle. Once the texture is similar to wet sand, leave the pestle in the tube and proceed to the next sample.
3. In the hood, add 500 µL of TRIzol. Mix using the disposable pestle already in the tube. Remove the pestle and ensure no sample is on it before disposing in a Ziploc bag.
4. Add an additional 500 µL of TRIzol and pipet repeatedly to completely homogenize. Any remaining chunks of tissue should easily pass through the pipet tip.
5. Let sit 5 minutes at room temperature.

**Phase separation**:

1. Add 100 µl BCP and shake vigorously for 15 seconds.
2. Let sit for 2-15 min at room temperature.
3. Centrifuge 15 min at 4°C / 12,000 xg.
4. While centrifuge is running, remove the glycogen from the -20ºC to thaw. Label 1 low-adhesion tube for each sample and blank.
5. Transfer the upper, clear aqueous phase to a fresh tube (about 450 µL - 470 µL).

**Pelleting RNA**:

1. Add 1 µL glycogen (5-20 mg / mL) and pipet to mix.
2. Add 500 µl isopropanol and vortex gently.
3. Incubate samples at -20ºC overnight. Samples can be incubated for up to 7 days at -20ºC.

**Cleaning and resuspending RNA**:

1. Centrifuge 8 min at 4°C / 12,000 xg.
2. While samples are centrifuging, prepare just enough 75% ethanol for samples (2000 µL).
3. Remove and discard the supernatant, being careful not to disturb the pellet.
4. Add 1000 µL 75% ethanol and vortex to mix.
5. Centrifuge 5 min at 4°C / 7,500 xg.
6. Remove and discard the supernatant, being careful not to disturb the pellet.
7. Add 1000 µL 75% ethanol and vortex to mix.
8. Centrifuge 5 min at 4°C / 7,500 xg.
9. Remove and discard the supernatant, being careful not to disturb the pellet. For this final step, it helps to remove supernatant with the 1000 µL pipet, then go back in with a 10 or 20 µL pipet to remove the remainder of the liquid.
10. Air-dry the RNA pellet for 3 - 5 min. Do not let it dry completely.
11. While the pellet is air drying, aliquot RNAse-free water and place in a heat block set to 56ºC.
12. Add 50 µL RNAse-free water and resuspend pellet by pipetting.
13. Incubate for 15 minutes at 56°C to fully resuspend RNA.

**Quantify RNA**:

1. While samples are incubating, prepare for Qubit quantification. Label a Qubit tube for each sample and two standards.
2. Use the Qubit to calculate the amount of buffer and reagent to use for all samples, two standard, and an extra tube (overage).
3. Combine the RNA Broad Range buffer and reagent. Add 190 µL of combined reagent mix to each standard tube, and 198 µl of working stock to each sample tube.
4. Add 10 µL of Standard 1 to the correct Qubit tube. Repeat for Standard 2.
5. Remove RNA samples from heat block and vortex gently. Add 2 µL of RNA to the correct Qubit tube.
6. Vortex Qubit tubes and incubate at room temperature for 2 minutes.
7. Read standards, then samples, with Qubit.

### 2025-03-31

Carolyn is walking me through the extraction protocol since it is my first time! We got to work after thoroughly cleaning the RNA room.

#### Notes

- Started RNA extractions: 25-117, 25-058, 25-193, 5-124, 5-187
  - Dropped 5-124 on the clean bench. Added it to tube prior to homogenization.
  - Performed 5 minute incubation after BCP addition

### 2025-04-02

#### Notes

- Finished extractions and quantified: 25-117, 25-058, 25-193, 5-124, 5-187
  - Did centrifuge steps at 4ºC
  - Air dried samples for 3 minutes, then removed even more liquid from samples, and air dried again for 2 minutes
  - Qubit could not calibrate after I ran the standards! This means we need new buffer and reagent. I quantified the samples using the previously-read samples just to make sure I had something

#### Results

**Table 1**. RNA concentrations

| **Sample** | **µg/µL** | **ng/µL** |
|:----------:|:---------:|:---------:|
|   25-117   |   0.0892  |    89.2   |
|   25-058   |   0.0576  |    57.6   |
|   25-193   |   0.0371  |    37.1   |
|    5-124   |   0.0777  |    77.7   |
|    5-187   |   0.0782  |    78.2   |
|     EB     |  TOO LOW  |  TOO LOW  |

### Going forward

1. Find all RNA samples and move to -20ºC
3. Extract RNA
2. Find all metabolomics samples
2. Send samples for metabolomics
3. Try NEB UltraExpress Kit
4. Order library preparation materials
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
