---
layout: post
comments: true
title: Virginica Gonad DNA Extractions
tags: virginica labwork gonad WGBS
---

## Testing the Zymo MicroPrep Kit (again)

Based on the coral methylation paper results, we decided to do WGBS for the male and female samples. Our results show that samples tend to cluster by method instead of biological sample, which is concerning if we're trying to compare WGBS zygotes and larvae with MBSBS adults! My [sperm MBD samples](https://yaaminiv.github.io/Sperm-MBD-Enrichment-Day11/) aren't going to be used in the main analysis, but I think there will still be a way to do an oyster methylation comparison with WGBS and MBDBS that doesn't involve a large methylated genome or symbiont contamination.

But I digress. Alan sent [sperm and female gonad tissue samples](https://docs.google.com/spreadsheets/d/1CQ1WpMPchdFV3iDc8cgP_dCbK6rsJoKOeRg8oPCm-E0/edit#gid=0) for me to extract DNA from for WGBS. This means I'll need 500 ng of DNA in a 25 ng/µL solution for each sample. Since [my larger goals](https://yaaminiv.github.io/Gigas-and-Virginica-Labwork-Priorities/) include understanding methylation impacts on gene expression, I want to get RNA from these samples. Originally I thought I'd do some Trizol extractions, but Sam suggested I give the [Zymo Quick DNA/RNA Microprep Plus Kit](https://github.com/RobertsLab/resources/blob/master/protocols/Commercial_Protocols/ZymoResearch_quick-dna-rna_microprep_plus_kit_20190411.pdf) another try. [Previously, I tested the kit on *C. gigas* adductor tissue](https://yaaminiv.github.io/Gigas-Broodstock-RNA-Extraction/). I messed up a bit and got some poor yields, but even if my yields were doubled it would have been minimal. I posted my concerns in [this issue](https://github.com/RobertsLab/resources/issues/1012), and Sam suggested I do a liquid nitrogen homogneization. He also confirmed that an overnight incubation would probably be alright.

To test this protocol, I selected samples with higher tissue mass. Since the protocol only needs 5 mg of tissue per sample, this would give me enough leftover tissue to test protocol variations if needed. I picked samples 36 and 54 for the female samples, since the male samples didn't have associated masses. 

### Methods: Sample Preparation

#### Step 1: Prepare for extractions.

- Label 3 sets of tubes RNase-free centrifuge tubes per sample: one for frozen tissue, one for final RNA storage, and one for final DNA storage.
- Add 1040 µL Proteinase K Storage Buffer to Proteinase K vial. Vortex and store at -20ºC
- Set heat block to 55ºC
- Obtain samples from -80ºC freezer and place on wet ice

#### Step 2: Cut and weigh no more than 5 mg of frozen tissue. Record weight of tissue used in extractions and place tissue in a new, labelled test tube.

- I tared the scale with a piece of weigh paper. I used a clean spatula with a sharp edge to cut the tissue in the tube. Once I got the weight I wanted, I transferred the tissue to the labelled centrifuge tube with the tweezers.
- The spatulas were washed in 200 mL of a 10% bleach solution, then rinsed with DI water after both samples were prepared.

**Table 1**. Mass of samples used for DNA/RNA extractions.

| **Sample ID** | **Mass (mg)** |
|:-------------:|:-------------:|
|       36      |       4.9     |
|       54      |       4.8     |

After this step, I remembered how small 5 mg of tissue is! I showed the quantity to Sam, and he changed his mind about liquid nitrogen homogenization since I'd probably not recover any of the sample. I put the 0.5 mg and original samples back in the -80ºC, since I only needed to place the samples on the heat block in the afternoon.

#### Step 3: Add at least 150 µL of DNA/RNA shield (2X) and 150 µL nuclease-free water to create 300 µL DNA/RNA shield (1x) to each sample. If the sample is not covered by water, add more DNA/RNA shield (1x) until covered and record the volume of liquid added.

#### Step 4: For every 300 µL of sample, add 30 µL PK Digestion Buffer and 15 µL Proteinase K. Mix by vortexing gently.

- After adding 300 µL nuclease-free water, my sample volume had effectively doubled. I added an additional 30 µL PK Digestion Buffer and 15 µL Proteinase K, totalling 60 µL PK Digestion Buffer and 30 µL Proteinase K per sample.

#### Step 5: Place samples on a heat block at 55ºC overnight.

- I started the incubation at 4:20 p.m.

If using 5 mg of gonad doesn't give me a high enough DNA yield tomorrow, I can start with a higher tissue mass (probably 10 mg) to see if that improves my yields without clogging the spin column. And if 10 mg doesn't work.......I'll think more seriously about Trizol or splitting the tissue masses in half for the E.Z.N.A. kit and separate RNA extractions.

### Going forward

1. Extract DNA and RNA
2. Check DNA and RNA yield
3. Determine if I need to tweak protocol

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
