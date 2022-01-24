---
layout: post
comments: true
title: Coral Hypoxia RNA Extractions Part 7
tags: coral-hypoxia labwork RNA tagseq
---

## Continuing extractions

TL;DR I flubbed these extractions but saved the RNA just in case I could salvage it with an RNA precipitation or clean-up.

### Extraction protocol

#### Preparing samples for extractions

1. Obtain coral sample tubes from the -80ºC freezer and place on ice. Label bead beating tubes, 2.0 mL centrifuge tubes, spin columns, column collection tubes, and 1.5 mL elution tubes for all samples. Sterilize tweezers in a 70% ethanol solution and dry with a kim wipe.
2. Fill a bead beating tube with glass beads up to the white line at the bottom of the tube, and add 700 µL of lysis solution.  
3. Using a sterilized dry tweezer, obtain a fragment of coral. The coral skeleton in this fragment should have plenty of visible tissue. Samples should be kept on ice when not in use.
  - I aimed for 4 corallites per sample
4. Place the bead beating tubes with lysis solution and coral fragments on a vortex. At high speed, bead beat the tubes for two minutes total over two one-minute intervals.
5. Keep samples on ice for 30 minutes. After 15 minutes, vortex samples for 15 seconds.
6. Centrifuge the samples in the bead beating tubes for 3 minutes at 16,500 rcf to precipitate the skeleton and glass beads. Transfer the lysate to a clean labelled 2.0 mL centrifuge tube. Samples should be kept on ice when not in use.
7. Add 700 µL of 60% ethanol to the lysate in the 2 mL tubes and pipet up and down to mix. Samples should be kept on ice when not in use.

#### Isolating RNA

1. Pipet 30 µL of elution solution per sample into a centrifuge tube. Place in a 70ºC water bath.
2. Pipet 600 µL of the lysate into the spin column. Centrifuge the sample for 60 seconds at 21,130 rcf and discard the flow-through. Repeat once more with the remaining lysate in the 2 mL sample tubes.
  - +1 minute 5C, 15A, 6D, 16D, 12B, 16B
  - +2 minutes 5C, 15A, 6D, 12B, 16B
  - +2 minutes 5C, 6D, 12B, 16B
  - +4 minutes 5C
3. Add 700 µL of low stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
  - Started with a 1 minute centrifuge for all samples
  - +2 minutes 5C, 16B
4. Obtain reconstituted DNase I aliquots from the freezer. After thawing, mix 5 µL of reconstituted DNAse I per sample + 1 and 75 µL of DNase dilution solution per sample + 1 in a separate centrifuge tube.
  - I messed this up and did a 375 µL dilution solution : 5 µL DNAse I solution. That's 5x too dilute! Yikes.
5. Place 80 µL of the reconstituted DNase I in DNAase dilution solution in each spin column. Incubate at room temperature for 25 minutes.
  - I added 80 µL to each column to start with. 10 minutes in, I realized I could add an extra 320 µL of solution to get the DNAse I to be the correct concentration. However, I ran out of solution when I got to sample 5C, so it did not get an extra 320 µL of solution.
  - I added an extra 10 minutes to the DNAse incubation time.
  - After the incubation, I centrigued all samples for 1 minute to drain the DNAse. Only 16B was slightly clogged after this centrifuging.
6. Add 700 µL of high stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
  - Started with a 1 minute centrifuge for all samples
  - +3 minutes for 5C and 16B
7. Repeat Step 3.
  - Started with a 1 minute centrifuge for all samples
  - +1 minute for 5C
8. Centrifuge samples for two minutes at 21,130 rcf to dry samples. Discard the flow-through.
9. Transfer the spin column to a clean labelled 1.5 µL centrifuge tube. Add 30 µL of elution solution to each spin column and incubate at room temperature for 1 minute. Centrifuge for 2 minutes at 21,130 rcf to elute RNA. Place eluted RNA on ice.

#### Quantifying RNA yield

1. Clean Nanodrop with a kim wipe
2. Pipet 1 µL of elution solution onto the Nanodrop to calibrate it. Avoid pipetting any air bubbles onto the device.
3. Pipet 1 µL of each sample onto the Nanodrop to quantify RNA yield. Avoid pipetting any air bubbles onto the device.
4. After quantification, store RNA at -80ºC.

### Results

**Table 1**. RNA quantification results from Nanodrop. All results can also be found in [this spreadsheet](https://github.com/yaaminiv/coral-hypoxia-omics/blob/main/metadata/Coral_Hypoxia_RNA_Yields.xlsx)

| **Sample** | **Oxygen** | **Temperature** | **Yield (ng/µL)** | **A260/A280** | **A260/A230** |
|:----------:|:----------:|:---------------:|:-----------------:|:-------------:|:-------------:|
|     22D    |   Ambient  |       29ºC      |        3.5        |      1.77     |      0.13     |
|     5C     |   Ambient  |      31.5ºC     |        21.2       |      1.89     |      0.95     |
|     15A    |     Low    |      31.5ºC     |        21.7       |      2.01     |      0.21     |
|     6D     |   Ambient  |       29ºC      |        14.5       |      1.94     |      1.12     |
|     16D    |   Ambient  |      31.5ºC     |        81.8       |      2.02     |      1.29     |
|     12B    |     Low    |       29ºC      |        8.4        |      1.88     |      0.38     |
|     16B    |   Ambient  |      31.5ºC     |        22.3       |      1.99     |      0.56     |
|     14D    |     Low    |       29ºC      |        1.2        |      5.27     |      0.02     |

Not great, but that's to be expected with genomic DNA contamination. I kept the samples anyways in case I can clean the sample up, but I'll definitely have to redo these.

### Going forward

1. Run a gel to validate RNA extraction quality with control *Nematostella* RNA
5. Work through *A. cervicornis* extractions
6. Work through *O. faveolata* extractions

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
