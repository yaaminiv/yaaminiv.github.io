---
layout: post
comments: true
title: Sperm DNA MBD Enrichment Day 8
tags: virginica labwork sperm MethylMiner
---

## DNA elution and ethanol precipitation

Today I took the DNA samples bound to MBD-Biotin I placed on the rotating mixer [yesterday](https://yaaminiv.github.io/Sperm-DNA-MBD-Enrichment-Day7/), obtained the bound DNA, and started the ethanol precipitation process.

### Step 0: Prepare for the day

- Labelled six sets of 10 1.7 mL centrifuge tubes. I made sure each set was a different color so it would be easier to tell the samples apart when working with many tubes.
  - Sample Number + Wash 0 (ex. 6 W0)
  - Sample Number + Wash A (ex. 6 WA)
  - Sample Number + Wash B (ex. 6 WB)
  - Sample Number + Elution 1 (ex. 6 E1)
  - Sample Number + Elution 2 (ex. 6 E2)
  - Sample Number + Elution 3 (ex. 6 E3)
- Got ice

### Step 1: Remove non-captured DNA

- Removed tubes from the rotating mixer at 4ºC
- For each tube:
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "W0" tube, and placed "W0" tube on ice
  - Added 200 µL 1X Bind/Wash buffer to "B" tube
- For each tube:
  - Placed tube on rotating mixer for 3 minutes
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "WA" tube, and placed "WA" tube on ice
  - Added 200 µL 1X Bind/Wash buffer to "B" tube
  - Repeated entire process once more
- For each tube:
  - Placed tube on rotating mixer for 3 minutes
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "WB" tube, and placed "WB" tube on ice
  - Added 200 µL 1X Bind/Wash buffer to "B" tube
  - Repeated entire process once more, *EXCEPT* I added 400 µL of High Salt Elution Buffer to the "B" tube after pipetting the supernatant from the "B" tube and into the "WB" tube on the second round. I got the High Salt Elution Buffer from the 4ºC fridge

### Step 2: Elute captured DNA

- For each tube:
  - Placed tube on rotating mixer for 3 minutes
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "E1" tube, and placed "E1" tube on ice
  - Added 400 µL High Salt Elution Buffer to "B" tube
- For each tube:
  - Placed tube on rotating mixer for 3 minutes
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "E2" tube, and placed "E2" tube on ice
  - Added 400 µL High Salt Elution Buffer to "B" tube
- For each tube:
  - Placed tube on rotating mixer for 3 minutes
  - Placed tube on magnetic rack for one minute
  - Pipetted supernatant from "B" tube into corresponding "E3" tube, and placed "E3" tube on ice
  - Discarded "B" tubes
  
I finished this stage at 9:35 a.m., and took a break until 10:35 a.m. to as Sam questions about ethanol precipitations. I covered the samples with extra ice to keep them cool.

### Step 3: Ethanol precipitation

[Last time I did this](https://yaaminiv.github.io/Virginica-MBDSeq-Day4/) I added glycogen to all tubes: non-captured DNA, washes, and elutions. Talking to Sam, he suggested I put the non-captured DNA and wash tubes (W0, WA, WB) in the -20ºC. I would only need to elute DNA from those tubes if the DNA concentration from the elution is too low. I looked at [Sam's lab notebook post](https://robertslab.github.io/sams-notebook/2018/02/07/ethanol-precipitation-dna-quantification-c-virginica-mbd-dna-from-yaamini.html) to figure out how to proceed with the rest of the precipitation. He used 25 µL of Qiagen Buffer EB for the elution instead of 60 µL of DNAse-free water. I obtained Buffer EB and checked the volume to ensure I had at least 1000 µL of buffer to elute all my samples. Since I had more than enough buffer, I decided to use that. 

I also asked if there was any way I could consolidate elution tubes. I have three separate tubes for the elution with my captured, methylated DNA that will be sent for sequencing. However, I'm only going to send one tube per sample to be sequenced. Sam suggested that after the incubation in the freezer, I take one set of tubes (E1) and pellet the DNA. After removing the supernatant, I can add the liquid from another set of tubes (E2) and pellet the DNA. Finally, I'd repeat that once more and remove the supernatant, add liquid from the E3 tube set, and pellet the DNA. Although this will take longer, I'll only have one set of tubes to work with when adding cold 70% ethanol through the final reconstitution with elution buffer.

- To each elution tube (E1, E2, and E3)
  - Added 1 µL glycogen (from -20ºC freezer)
  - Added 1/10th sample volume of pH 5.2 3 M sodium acetate (made by Sam in March 2007) to each sample
    - Sample volume was 400 µL, so I added 40 µL
  - Added 2 sample volumes of 100% ethanol (200 proof) to each sample
    - Added 800 µL to all tubes
- Gently mixed all tubes by inverting up and down
- Placed tubes in the -80ºC freezer
  - My original plan was to incubate at -80ºC for two hours like the MethylMiner protocol states. After reading [these ethanol precipitation tips](https://bitesizebio.com/2839/dna-precipitation-ethanol-vs-isopropanol/#:~:text=DNA%20is%20less%20soluble%20in,faster%20even%20at%20low%20concentrations.&text=Using%20ethanol%2C%20the%20final%20concentration,2-2.5%20volumes%20of%20sample) Shelly sent, I moved them to the -20ºC freezer to incubate overnight. They were in the -80ºC for one hour.

Tomorrow I'll finish up the ethanol precipitation and start the MBD binding with my second batch of samples!

### Going forward

1. Complete ethanol precipitation
2. Process second batch of *C. virginica* sperm samples
3. Check methylated DNA concentrations
4. Send samples for sequencing

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
