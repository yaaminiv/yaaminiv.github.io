---
layout: post
comments: true
title: Coral Hypoxia RNA Extractions Part 6
tags: coral-hypoxia labwork RNA tagseq
---

## Using samples from different conditions

Well...in looking at the [sample inventory spreadsheet](https://github.com/yaaminiv/coral-hypoxia-omics/blob/main/metadata/Coral_Hypoxia_Sample_Inventory_2021.xlsx) I realized that I wasn't necessarily using samples from ambient oxygen and temperature conditions for testing! Here's a rundown of the sample conditions I used previously, and the ones I used today. So far I've mainly tried samples from ambient oxygen conditions at either temperature, but I did try one sample in hypoxic conditions!

**Table 1**. Conditions for extracted samples. For samples that were extracted multiple times, "maximum yield" refers to the largest quantity (ng) of RNA extracted at once.

| **Sample** | **Oxygen** | **Temperature** | **Maximum Yield (ng)** |
|:----------:|:----------:|:---------------:|:----------------------:|
|     1C     |   Ambient  |      31.5ºC     |          887.4         |
|     2A     |   Ambient  |       29ºC      |         1444.2         |
|     4A     |     Low    |      31.5ºC     |         3100.1         |
|     5B     |   Ambient  |      31.5ºC     |         1458.7         |
|     6A     |   Ambient  |       29ºC      |         2070.6         |
|     9D     |   Ambient  |      31.5ºC     |         3047.9         |
|     13A    |   Ambient  |      31.5ºC     |         3149.4         |
|     15B    |   Ambient  |       29ºC      |          1827          |
|     20E    |   Ambient  |       29ºC      |         1789.3         |
|     24C    |   Ambient  |      31.5ºC     |         3656.9         |
|     29A    |   Ambient  |       29ºC      |          101.5         |
|     30D    |   Ambient  |       29ºC      |         1502.2         |

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
  - After adding the second round of lysate, all of the columns were clogged
  - Centrifuged all an extra minute
  - Centrifuged 20E two extra minutes total
  - Centrifuged 15B five extra minutes total
3. Add 700 µL of low stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
  - Centrifuged 13A an extra 30 seconds
  - Centrifuged 15B an extra 1 minute
4. Obtain reconstituted DNase I aliquots from the freezer. After thawing, mix 5 µL of reconstituted DNAse I per sample + 1 and 75 µL of DNase dilution solution per sample + 1 in a separate centrifuge tube.
5. Place 80 µL of the reconstituted DNase I in DNAase dilution solution in each spin column. Incubate at room temperature for 25 minutes.
  - I thought I added an extra 80 µL of DNAse to sample 20E by accident. After the incubation, all samples had a similar volume of DNAse at the bottom of the column, so I think it's okay.
6. Add 700 µL of high stringency wash to each spin column. Centrifuge for 30 seconds at 21,130 rcf and discard the flow-through.
  - Centrifuged 13A an exra 30 seconds
  - Centrrifuged 15B an extra 2.5 minutes
7. Repeat Step 3.
  - Centrifuged 15B an extra minute
8. Centrifuge samples for two minutes at 21,130 rcf to dry samples. Discard the flow-through.
9. Transfer the spin column to a clean labelled 1.5 µL centrifuge tube. Add 30 µL of elution solution to each spin column and incubate at room temperature for 1 minute. Centrifuge for 2 minutes at 21,130 rcf to elute RNA. Place eluted RNA on ice.

#### Quantifying RNA yield

1. Clean Nanodrop with a kim wipe
2. Pipet 1 µL of elution solution onto the Nanodrop to calibrate it. Avoid pipetting any air bubbles onto the device.
3. Pipet 1 µL of each sample onto the Nanodrop to quantify RNA yield. Avoid pipetting any air bubbles onto the device.
4. After quantification, store RNA at -80ºC.

### Results

**Table 2**. RNA quantification results from Nanodrop. All results can also be found in [this spreadsheet](https://github.com/yaaminiv/coral-hypoxia-omics/blob/main/metadata/Coral_Hypoxia_RNA_Yields.xlsx)

| **Sample** | **Yield (ng/µL)** | **A260/A280** | **A260/A230** |
|:----------:|:-----------------:|:-------------:|:-------------:|
|     13A    |       108.6       |      2.03     |      1.85     |
|     15B    |        63.0       |      2.04     |      0.98     |
|     20E    |        61.7       |      2.05     |      1.33     |
|     24C    |       126.1       |      2.05     |      2.13     |

Clearly there's some sample-to-sample variability, even within the ambient treatment! Still undecided if 3 vs. 4 corallites is better, but I'm leaning towards using 4 and then minimizing the amount of extra skeleton around the corallites. Tomorrow I'll try a few non-ambient samples to see if that impacts my yield, then plan a time to do a gel with Ann. My gut feeling is that this protocol will work fine for all samples, but there are likely some with less tissue that I'll need to extract multiple times.

I also ran a gel to look at RNA degradation in my samples. I picked the tube with the maximum yield for each unique sample I've extracted so far.

**Figure 1**. Gel image. Samples from left to right: 13A, 15B, 24C, 20E, 9D-4C, 1C-4C, 5B-3C, 4A-4C, 6A-3C

<img width="534" alt="Screen Shot 2022-01-20 at 5 09 18 PM" src="https://user-images.githubusercontent.com/22335838/150430137-4d223944-8ef5-4daf-a4d0-3d6a0c6044cf.png">

I'm going to send this gel photo to Maggie to see if she can send me any of her pictures. I know she had issues with degradation, but I want to see if any of the samples that sequenced well had similar gel patterns. I'm going to extract some more samples tomorrow, then run the samples with a positive control of *Nematostella* RNA so I can figure out if the issue is my RNA or any buffer.

### Going forward

1. Inventory the extraction kits and tell Maggie if we need to order more
2. Work through more *A. cervicornis* extractions and run a gel with a *Nematostella* positive control.
5. Finish *A. cervicornis* extractions
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
