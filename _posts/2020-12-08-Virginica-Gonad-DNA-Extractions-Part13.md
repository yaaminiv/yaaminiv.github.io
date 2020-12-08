---
layout: post
comments: true
title: Virginica Gonad DNA Extractions Part 13
tags: virginica labwork sperm WGBS
---

## Final sperm sample extractions

Since saving cells in seawater can lead to lysis, there's a small chance that these sperm cells would be viable for ATAC-Seq. Instead of saving the tissue samples, I decided to do one last round of DNA and RNA extraction! My DNA yields are pretty good and another round of DNA extractions wouldn't hurt, so I was mainly focused on getting more RNA. Compared to the female samples, the sperm samples have lower RNA yields. This makes sense since eggs are filled with maternal RNA (and I had a lot more starting sample).

### Methods: Sample Preparation

#### Step 1: Prepare for extractions.

- Label 2 sets of tubes RNase-free centrifuge tubes per sample: one for final RNA storage, and one for final DNA storage.
- Label spin columns
- Obtain samples from -80ºC freezer
- Placed the nuclease-free water on a heat block at 95ºC
- Obtained DNase I from the -20ºC freezer

#### Step 2: Obtain 50 µL of sperm sample in a seawater solution and pipet into a labelled centrifuge tube.

- Since the shipping tubes had 50 µL of left, I didn't need to transfer it to a separate tube

#### Step 3: Add 4 volumes of DNA/RNA Lysis Buffer and mix

- I added 200 µL of Lysis Buffer to each sample for a total volume of 250 µL
- Vortexed for 10 pulses

### Methods: Sample Purification

#### Step 4: Transfer the sample into a IC-MX spin column in a collection tube and centrifuge. 

- These reagents are really soapy, so I learned that if I pipetted slowly I could avoid creating "soap" bubbles in the sample
- Kept the labelled spin columns for DNA extractions
- Saved the flow-through for RNA extractions

#### Step 5: For RNA only, add an equal volume of 95-100% ethanol to the flow-through and mix by pipetting. Transfer the flow-through into a new IC spin column in a clean collection tube. Centrifuge and discard the flow-through.

- Added 250 µL of EtOH and mixed

#### Step 6: For the RNA IC spin columns, perform a DNAse I treatment

- Added 400 µL of the DNA/RNA Wash Buffer to each column and centrifuged, then discarded the flow-through. 
  - At this point, I continued with Step 7 for the DNA samples
- Made a DNase REaction Mix with 5 µL of DNase I and 35 µL DNA Digestion Buffer for each sample. 
- Added the mix to the column matrix and incubated it at room temperature for 15 minutes before proceeding.
  - While the RNA samples were incubating, I finished Steps 8-10 with the DNA samples

#### Step 7: Add 400 µL DNA/RNA Prep Buffer to the column. Centrifuge and discard the flow-through.

#### Step 8: Add 700 µL DNA/RNA Wash Buffer to the column. Centrifuge and discard the flow-through.

#### Step 9: Add 400 µL DNA/RNA Wash Buffer to the column. Centrifuge at 16,000 rcf for 2 minutes. Transfer the column to a new microcentrifuge tube.

- Grace came into lab to help out, so she did Steps 9-10 with the RNA samples!

#### Step 10: Add 15 µL DNase/RNase-Free Water to the column. Incubate at room temperature for 5 minutes. Centrifuge to elute the DNA and RNA.

- Added 15 µL of 95ºC nuclease-free water to the column
- Samples were centrifuged for 1 minute

### Methods: Sample Quantification

#### Step 11: Prepare the master solution using a 1:200 ratio of buffer to dye. For each sample and both standards, 200 µL of master solution is needed.

- RNA: I made 1600 µL of master solution using 8 µL of dye and 1592 µL of buffer. This would suffice for 8 samples (2 extra sample volumes)
- DNA: Pre-mixed master solution for the Qubit dsDNA HS kit (aka my favorite thing)

#### Step 12: For each standard’s designated Qubit assay tube, add 10 µL of the correct standard and 190 µL of the master solution.

#### Step 13: For each sample’s designated Qubit assay tube, add 1 µL of the sample and 199 µL of the master solution. 

#### Step 14: Vortex all Qubit assay tube for 2-3 seconds at maximum speed. Incubate tubes at room temperature for two minutes.

#### Step 15: Use Qubit to quantify RNA and DNA yield in each sample tube.

- Updated yields can be found [here](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-Adult-DNA-RNA-Extractions.csv)
- After we quantified everything, we consolidated all sample tubes and reorganized the box in the -80ºC so there was only one DNA tube and one RNA for each sample. The final yields are in [this spreadsheet](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-Final-DNA-RNA-Yield.csv)
  - While consolidated, I discarded tubes that had no RNA yield: 57-1, 57-2, and 64-2.I have a sinking feeling I discarded 48-1 (a tube that had RNA in it) instead of 57-1...I'll check the box in the -80ºC and re-quantify yield tomorrow if needed.

Nothing to do now but wait for Zymo to get back to me and send the samples out!

### Going forward

1. Send DNA and RNA for library preparation and sequencing

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
