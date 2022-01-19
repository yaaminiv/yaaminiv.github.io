---
layout: post
comments: true
title: Coral Hypoxia RNA Extractions Part 4
tags: coral-hypoxia labwork RNA tagseq
---

## Increased lysis time

Another suggestion Maggie had was to increase the lysis time for the samples. Dr. Carly Kenkel has kept samples homogenized in lysate on ice for a period of time, with intermittent vortexing, to have more of the sample lyse. Maggie suggested I keep samples on ice for 30 minutes, and vortex the samples after 15 minutes, prior to centrifuging and removing the lysate.

### Extraction protocol

#### Preparing samples for extractions

1. Obtain coral sample tubes from the -80ºC freezer and place on ice. Label bead beating tubes, 2.0 mL centrifuge tubes, spin columns, column collection tubes, and 1.5 mL elution tubes for all samples. Sterilize tweezers in a 70% ethanol solution and dry with a kim wipe.
2. Fill a bead beating tube with glass beads up to the white line at the bottom of the tube, and add 700 µL of lysis solution.  
3. Using a sterilized dry tweezer, obtain a fragment of coral. The coral skeleton in this fragment should have plenty of visible tissue. Samples should be kept on ice when not in use.
  - I used either 3 or 4 corallites, not including the parts of the skeleton surrounding the corallites.
4. Place the bead beating tubes with lysis solution and coral fragments on a vortex. At high speed, bead beat the tubes for two minutes total over two one-minute intervals.
5. Keep samples on ice for 30 minutes. After 15 minutes, vortex samples for 15 seconds.
6. Centrifuge the samples in the bead beating tubes for 3 minutes at 16,500 rcf to precipitate the skeleton and glass beads. Transfer the lysate to a clean labelled 2.0 mL centrifuge tube. Samples should be kept on ice when not in use.
7. Add 700 µL of 60% ethanol to the lysate in the 2 mL tubes and pipet up and down to mix. Samples should be kept on ice when not in use.

#### Isolating RNA

1. Pipet 30 µL of elution solution per sample into a centrifuge tube. Place in a 70ºC water bath.
2. Pipet 600 µL of the lysate into the spin column. Centrifuge the sample for 60 seconds at 21,130 rcf and discard the flow-through. Repeat once more with the remaining lysate in the 2 mL sample tubes.
  - After adding the second round of lysate, all of the columns clogged. Maybe I'm sucking up some beads or coral skeletal fragments? *A. cervicornis* is also mucousy, which could clog columns.
  - Centrifuged all samples an extra minute
  - Added another 2 minutes for 6A-3C-30M and 6A-4C-30M (extra 3 total)
  - Added another 1 minutes for 6A-4C-30M (extra 4 total)
3. Add 700 µL of low stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
  - Added an extra minute to 6A-3C-30M and 6A-4C-30M
4. Obtain reconstituted DNase I aliquots from the freezer. After thawing, mix 5 µL of reconstituted DNAse I per sample + 1 and 75 µL of DNase dilution solution per sample + 1 in a separate centrifuge tube.
5. Place 80 µL of the reconstituted DNase I in DNAase dilution solution in each spin column. Incubate at room temperature for 25 minutes.
6. Add 700 µL of high stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
  - Added an extra minute to 6A-3C-30M and 6A-4C-30M
  - Added an extra 30 seconds to 6A-3C-30M (extra 1.5 total)
7. Repeat Step 3.
  - Added an extra 1 minute to 6A-3C-30M
8. Centrifuge samples for two minutes at 21,130 rcf to dry samples. Discard the flow-through.
9. Transfer the spin column to a clean labelled 1.5 µL centrifuge tube. Add 30 µL of elution solution to each spin column and incubate at room temperature for 1 minute. Centrifuge for 2 minutes at 21,130 rcf to elute RNA. Place eluted RNA on ice.

#### Quantifying RNA yield

1. Clean Nanodrop with a kim wipe
2. Pipet 1 µL of elution solution onto the Nanodrop to calibrate it. Avoid pipetting any air bubbles onto the device.
3. Pipet 1 µL of each sample onto the Nanodrop to quantify RNA yield. Avoid pipetting any air bubbles onto the device.
4. After quantification, store RNA at -80ºC.

### Results

**Table 1**. RNA quantification results from Nanodrop. All results can also be found in [this spreadsheet](https://github.com/yaaminiv/coral-hypoxia-omics/blob/main/metadata/Coral_Hypoxia_RNA_Yields.xlsx)

| **Sample** | **Yield (ng/µL)** | **A260/A280** | **A260/A230** |
|:----------:|:-----------------:|:-------------:|:-------------:|
|  6A-3C-30M |        71.4       |      2.05     |      1.42     |
|  6A-4C-30M |        21.1       |      1.97     |      0.39     |
|  4A-3C-3OM |        97.1       |      2.04     |      1.49     |
|  4A-4C-3OM |       106.9       |      2.03     |      1.62     |

The increased lysis time definitely helps! I'll keep that moving forward. It seems like four corallites may be too much for sample 6A, which has more tissue on it than 4A. I'm going to keep testing three and four corallites with some new samples to see if I can get a feel for what samples may need which input quantity.

### Going forward

1. Try RNA isolation on different *A. cervicornis* samples to see if I can consistently get good yields
4. Run a gel to validate RNA extraction quality for a subset of control samples
2. Reconstitute a fresh batch of DNase I
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
