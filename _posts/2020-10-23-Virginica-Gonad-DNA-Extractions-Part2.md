---
layout: post
comments: true
title: Virginica Gonad DNA Extractions Part 2
tags: virginica labwork gonad WGBS
---

## Yields from Zymo MicroPrep Kit test samples

I had Grace's help today in the lab! We processed [the test samples I started yesterday](https://yaaminiv.github.io/Virginica-Gonad-DNA-Extractions/) and quantified DNA and RNA yield to see if this kit will be an effective way to extract DNA and RNA from the rest of the adult samples.

### Methods: Sample Preparation

#### Step 6: Vortexed sample and centrifuge at maximum speed for 2 minutes to pellet debris. Transfer the aqueous supernatant to an RNase-free tube.

- I removed the samples from the heat block at 8:45 a.m. There was still tissue in the tubes after **12 HOURS**! I thought I needed to find a way to homogenize the sample so all of the tissue breaks down. When I asked Sam about this, he said that gonad tissue has a lot of connective tissue, and he's not surprised that I didn't get full dissolution, and he also said it probably wasn't necessary.
- To vortex, I did 10 pulses at maximum speed
- Samples were centrifuged at 21,130 rcf

#### Step 7: Add an equivalent volume of DNA/RNA Lysis Buffer to each sample and mix by vortexing.

- Added 345 µL of DNA/RNA Lysis Buffer to each sample

### Methods: Sample Purification

I had to step out of the MethCompare meeting, so Grace took over from here! All samples were centrifuged for 16,000 rcf for 30 seconds unless specified below.

#### Step 8: Transfer the sample into a IC-MX spin column in a collection tube and centrifuge. 

- Kept the labelled spin columns for DNA extractions
- Saved the flow-through for RNA extractions

#### Step 9: For RNA only, add an equal volume of 95-100% ethanol to the flow-through and mix by pipetting. Transfer the flow-through into a new IC spin column in a clean collection tube. Centrifuge and discard the flow-through.

- Grace's notes: Added 750 µL of EtOH and mixed. Had to centrifuge twice because the column could only hold 750 µL of liquid at once, so the original volume was greater than 750 µL

#### Step 10: Add 400 µL DNA/RNA Prep Buffer to the column. Centrifuge and discard the flow-through.

#### Step 11: Add 700 µL DNA/RNA Wash Buffer to the column. Centrifuge and discard the flow-through.

#### Step 12: Add 400 µL DNA/RNA Wash Buffer to the column. Centrifuge at 16,000 rcf for 2 minutes. Transfer the column to a new microcentrifuge tube.

#### Step 13: Add 15 µL DNase/RNase-Free Water to the column. Incubate at room temperature for 5 minutes. Centrifuge to elute the DNA and RNA.

- Grace's notes: Eluted by centrifuging for 1 minute at 16,000 rcf

### Methods: Sample Quantification

#### Step 14: Prepare the master solution using a 1:200 ratio of buffer to dye. For each sample and both standards, 200 µL of master solution is needed.

- RNA: Grace made the master mix solution since she's done more RNA HS Qubit runs
- DNA: I came back from my meeting and jumped in. I grabbed the pre-mixed master solution for the Qubit dsDNA HS kit (aka my favorite thing)

#### Step 14: For each standard’s designated Qubit assay tube, add 10 µL of the correct standard and 190 µL of the master solution.

#### Step 14: For each sample’s designated Qubit assay tube, add 1 µL of the sample and 199 µL of the master solution. 

#### Step 14: Vortex all Qubit assay tube for 2-3 seconds at maximum speed. Incubate tubes at room temperature for two minutes.

#### Step 14: Use Qubit to quantify RNA and DNA yield in each sample tube.

- Final DNA sample volume was 14 µL after using 1 µL for the Qubit
- For the RNA HS kit, our first sample readings said that the concentration was too high! I made a 1:20 dilution for the samples using 1 µL of sample and 19 µL of nuclease-free water. This time, the RNA concentration was too low! So I split the difference and made a 1:10 dilution (1 µL of sample and 9 µL of nuclease-free water). We got readings with the 1:10 dilutions. The final RNA sample volume is 12 µL.

### Results

**Table 2**. DNA and RNA concentration for extracted samples. A total of 15 µL of RNA or DNA was eluted per sample. RNA was diluted by a 1:10 ratio before performing the Qubit assay. Final RNA or DNA yields only consider the remaining 14 µL for DNA and 12 µL for RNA. DNA S1: 77.10, DNA S2: 38263.90, RNA S1: 80.08, RNA S2: 1518.94

| **Sample ID** | **DNA Concentration (ng/µL)** | **Total DNA Yield (ng)** | **RNA Concentration of 1:10 dilution (ng/µL)** | **Total RNA Yield (ng)** |
|:-------------:|:-----------------------------:|:------------------------:|:----------------------------------------------:|:------------------------:|
|       36      |              34.2             |           478.8          |                       72.8                     |           8736           |
|       54      |              13.8             |           193.2          |                       71.6                     |           8592           |

Alright! Let's pretend that quality is great for all of these extractions. Looking at these yields, I have a BUNCH of RNA. Sample 36 looks good for DNA for WGBS, but I'd need another round of DNA extractions for sample 54. Which begs the question: do I move forward with 5 mg of tissue and multiple extractions, or can I use a higher tissue input? Since I remembered that Hollie used this kit for the coral project, I looked at protocols from [Emma](https://github.com/emmastrand/EmmaStrand_Notebook/blob/master/_posts/2019-06-05-Soft-and-Hard-Homogenization-Protocol.md) and [Kevin](https://github.com/kevinhwong1/KevinHWong_Notebook/blob/master/_posts/2019-03-03-Zymo-Quick-DNA-Mini-Prep-Kit-Troubleshooting-on-Adult-P.-astreoides.md). Looks like they stored samples in the DNA/RNA Shield, homogenized in that, and then used 300 µL of sample volume in a column. It could be worth adding the DNA/RNA Shield to the tissue, then using the TissueTearor to homogenize the sample. I also want to try the kit's suggestion to warm the DNase/RNase-free water to 95ºC to increase yield! Although I didn't have an RNA problem this time, I think I should use the DNase I to remove any DNA contamination from the rNA column.

### Going forward

1. Figure out if there's a better way to homogenize samples (or if that's needed)
3. Continue extracting DNA and RNA
2. Check DNA and RNA yield and quality
3. Send DNA and RNA for library preparation and sequencing

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
