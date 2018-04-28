---
layout: post
comments: true
title: Virginica MBDSeq Day 3
tags: virginica labwork
---

## Dynabeads are dynamite

But first, a couple of things I forgot to mention [I did yesterday](https://yaaminiv.github.io/Virginica-MBDSeq-Day2/):

- Made 10 mL of the 1x Bind/Wash Buffer following the [MethylMiner protocol](https://github.com/RobertsLab/resources/blob/master/protocols/Commercial_Protocols/Invitrogen_MethylMiner_Manual.pdf)
  - 2 mL 5x Bind/Wash Buffer mixed with 8 mL DNAse free water
- Sample 106 had a very low concentration of DNA, so Sam set it in the speed vacuum to evaporate the liquid and further concentrate it before I used it

Today, I went through the protocol and stopped when incubating the DNA, dynabeads, and MBD-Biotin protein together. The reagent volumes I used in several steps, if not specified below, can be found [here](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-01-23-MBDSeq-Labwork/2018-01-23-Virginica-MBDSeq-Labwork-Calculations.xlsx).

### Step 1: Calculate new concentration for sample 106

- Sam removed the sample from the speed vacuum yesterday and placed the sample back in the fridge.
- Used a pipet to measure volume of sample
- Calculated the new sample concentration using the original sample concentration, the original volume, and the new volume
- Calculated the volume of DNAse-free water needed to dilute the sample to 25 ng/µL, which is the concentration specified by the protocol
- Added DNAse-free water to sample

### Step 2: Prepare beads

- Labelled ten 1.7 mL centrifuge tubes with sample number and "B" (ex. 31 B)
- Obtained Dynabead stock from fridge
- Resuspended beads by gently pipetting up and down
- Added 10 µL of beads for each µg of DNA to each centrifuge tube
- Brought volume in tube up to 100 µL with 1x Bind/Wash Buffer, and gently mixed tube contents by pipetting
- For each tube:
  - Placed tubes on a magnetic rack for 1 minute to concentrate beads
  - Kept tube in rack and removed supernatant. It's important to not touch the beads in this step!
  - Immediately removed tube in the rack and added 100 µL of 1X Bind/Wash Buffer to resuspend beads. The beads cannot be left dry for more than a minute. 
  - Repeated this procedure a total of two times.

### Step 3: Bind MBD-Biotin protein

- Labelled ten 1.7 mL centrifuge tubes with sample number and "P" (ex. 31 P)
- Obtained MBD-Biotin Protein from -80ºC freezer
- For each µg of input DNA, I added 7 µL of MBD-Biotin Protein
- Topped off volume to 200 µL with 1X Bind/Wash Buffer, gently pipetting to mix
- Transfered MBD-Biotin protein tube contents to corresponding "B" tube
- Placed all "B" tubes on a rotating mixer for one hour at room temperature

### Step 4: Wash MBD and beads

- For each tube:
  - Placed tubes on a magnetic rack for 1 minute to concentrate beads
  - Kept tube in rack and removed supernatant. It's important to not touch the beads in this step!
  - Immediately removed tube in the rack and added 100 µL of 1X Bind/Wash Buffer to resuspend beads. The beads cannot be left dry for more than a minute.
  - Placed tubes back in rotating mixer for 5 minutes at room temperature
  - Repeated this procedure a total of four times.
- For each tube:
  - Placed tubes on a magnetic rack for 1 minute to concentrate beads
  - Kept tube in rack and removed supernatant. It's important to not touch the beads in this step!
  - Immediately removed tube in the rack and added 100 µL of 1X Bind/Wash Buffer to resuspend beads. The beads cannot be left dry for more than a minute. 

### Step 5: Add DNA

- Labelled ten 1.7 mL centrifuge tubes with sample number and "DNA") (ex. 31 DNA)
- Added 100 µL of 5X Bind/Wash Buffer to each tube
- Added designated sample DNA volume from calcuations to each "DNA" tube
  - Here's where I boofed a little. The first sample I added to its designated "DNA" tube was sample 106. I used all of the sample I had, since that's what Sam and I discussed. I then decided to do the next three samples with the lowest concentrations: 108, 31, and 32. I added all of the sample volume from those tubes, since that's what Sam and I discussed (normalizing our sample volumes to the ones with lowest concentrations). I went to pull 160 µL from the next sample, but there wasn't enough sample volume! That's when I realized that the rest of the samples had not been diluted to 25 ng/µL! I quickly used DNAse-free water to dilute the samples to the the appropriate concentration. For the DNA samples I had already into the "DNA" tubes, I diluted them in those tubes. I accidentally added the 60 µL of water I was going to use to dilute 108 DNA into 108 B. Seeing how the next steps involve me mixing everything anyways, I figured it should be okay. Just another case of me messing up in the best possible way.
- Topped "DNA" tube volumes to 500 µL with DNAse-free water
- Transfered "DNA" tube contents to corresponding "B" tubes
- Mixed tubes on a rotating mixer overnight at 4ºC

One day of labwork done! Here's what I need to do to first thing tomorrow:

- Make 10 mL more of the 1X Bind/Wash Buffer
- Label six sets of 1.7 mL centrifuge tubes for non-captured DNA, washes, and elutions

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
