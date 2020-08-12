---
layout: post
comments: true
title: Sperm DNA MBD Enrichment Day 7
tags: virginica labwork sperm MethylMiner
---

## MBD-Biotin wash and binding

Before I started labwork in earnest, I prepared 10 mL of 1X Bind/Wash Buffer with 2 mL of 5X Bind/Wash Buffer and 8 mL of DNAse-free water. I then found the rotating mixer (FTR 213 fridge) and magnetic rack (FTR 209 drawer 43). I also updated [this spreadsheet](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-MBDSeq-Labwork-Calculations.xlsx) with my final DNA volumes and concentrations [after shearing and quality checks](https://yaaminiv.github.io/Sperm-DNA-MBD-Enrichment-Day6/). I have ten viable samples (not processing 31 due to low yield), six of which I'm able to use 4 µg of DNA from. I decided to process five samples today that I can use 4 µg of DNA from: 6, 7, 12, 13, and 23. The other five samples all require variable input volumes, so I can process them in a second batch. Working with five samples at a time also reduces the amount of tubes I'll juggle when eluting DNA fragments.

I started the [MethylMiner protocol](https://github.com/RobertsLab/resources/blob/master/protocols/Commercial_Protocols/Invitrogen_MethylMiner_Manual.pdf) stopped when incubating the DNA, dynabeads, and MBD-Biotin protein together. The reagent volumes I used in several steps, if not specified below, can be found [here](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-MBDSeq-Labwork-Calculations.xlsx).

### Step 1: Prepare beads

- Labelled ten 1.7 mL centrifuge tubes with sample number and "B" (ex. 6 B)
- Obtained Dynabead stock from 4ºC fridge
- Resuspended beads by gently pipetting up and down (slow and only to the first stop)
- Added 10 µL of beads for each µg of DNA to each centrifuge tube
- Brought volume in tube up to 100 µL with 1x Bind/Wash Buffer, and gently mixed tube contents by pipetting
- For each tube (one tube at a time):
  - Placed tubes on a magnetic rack for 1 minute to concentrate beads
  - Kept tube in rack and removed supernatant. It's important to not touch the beads in this step!
  - Immediately removed tube in the rack and added 100 µL of 1X Bind/Wash Buffer to resuspend beads. The beads cannot be left dry for more than a minute. 
  - Repeated this procedure once more for a total of two times.

### Step 2: Bind MBD-Biotin protein

- Labelled ten 1.7 mL centrifuge tubes with sample number and "P" (ex. 6 P)
- Obtained MBD-Biotin Protein from -80ºC freezer (rack 12, column 3, row 2)
- For each µg of input DNA, I added 7 µL of MBD-Biotin Protein
- Topped off volume to 200 µL with 1X Bind/Wash Buffer, gently pipetting to mix
- Transfered MBD-Biotin protein tube contents to corresponding "B" tube
- Placed all "B" tubes on a rotating mixer for one hour at room temperature
  - Start: 10:47 a.m., end: 11:47 a.m.

### Step 3: Wash MBD and beads

- For each tube:
  - Placed tubes on a magnetic rack for 1 minute to concentrate beads
  - Kept tube in rack and removed supernatant. It's important to not touch the beads in this step!
  - Immediately removed tube in the rack and added 100 µL of 1X Bind/Wash Buffer to resuspend beads. The beads cannot be left dry for more than a minute.
  - Placed tubes back in rotating mixer for 5 minutes at room temperature
  - Repeated this procedure twice more for a total of three times.
- For each tube:
  - Placed tubes on a magnetic rack for 1 minute to concentrate beads
  - Kept tube in rack and removed supernatant. It's important to not touch the beads in this step!
  - Immediately removed tube in the rack and added 100 µL of 1X Bind/Wash Buffer to resuspend beads. The beads cannot be left dry for more than a minute. 

### Step 4: Add DNA

- Set up rotating mixer in the 4ºC fridge in FTR 213
- Diluted samples to 25 ng/µL with DNAse-free water
  - Sample 7 needed to be diluted with ~800 µL of water so I needed to transfer the sample from the small shearing tube to a 1.7 mL centrifuge tube
- Labelled ten 1.7 mL centrifuge tubes with sample number and "DNA") (ex. 6 DNA)
- Added 100 µL of **5X Bind/Wash Buffer** to each tube
- Added designated sample DNA volume from calcuations to each "DNA" tube
  - Used all of sample 6 and 12
- Topped "DNA" tube volumes to 500 µL with DNAse-free water
  - And...I messed this up a little bit. Before I topped off the volumes, I realized I added 100 µL of 1X Bind/Wash Buffer and not 5X Bind/Wash Buffer! Adding 100 µL of 1X Bind/Wash Buffer means that I added 20 µL of 1X Bind/Wash Buffer and 80 µL of DNAse-free water. I added 80 µL of 5X Bind/Wash Buffer to each "DNA" tube. I subtracted 80 µL from the amount of DNAse-free water needed to top off the volumes to 500 µL, then added that! Another instance of a potentially-disasterous-but-luckily-not lab struggle.
- Transfered "DNA" tube contents to corresponding "B" tubes
  - When I transfered the "DNA" tube contents to the "B" tube contents, the measured volume of all the tubes was 500 µL. My calculations in the previous step worked out.
  - I spilled a little bit of sample 13 when transferring the contents. Only a little bit leaked out of the pipet and when I looked at the volume of liquid in 13 B vs. 23 B, they looked very similar
- Mixed tubes on a rotating mixer overnight at 4ºC
  - Placed the tubes in the mixer at 1 p.m.

### Going forward

1. Label six sets of 1.7 mL centrifuge tubes for non-captured DNA, washes, and elutions
2. Make 10 mL more of the 1X Bind/Wash Buffer
3. Complete elutions and ethanol precipitations
4. Process second batch of *C. virginica* sperm samples

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
