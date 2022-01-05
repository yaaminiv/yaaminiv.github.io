---
layout: post
comments: true
title: Coral Hypoxia RNA Extractions
tags: coral-hypoxia labwork RNA tagseq
---

## Testing the TagSeq RNA extraction protocol

Maggie and I spent today testing an RNA extraction protocol she used previously to generate TagSeq libraries. The goal was to quantify the RNA yields and come up with a plan for me to finish the remaining extractions.

### Experimental design

Maggie exposed two Caribbean corals — *Acropora cervicornis* and *Orbicella faveolata* — to hypoxia and warming in a factorial design. There were five *A. cervicornis* genotypes and six *O. faveolata* genotypes represented in the experiment. The exposures lasted until half of the corals in the hypoxia and warming treatment bleached. For *A. cervicornis*, the exposures only lasted a few days, whereas the *O. faveolata* exposure lasted for a few weeks. Maggie collected coral fragments at the end of the experiment for each species and saved them in RNAlater. My role for this project is to extract and sequence RNA using TagSeq to determine how gene expression changes with treatment, genotype, and species. I can also pair this gene expression data with metabolomics, metagenomics analyses of bacterial community composition, and metatranscriptomics data for symbiont gene expression (but I don't want to get too ahead of myself).

Our focus today was to see how much RNA we could get out of a few samples! We tested the [Aurum Total RNA Mini Kit](https://www.bio-rad.com/sites/default/files/webroot/web/pdf/lsr/literature/4110133.pdf) protocol using two *A. cervicornis* (29A and 30D; control samples kept in a flow-through system instead of the static system used for the experiments) and two *O. faveolata* (63-2 and 62-6; control) samples. For *A. cervicornis*, the sample naming scheme is tank number + genotype letter, and each coral sample has three associated fragment tubes. For *O. faveolata*, it's genotype-tank number, and there is only one tube per sample.

### Extraction protocol

We followed a modified version of the animal tissue protocol based on previous extractions Maggie and Ann did.

#### Preparing samples for extractions

0. Obtain coral sample tubes from the -80ºC freezer and place on ice. Label bead beating tubes, 1.0 mL centrifuge tubes, spin columns, column collection tubes, and 1.5 mL elution tubes for all samples. Sterilize tweezers in a 70% ethanol solution and dry with a kim wipe.
2. Fill a bead beating tube with glass beads up to the white line at the bottom of the tube, and add 700 µL of lysis solution.  
3. Using a sterilized dry tweezer, obtain a fragment of coral roughly half the size of a pinkie nail. The coral skeleton in this fragment should have plenty of visible tissue. Samples should be kept on ice when not in use.
4. Place the bead beating tubes with lysis solution and coral fragments on a vortex. At high speed, bead beat the tubes for two minutes total over two one-minute intervals.
5. Centrifuge the samples in the bead beating tubes for 3 minutes at 16,500 rcf to precipitate the skeleton and glass beads. Transfer the lysate to a clean labelled 2.0 mL centrifuge tube. Samples should be kept on ice when not in use.
  - I had to recentrifuge 29A and 30D for 30 additional seconds to ensure I could remove all of the lysate from the tube. I noticed the symbiont cells were clumped at the bottom of the tube. Not a problem for coral RNA extractions, but it could be an issue for potential metatranscriptomics analyses.
6. Add 700 µL of 60% ethanol to the lysate in the 2 mL tubes and pipet up and down to mix. Samples should be kept on ice when not in use.

#### Isolating RNA

0. Pipet 30 µL of elution solution per sample into a centrifuge tube. Place in a 70ºC water bath.
1. Pipet 600 µL of the lysate into the spin column. Centrifuge the sample for 60 seconds at 21,130 rcf and discard the flow-through. Repeat once more with the remaining lysate in the 2 mL sample tubes.
2. Add 700 µL of low stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
3. Obtain reconstituted DNase I aliquots from the freezer. After thawing, mix 5 µL of reconstituted DNAse I per sample and 75 µL of DNase dilution solution sample in a separate centrifuge tube.
4. Place 80 µL of the reconstituted DNase I in DNAase dilution solution in each spin column. Incubate at room temperature for 25 minutes.
5. Add 700 µL of high stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
6. Repeat Step 2.
7. Centrifuge samples for two minutes at 21,130 rcf to dry samples. Discard the flow-through.
8. Transfer the spin column to a clean labelled 1.5 µL centrifuge tube. Add 30 µL of elution solution to each spin column and incubate at room temperature for 1 minute. Centrifuge for 2 minutes at 21,130 rcf to elute RNA. Place eluted RNA on ice.

#### Quantifying RNA yield

1. Clean Nanodrop with a kim wipe
2. Pipet 1 µL of elution solution onto the Nanodrop to calibrate it. Avoid pipetting any air bubbles onto the device.
3. Pipet 1 µL of each sample onto the Nanodrop to quantify RNA yield. Avoid pipetting any air bubbles onto the device.
4. After quantification, store RNA at -80ºC.

### Results

**Table 1**. RNA quantification results from Nanodrop

| ** Sample ** |   ** Species **  | ** Yield (ng/µL) ** | ** A260/A280 ** | ** A260/A230 ** |
|:------------:|:----------------:|:-------------------:|:---------------:|:---------------:|
|      29A     | *A. cervicornis* |         3.5         |       2.00      |       0.21      |
|      30D     | *A. cervicornis* |         51.8        |       2.04      |       0.95      |
|     63-2     |  *O. faveolata*  |         15.2        |       1.96      |       0.73      |
|     62-6     |  *O. faveolata*  |         19.3        |       1.85      |       0.87      |

Well, the yields weren't great. 29A is especially low because there wasn't much tissue on that sample to begin with. At least we know the protocol does extract RNA!

### Going forward

1. Try extraction protocol with additional *A. cervicornis* control samples using more starting material
2. Reconstitute a fresh batch of DNase I
3. Look at other coral TagSeq papers for additional steps to optimize yields.
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
