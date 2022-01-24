---
layout: post
comments: true
title: Coral Hypoxia RNA Extractions Part 8
tags: coral-hypoxia labwork RNA tagseq
---

## Continuing extractions

TL;DR I flubbed these extractions too and put the RNA from today and Friday in a separate box. Here's hoping we're done with protocol flubs.

Previously, Maggie and I decided that the simplest sequencing design would be to extract two genotypes per species. We determined genotype E for *A. cervicornis* should definitely be extracted, along with one other genoytpe. While we aren't sure which other genotype to prioritize for sequencing, I decided to extract a few samples from genotype E.

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
  - Started with a 2 minute centrifuge for all samples
  - +1 minute 12E, 2E, 3E, 14E, 4E
  - +3 minutes 2E
3. Add 700 µL of low stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
4. Obtain reconstituted DNase I aliquots from the freezer. After thawing, mix 5 µL of reconstituted DNAse I per sample + 1 and 75 µL of DNase dilution solution per sample + 1 in a separate centrifuge tube.
5. Place 80 µL of the reconstituted DNase I in DNAase dilution solution in each spin column. Incubate at room temperature for 25 minutes.
6. Add 700 µL of high stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
  - +30 seconds 2E
7. Repeat Step 3.
8. Centrifuge samples for two minutes at 21,130 rcf to dry samples. Discard the flow-through.
 - I FORGOT TO DO THIS STEP PRIOR TO ELUTION. This means I eluted RNA, but with a decent volume of contaminants.
9. Transfer the spin column to a clean labelled 1.5 µL centrifuge tube. Add 30 µL of elution solution to each spin column and incubate at room temperature for 1 minute. Centrifuge for 2 minutes at 21,130 rcf to elute RNA. Place eluted RNA on ice.

#### Quantifying RNA yield

1. Clean Nanodrop with a kim wipe
2. Pipet 1 µL of elution solution onto the Nanodrop to calibrate it. Avoid pipetting any air bubbles onto the device.
3. Pipet 1 µL of each sample onto the Nanodrop to quantify RNA yield. Avoid pipetting any air bubbles onto the device.
4. After quantification, store RNA at -80ºC.

### Results

**Table 1**. RNA quantification results from Nanodrop. All results can also be found in [this spreadsheet](https://github.com/yaaminiv/coral-hypoxia-omics/blob/main/metadata/Coral_Hypoxia_RNA_Yields.xlsx)

| **Sample** | **Oxygen** | **Temperature** | **Volume (µL)** | **Yield (ng/µL)** | **A260/A280** | **A260/A230** |
|:----------:|:----------:|:---------------:|:---------------:|:-----------------:|:-------------:|:-------------:|
|     19E    |     Low    |      31.5ºC     |       120       |        3.3        |      2.01     |      0.03     |
|     8E     |     Low    |       29ºC      |        80       |        28.9       |      1.44     |      0.12     |
|     12E    |     Low    |       29ºC      |       100       |        15.2       |      1.69     |      0.09     |
|     2E     |   Ambient  |       29ºC      |        80       |        13.7       |      1.55     |      0.16     |
|     3E     |     Low    |       29ºC      |       120       |        13.2       |      1.55     |      0.10     |
|     14E    |     Low    |       29ºC      |       120       |        3.4        |      1.76     |      0.03     |
|     22E    |     Low    |       29ºC      |       140       |        5.8        |      1.67     |      0.03     |
|     4E     |     Low    |      31.5ºC     |       130       |        26.6       |      1.41     |      0.18     |

Welp. A260/A230 values are indicative of the high level of washes and other potential contaminants. There's also a likelihood there's leftover DNAse solution in these samples since I didn't dry out the columns. I put this in the box with my [samples from Friday](https://yaaminiv.github.io/Coral-Hypoxia-RNA-Extractions-Part7/) for potential salvaging, but I'll add these to the list of samples to re-extract.

Moving forward with the rest of the extractions, what I need is a strategy to decide which samples to extract first. I think I'll continue extracting samples from genotype E, but I'd like to know which other *A. cervicornis* genotype to prioritize so I can extract them at the same time and avoid any genotype-based batch effects. If I can get usable, good-quality RNA from those samples first, I can focus on extracting other low-priority samples, or move on to extracting the priority *O. faveolata* samples.

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
