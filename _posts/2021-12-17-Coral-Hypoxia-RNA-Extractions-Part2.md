---
layout: post
comments: true
title: Coral Hypoxia RNA Extractions Part 2
tags: coral-hypoxia labwork RNA tagseq
---

## Testing the TagSeq RNA extraction protocol (again)

Based on [my last extraction test](https://yaaminiv.github.io/Coral-Hypoxia-DNA-Extractions/), a good place to start optimizing the extractions is to use more starting material. I was using a coral fragment roughly half the size of a pinkie nail based on Maggie and Ann's previous work. Today, I tried extractions with additional starting material from sample 29A (tested last time) and 6A and 2A (two new samples from ambient conditions with plenty of tissue on the skeleton).

### Extraction protocol

#### Preparing samples for extractions

1. Obtain coral sample tubes from the -80ºC freezer and place on ice. Label bead beating tubes, 2.0 mL centrifuge tubes, spin columns, column collection tubes, and 1.5 mL elution tubes for all samples. Sterilize tweezers in a 70% ethanol solution and dry with a kim wipe.
2. Fill a bead beating tube with glass beads up to the white line at the bottom of the tube, and add 700 µL of lysis solution.  
3. Using a sterilized dry tweezer, obtain a fragment of coral. The coral skeleton in this fragment should have plenty of visible tissue. Samples should be kept on ice when not in use.
  - For these extractions, I used coral fragments that were the size of my pinkie nail (not just the white part, but my full actual nail)
4. Place the bead beating tubes with lysis solution and coral fragments on a vortex. At high speed, bead beat the tubes for two minutes total over two one-minute intervals.
  - I bead beat samples 6A and 2A for an additional minute since I saw some tissue still left on the coral skeletons
5. Centrifuge the samples in the bead beating tubes for 3 minutes at 16,500 rcf to precipitate the skeleton and glass beads. Transfer the lysate to a clean labelled 2.0 mL centrifuge tube. Samples should be kept on ice when not in use.
6. Add 700 µL of 60% ethanol to the lysate in the 2 mL tubes and pipet up and down to mix. Samples should be kept on ice when not in use.

#### Isolating RNA

0. Pipet 30 µL of elution solution per sample into a centrifuge tube. Place in a 70ºC water bath.
1. Pipet 600 µL of the lysate into the spin column. Centrifuge the sample for 60 seconds at 21,130 rcf and discard the flow-through. Repeat once more with the remaining lysate in the 2 mL sample tubes.
  - The spin columns are clogged: not all of the liquid made it through the column. I repeated step 2 for all samples, and repeated it a third time for samples 6A and 2A. My guess is that there was too much starting material for those samples (sample 29A has way less starting tissue than 6A and 2A)
2. Add 700 µL of low stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
  - All samples were centrifuged for 60 seconds. Sample 6A was centrifuged for an additional 60 seconds, for 120 seconds total
3. Obtain reconstituted DNase I aliquots from the freezer. After thawing, mix 5 µL of reconstituted DNAse I per sample and 75 µL of DNase dilution solution sample in a separate centrifuge tube.
4. Place 80 µL of the reconstituted DNase I in DNAase dilution solution in each spin column. Incubate at room temperature for 25 minutes.
5. Add 700 µL of high stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
  - In line with the columns being clogged, all of the DNaseI didn't permeate the spin columns. I didn't want to risk the DNaseI degrading the RNA with a longer incubation, so I added the high stringency wash to whatever liquid was in the column
  - Samples 29A and 2A were centrifuged for 60 seconds, and 6A was centrifuged for 120 seconds
6. Repeat Step 2.
  - All samples were centrifuged for 60 seconds
7. Centrifuge samples for two minutes at 21,130 rcf to dry samples. Discard the flow-through.
8. Transfer the spin column to a clean labelled 1.5 µL centrifuge tube. Add 30 µL of elution solution to each spin column and incubate at room temperature for 1 minute. Centrifuge for 2 minutes at 21,130 rcf to elute RNA. Place eluted RNA on ice.
  - Samples had similar elution volumes!

#### Quantifying RNA yield

1. Clean Nanodrop with a kim wipe
2. Pipet 1 µL of elution solution onto the Nanodrop to calibrate it. Avoid pipetting any air bubbles onto the device.
3. Pipet 1 µL of each sample onto the Nanodrop to quantify RNA yield. Avoid pipetting any air bubbles onto the device.
4. After quantification, store RNA at -80ºC.

### Results

**Table 1**. RNA quantification results from Nanodrop. All results can also be found in [this spreadsheet](https://github.com/yaaminiv/coral-hypoxia-omics/blob/main/metadata/Coral_Hypoxia_RNA_Yields.xlsx)

| **Sample** |    **Species**   | **Yield (ng/µL)** | **A260/A280** | **A260/A230** |
|:----------:|:----------------:|:-----------------:|:-------------:|:-------------:|
|     29A    | *A. cervicornis* |        2.1        |      1.34     |      0.16     |
|     6A     | *A. cervicornis* |        15.9       |      1.84     |      0.25     |
|     2A     | *A. cervicornis* |        49.8       |      2.00     |      0.96     |

Again, not great. 29A had barely any tissue on it to begin with, and the yield was similar to my [last try](https://yaaminiv.github.io/Coral-Hypoxia-DNA-Extractions/). Sample 6A had more tissue than 2A, so I'm not surprised that the yield for that sample was worse. My guess is that if I did not clog the columns, I would have better yields! My next step is to take samples 6A and 2A and try a gradient of starting material amounts to determine how much I should be using.

### Going forward

1. Try different amounts of starting material for multiple samples to identify the optimal amount of starting material
3. Try bead beating, sitting on ice for 15 minutes, bead beating again, and another 15 minute ice rest
2. Reconstitute a fresh batch of DNase I
4. Run a gel to validate RNA extraction quality for a subset of control samples
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
