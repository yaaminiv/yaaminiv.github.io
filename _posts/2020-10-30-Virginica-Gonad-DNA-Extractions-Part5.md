---
layout: post
comments: true
title: Virginica Gonad DNA Extractions Part 5
tags: virginica labwork gonad WGBS
---

## Remaining female DNA and RNA extractions

Alright, time to finish up the female samples. Last night I set up the [samples I prepared Wednesday](https://yaaminiv.github.io/Virginica-Gonad-DNA-Extractions-Part4/) for the overnight incubation and lysis!

### Methods: Sample Preparation

#### Step 3: Added at least 150 µL of DNA/RNA shield (2X) and 150 µL nuclease-free water to create 300 µL DNA/RNA shield (1x) to each sample. If the sample is not covered by water, add more DNA/RNA shield (1x) until covered and record the volume of liquid added.

- At this step, I also vortexted the samples to help break them down before I added more reagents. I also homogenized the samples using the blue plastic pestles.

#### Step 4: For every 300 µL of sample, add 30 µL PK Digestion Buffer and 15 µL Proteinase K. Mix by vortexing gently.

#### Step 5: Place samples on a heat block at 55ºC overnight.

- I started the incubation at 4:50 p.m.

Today Grace and I extracted DNA and RNA from the prepared samples! I grabbed the DNase I from the -20ºC to bring to room temperature.

#### Step 6: Vortexed sample and centrifuge at maximum speed for 2 minutes to pellet debris. Transfer the aqueous supernatant to an RNase-free tube.

- I removed the samples from the heat block at 8:50 a.m. For the samples that were still in the white mailing tubes, I pipetted all of the sample and any remaining tissue into a 1.5 µL labelled centrifuge tube.
- I put the aliquot of nuclease-free water on the heat block 95ºC.
- To vortex, I did 10 pulses at maximum speed
- Samples were centrifuged at 21,130 rcf
- Grace and I accidentally got very, very small pieces of tissue in samples 22, 35, 16, and 39.

#### Step 7: Add an equivalent volume of DNA/RNA Lysis Buffer to each sample and mix by vortexing.

- Added 345 µL of DNA/RNA Lysis Buffer to each sample
  - I forgot to change the volume on my pipet after transferring samples to the RNase-free tubes! I noticed when pipetting my first sample (3). When I measured the final tube volume, it was a little less than my target volume of 690 µL, so I didn't add too much lysis buffer. I figured too little lysis buffer would be better and just decided to keep moving forward.

### Methods: Sample Purification

#### Step 8: Transfer the sample into a IC-MX spin column in a collection tube and centrifuge. 

- Samples 29 and 50 were centrifuged again since there was still some liquid in the column
- Kept the labelled spin columns for DNA extractions
- Saved the flow-through for RNA extractions

#### Step 9: For RNA only, add an equal volume of 95-100% ethanol to the flow-through and mix by pipetting. Transfer the flow-through into a new IC spin column in a clean collection tube. Centrifuge and discard the flow-through.

- Added 750 µL of EtOH and mixed. Had to centrifuge twice because the column could only hold 750 µL of liquid at once, so the original volume was greater than 750 µL

#### Step 10: For the RNA IC spin columns, perform a DNAse I treatment

- Grace took the lead on this! She added 400 µL of the DNA/RNA Wash Buffer to each column and centrifuged, then discarded the flow-through. Then, she made a DNase REaction Mix with 5 µL of DNase I and 35 µL DNA Digestion Buffer for each sample. She added the mix to the column matrix and incubated it at room temperature for 15 minutes before proceeding.
- Sample 50 had < 40 µL of DNase I treatment added (Grace said she miscalculated how much to make), but since it's an optional treatment I'm not worried.

#### Step 11: Add 400 µL DNA/RNA Prep Buffer to the column. Centrifuge and discard the flow-through.

#### Step 12: Add 700 µL DNA/RNA Wash Buffer to the column. Centrifuge and discard the flow-through.

#### Step 13: Add 400 µL DNA/RNA Wash Buffer to the column. Centrifuge at 16,000 rcf for 2 minutes. Transfer the column to a new microcentrifuge tube.

#### Step 14: Add 15 µL DNase/RNase-Free Water to the column. Incubate at room temperature for 5 minutes. Centrifuge to elute the DNA and RNA.

- Added 15 µL of 95ºC nuclease-free water to the column
- Samples were centrifuged for 1 minute

### Methods: Sample Quantification

#### Step 15: Prepare the master solution using a 1:200 ratio of buffer to dye. For each sample and both standards, 200 µL of master solution is needed.

- RNA: Grace made the master mix solution since she's done more RNA HS Qubit runs
- DNA: Pre-mixed master solution for the Qubit dsDNA HS kit (aka my favorite thing)

#### Step 16: For each standard’s designated Qubit assay tube, add 10 µL of the correct standard and 190 µL of the master solution.

#### Step 17: For each sample’s designated Qubit assay tube, add 1 µL of the sample and 199 µL of the master solution. 

#### Step 19: Vortex all Qubit assay tube for 2-3 seconds at maximum speed. Incubate tubes at room temperature for two minutes.

#### Step 20: Use Qubit to quantify RNA and DNA yield in each sample tube.

- Some DNA samples needed to be diluted to 1:10 (19, 29, 39, 41, 50). I misread my own notes/tube labels and prepared 1:10 dilutions for 44, 53, and 77 even though I didn't need them, so that changed my final volume.
- Except for 16, all RNA samples needed to be diluted!

### Results

Since I have a lot of data, I created [this spreadsheet](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-Adult-DNA-RNA-Extractions.csv) with DNA or RNA concentrations, dilution information, final sample volume after the Qubit assays, and total yield! All of my samples have enough for WGBS and RNA-Seq libraries except for 16. I checked my notes and there is still some tissue in the -80ºC, so I'll re-extract that sample. Next up, BioAnalyzer!

### Going forward

1. Re-extract DNA from 16
2. Check DNA and RNA quality on the BioAnalyzer
3. Test extraction protocol for male samples
4. Extract male samples and check quality
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
