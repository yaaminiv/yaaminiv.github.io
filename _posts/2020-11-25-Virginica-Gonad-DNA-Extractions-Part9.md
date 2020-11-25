---
layout: post
comments: true
title: Virginica Gonad DNA Extractions Part 9
tags: virginica labwork gonad WGBS
---

## Re-extracting female samples 16 and 35

Based on [my extraction yields](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-Adult-DNA-RNA-Extractions.csv), I don't have enough of sample 16 for WGBS. I also have under 500 ng of DNA forsamples 35 and 76. Even though Zymo accepts DNA above 400 ng for WGBS if you ask, I figured I could re-extract 16 and 35 since I had tissue left! I also decided to extract RNA as well even though I have more than enough for RNA-Seq.

### Methods: Sample Preparation

I did these steps last night:

#### Step 1: Prepare for extractions.

- Label 3 sets of tubes RNase-free centrifuge tubes per sample: one for digested sample, one for supernatant, one for final RNA storage, and one for final DNA storage.
- Obtain samples from -80ºC freezer

#### Step 2: Cut and weigh no more than 10-15 mg of frozen tissue. Record weight of tissue used in extractions and place tissue in a new, labelled test tube.

When I was processing samples [last time](https://yaaminiv.github.io/Virginica-Gonad-DNA-Extractions-Part4/), I eyeballed the remaining tissue and noted that I had between 10-15 mg left for a second extraction. I didn't double check by weighing (even though I probably should have).

#### Step 3: Added at least 150 µL of DNA/RNA shield (2X) and 150 µL nuclease-free water to create 300 µL DNA/RNA shield (1x) to each sample. If the sample is not covered by water, add more DNA/RNA shield (1x) until covered and record the volume of liquid added.

- At this step, I also vortexted the samples to help break them down before I added more reagents. I also homogenized the samples using the blue plastic pestles.

#### Step 4: For every 300 µL of sample, add 30 µL PK Digestion Buffer and 15 µL Proteinase K. Mix by vortexing gently.

#### Step 5: Place samples on a heat block at 55ºC overnight.

- I started the incubation at 3:35 p.m.

This is where I picked up today! I grabbed the DNase I from the -20ºC to bring to room temperature.

#### Step 6: Vortexed sample and centrifuge at maximum speed for 2 minutes to pellet debris. Transfer the aqueous supernatant to an RNase-free tube.

- I removed the samples from the heat block at 8:50 a.m. I pipetted all of the sample and any remaining tissue into a 1.5 µL labelled centrifuge tube.
- I put the aliquot of nuclease-free water on the heat block 95ºC.
- To vortex, I did 10 pulses at maximum speed
- Samples were centrifuged at 21,130 rcf

#### Step 7: Add an equivalent volume of DNA/RNA Lysis Buffer to each sample and mix by vortexing.

- Added 345 µL of DNA/RNA Lysis Buffer to each sample

### Methods: Sample Purification

#### Step 8: Transfer the sample into a IC-MX spin column in a collection tube and centrifuge. 

- Kept the labelled spin columns for DNA extractions
- Saved the flow-through for RNA extractions

#### Step 9: For RNA only, add an equal volume of 95-100% ethanol to the flow-through and mix by pipetting. Transfer the flow-through into a new IC spin column in a clean collection tube. Centrifuge and discard the flow-through.

- Added 790 µL of EtOH and mixed, instead of 750 µL like previous runs. Had to centrifuge twice because the column could only hold 750 µL of liquid at once, so the original volume was greater than 750 µL

#### Step 10: For the RNA IC spin columns, perform a DNAse I treatment

- Since I didn't have Grace helping me today, I did this myself! 
  - I added 400 µL of the DNA/RNA Wash Buffer to each column and centrifuged, then discarded the flow-through. 
    - At this step, I moved to Step 11 with my DNA samples
  - Made a DNase REaction Mix with 5 µL of DNase I and 35 µL DNA Digestion Buffer for each sample. 
  - Added the mix to the column matrix and incubated it at room temperature for 15 minutes before proceeding. At this point, I returned back to my DNA samples for Steps 12-Step 14

#### Step 11: Add 400 µL DNA/RNA Prep Buffer to the column. Centrifuge and discard the flow-through.

#### Step 12: Add 700 µL DNA/RNA Wash Buffer to the column. Centrifuge and discard the flow-through.

#### Step 13: Add 400 µL DNA/RNA Wash Buffer to the column. Centrifuge at 16,000 rcf for 2 minutes. Transfer the column to a new microcentrifuge tube.

#### Step 14: Add 15 µL DNase/RNase-Free Water to the column. Incubate at room temperature for 5 minutes. Centrifuge to elute the DNA and RNA.

- Added 15 µL of 95ºC nuclease-free water to the column
- Samples were centrifuged for 1 minute

### Methods: Sample Quantification

#### Step 15: Prepare the master solution using a 1:200 ratio of buffer to dye. For each sample and both standards, 200 µL of master solution is needed.

- RNA: Made working solution with 1393 µL of buffer and 7 µL of dye
- DNA: Pre-mixed master solution for the Qubit dsDNA HS kit (aka my favorite thing)

#### Step 16: For each standard’s designated Qubit assay tube, add 10 µL of the correct standard and 190 µL of the master solution.

#### Step 17: For each sample’s designated Qubit assay tube, add 1 µL of the sample and 199 µL of the master solution. 

#### Step 19: Vortex all Qubit assay tube for 2-3 seconds at maximum speed. Incubate tubes at room temperature for two minutes.

#### Step 20: Use Qubit to quantify RNA and DNA yield in each sample tube.

- DNA: S1 = 71.14, S2 = 36580.82; RNA: S1 = 84.51, S2 = 1917.60
- Updated yields can be found [here](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-Adult-DNA-RNA-Extractions.csv)
- Sample 35 needed to be diluted for quantification. I tried a 1:10 and 1:20 dilution, but they were too high. I created a 1:40 dilution but did not have enough working solution to run the sample on the Qubit, so I saved it to be quantified later.

### Going forward

3. Test extraction protocol for male samples
4. Extract male samples and check RNA quality
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
