---
layout: post
comments: true
title: Virginica Gonad DNA Extractions Part 11
tags: virginica labwork sperm WGBS
---

## Extracting DNA and RNA from sperm samples

Since [the protocol worked yesterday](https://yaaminiv.github.io/Virginica-Gonad-DNA-Extractions-Part10/), Grace and I extracted the remaining sperm samples today! TL;DR some samples will definitely need to be re-extracted for WGBS libraries, and I'm worried about not having enough RNA for RNA-Seq.

### Methods: Sample Preparation

#### Step 1: Prepare for extractions.

- Label 3 sets of tubes RNase-free centrifuge tubes per sample: one for the sperm sample, one for final RNA storage, and one for final DNA storage.
- Label spin columns
- Obtain samples from -80ºC freezer
- Added 96 mL of 100% ethanol to the DNA Wash Buffer

#### Step 2: Obtain 50 µL of sperm sample in a seawater solution and pipet into a labelled centrifuge tube.

- After bringing the sample to room temperature, I pipetted up and down to mix and took out 50 µL.

#### Step 3: Add 4 volumes of DNA/RNA Lysis Buffer and mix

- I added 200 µL of Lysis Buffer to each sample for a total volume of 250 µL
- Vortexed for 10 pulses

### Methods: Sample Purification

I also grabbed DNAse from the -20ºC freezer and put the nuclease free water on a heat block at 95ºC.

#### Step 4: Transfer the sample into a IC-MX spin column in a collection tube and centrifuge. 

- Kept the labelled spin columns for DNA extractions
- Saved the flow-through for RNA extractions

#### Step 5: For RNA only, add an equal volume of 95-100% ethanol to the flow-through and mix by pipetting. Transfer the flow-through into a new IC spin column in a clean collection tube. Centrifuge and discard the flow-through.

- Added 250 µL of EtOH and mixed

#### Step 6: For the RNA IC spin columns, perform a DNAse I treatment

- Grace did this! 
  - Added 400 µL of the DNA/RNA Wash Buffer to each column and centrifuged, then discarded the flow-through. 
  - Made a DNase REaction Mix with 5 µL of DNase I and 35 µL DNA Digestion Buffer for each sample. 
    - We were out of DNase I, so we got one from another Zymo Microprep kit, added water as directed, and proceeded with the treatment
  - Added the mix to the column matrix and incubated it at room temperature for 15 minutes before proceeding.

#### Step 7: Add 400 µL DNA/RNA Prep Buffer to the column. Centrifuge and discard the flow-through.

#### Step 8: Add 700 µL DNA/RNA Wash Buffer to the column. Centrifuge and discard the flow-through.

#### Step 9: Add 400 µL DNA/RNA Wash Buffer to the column. Centrifuge at 16,000 rcf for 2 minutes. Transfer the column to a new microcentrifuge tube.

#### Step 10: Add 15 µL DNase/RNase-Free Water to the column. Incubate at room temperature for 5 minutes. Centrifuge to elute the DNA and RNA.

- Added 15 µL of 95ºC nuclease-free water to the column
- Samples were centrifuged for 1 minute
  - After elution, DNA samples 13, 31, and 64 had more than 15 µL of DNA (21, 23, and 28 µL respectively). Grace noted that RNA sample 31 also had more than 15 µL of RNA, but did not note an exact volume. It's possible that some of the DNA/RNA Wash Buffer didn't filter through and eluted with the nuclease-free water 

### Methods: Sample Quantification

#### Step 11: Prepare the master solution using a 1:200 ratio of buffer to dye. For each sample and both standards, 200 µL of master solution is needed.

- RNA: Grace did this!
- DNA: Pre-mixed master solution for the Qubit dsDNA HS kit (aka my favorite thing)

#### Step 12: For each standard’s designated Qubit assay tube, add 10 µL of the correct standard and 190 µL of the master solution.

#### Step 13: For each sample’s designated Qubit assay tube, add 1 µL of the sample and 199 µL of the master solution. 

#### Step 14: Vortex all Qubit assay tube for 2-3 seconds at maximum speed. Incubate tubes at room temperature for two minutes.

#### Step 15: Use Qubit to quantify RNA and DNA yield in each sample tube.

- DNA: S1 = 76.01, S2 = 37294.08; RNA: S1 = 82.69, S2 = 1698.63
- Updated yields can be found [here](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-Adult-DNA-RNA-Extractions.csv)
- I need to get additional DNA from 57 and 64, and additional RNA from 57. It seems that for the sperm, I have at least 200 µL of RNA for each sample. We'll see if that's enough for library preparation

### Going forward

1. Talk to Zymo rep about input quantity needed for RNA-Seq
2. Re-extract male samples and check yields
5. Send DNA and RNA for library preparation and sequencing

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
