---
layout: post
comments: true
title: Gigas Broodstock DNA Extraction Part 8
tags: manchester labwork gigas-broodstock
---

## Sample extraction: Day 1

Based on Sam's response in [this issue](https://github.com/RobertsLab/resources/issues/398), I was safe to extract DNA using the TissueTearor at setting 1 for 10 seconds for the following samples:

Low pH:

- 6-T1
- 9-T2
- 2-T1
- 5-T3
- 4-T3

Ambient pH:

- UK-07
- 12-T6
- UK-04
- UK-03
- UK-01

### Methods

Kailtyn and I followed [this protocol](https://github.com/RobertsLab/resources/blob/master/protocols/DNA-Extraction-from-Histology-Blocks.md). Here are some notes:

- Step 1: Kaitlyn carved out the tissue from histology blocks while I was in class. There are definitely some samples that I cannot re-extract DNA from or try and extract RNA from.

**Table 1**. Mass of samples used for DNA extractions.

| **Sample ID** | **Mass (mg)** |
|:-------------:|:-------------:|
|      2-T1     |      19.7     |
|      4-T3     |      19.8     |
|      6-T1     |      20.0     |
|      5-T3     |      20.0     |
|      9-T2     |      19.6     |
|     12-T6     |      20.0     |
|     UK-03     |      19.8     |
|     UK-01     |      19.7     |
|     UK-04     |      20.0     |
|     UK-07     |      19.9     |

![img_9503](https://user-images.githubusercontent.com/22335838/46176719-3dda4400-c265-11e8-90a6-e36212ddae8b.jpg)

**Figure 1**. Histology blocks after carving.

- Step 10: Kaitlyn removed sample 2-T1 from the heat block after 16 minutes of ethanol evaporation. She saw some ethanol left in the rest of the samples, so she pipetted the supernatant. The remaining samples were incubated on the heating block an additional 5 minutes (20 minutes total), then removed.
- Step 11: UK-07 was dropped, but nothing spilled out (from what we could tell)
- Step 12: Taught Kaitlyn how to use the TissueTearor!

![img_9504](https://user-images.githubusercontent.com/22335838/46176636-feabf300-c264-11e8-9928-69bc935f0aa4.jpg)

**Figure 2**. Kaitlyn using the TissueTearor.

- Step 17: Incubated samples with RNase an extra 5 minutes while waiting for the heat block to reach 80ºC.
- Step 18: Relabeled some sample tubes (markings on either top or side had rubbed off) after the fourth 10 minute incubation and one minute vortex. Both labels on UK-03 rubbed off.
- Step 22: Precipitate formed in all samples.
- Step 27: Kaitlyn needed to leave, but she wanted to learn how to use the Qubit. We stored the eluted DNA in the fridge and resumed the protocol on Friday.
- Step 28: On Friday, we needed to make a master solution with 3283.5 µL of buffer and 16.5 µL of dye. However, we did not have enough dye and I could not find anymore in the lab. Kailtyn [posted an issue](https://github.com/RobertsLab/resources/issues/400). We'll resume the protocol when we have more dye.

### Results

*Update 2018-10-05* 

- Step 28: We received Qubit dye and proceeded with Step 28.
- Step 30: While adding DNA to the Qubit assay tube, we decided it would be best to vortex our samples, then pipet. Samples 9-T2, 2-T1, 4-T3, and UK-07 were not vortexed because we had already added the sample to the assay tube.
- Step 32: S1 = 266.81; S2 = 31156.45

**Table 1**. DNA concentration for extracted samples. A total of 50 µL of DNA was eluated per sample.

| **Sample ID** | **DNA Concentration (ng/µL)** |
|:-------------:|:-----------------------------:|
|      2-T1     |              31.2             |
|      4-T3     |              14.4             |
|      6-T1     |              17.9             |
|      5-T3     |              9.12             |
|      9-T2     |              15.9             |
|     12-T6     |              5.64             |
|     UK-03     |              11.6             |
|     UK-01     |              63.0             |
|     UK-04     |              9.26             |
|     UK-07     |              6.40             |

### Going forward

These extractions gave me the highest yields I've seen! I'm confident with this method now. Kaitlyn and I will extract [batch 2](https://yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part7/) on Monday.

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
