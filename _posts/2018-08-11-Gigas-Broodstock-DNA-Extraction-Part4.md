---
layout: post
comments: true
title: Gigas Broodstock DNA Extraction Part 4
tags: manchester labwork gigas-broodstock
---

## Protocol test: Round 3

TL;DR Nothing really works.

I used the [same protocol](https://yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part3/) as the last two trials, but with one alteration. [Sam suggested](https://github.com/RobertsLab/resources/issues/277) I try four different methods for lysating my samples (Steps 12-13):

- Tissue Tearor at setting 2 for 20 s (Note: Laura said she used the Tissue Tearor at setting 15 in [this lab notebook](https://laurahspencer.github.io/LabNotebook/Test-DNA-Extraction/). She must have used one different than what's in FSH 213, because there was no setting 15 on the one I used).
- Glass beads + vortexing for 2 minutes
- Glass beads + vortexing for 5 minutes
- Glass beads + vortexing for 10 minutes

![img_9365](https://user-images.githubusercontent.com/22335838/43999396-6b4c6b62-9dc0-11e8-92a3-ef5420a3b518.JPG)

**Figure 1**. Tissue Tearor used for protocol.

### Notes

- My plan was to take 0.08 g of tissue from 3 separate tissue samples, and split it amongst the methods listed above. Tube labels consisted of a tissue marker (T1, T2, or T3), and the method (TT for Tissue Tearor, V2 for 2 minute vortex, V5 for 5 minute vortex, and V10 for 10 minute vortex). However, the tissues themselves had varying thickness and sizes, so I used 2 tissues from the same block for each tissue marker. Here's how the tissue was split up. T1 and T2 were taken from the 1 notch block, and T3 was taken from the 5 notch block.

![img_9366](https://user-images.githubusercontent.com/22335838/43999348-94021a76-9dbf-11e8-9ddc-54189c3982e4.JPG)

**Figure 2**. 5 notches (left) and 1 notch (right)

![img_9367](https://user-images.githubusercontent.com/22335838/43999349-94194b92-9dbf-11e8-850a-4c0c40005b42.jpg)

![img_9368](https://user-images.githubusercontent.com/22335838/43999350-9432b2c6-9dbf-11e8-8a2c-8d3f3fa41700.jpg)

**Figures 3-4**. Division of tissues for histology.

- I transfered the extracted tissue to the tube first, then mashed it in the tube so get it into finer pieces. While transferring, I accidentally added T3-V5 tissue to T3-V2. I took the big chunks that weren't pressed against the tube walls out and put it in T3-V5.
- In between tissues, I dipped my scalpel in a 20% bleach solution, then washed with DI water
- While carving out tissue, I avoided tissue that had histology ink on it. However, after the xylene addition, there was a blue histology ink hue in tube T1-TT.
- Step 9: I incubated the tubes for 10 minutes at room temperature, and 13 minutes at 37ºC.
- Step 12-13: The adapter for the vortex didn't work, so I had to hold the tubes on the vortex manually. Either I now have extremely great circulation in my hands, or I did something very bad.
- Step 12-13: In between samples while using the Tissue Tearor, I cleaned the tip in ethanol for 15 s, then 20 s in DI water
- Step 14: I got the adapter to work by holding it in place with my hands! Better than holding three tubes on the vortex for 10 minutes continuously.
- Step 16: There was no gelatinous pellet after incubation and vortexing, so I think the Proteinkinase K digestion went well
- Step 17: Incubated the tubes 1 extra minute while waiting for the thermomixer to reach 80ºC
- Step 28: I multipied the recipe from previous days by 5. I mixed 3283.5 µL of the buffer with 16.5 µL of dye. S1 = 181.41, S2 = 22377.75.

### Results

**Table 1**. Tube number, initial sample added, and DNA concentration. All of these concentrations are too low for downstream appliciations, and there does not seem to be any pattern regarding method and yield. T2-V2 yield was too low for the Qubit to measure. 50 µL of the elution buffer was used.

| **Tube** | **Initial Mass (g)** | **DNA Concentration (ng/µL)** |
|----------|----------------------|-------------------------------|
|   T1-TT  |        0.0211        |              2.09             |
|   T1-V2  |        0.0210        |              1.72             |
|   T1-V5  |        0.0206        |              2.82             |
|  T1-V10  |        0.0208        |             0.524             |
|   T2-TT  |        0.0208        |              2.05             |
|   T2-V2  |        0.0204        |              N/A              |
|   T2-V5  |        0.0201        |             0.484             |
|  T2-V10  |        0.0204        |             0.804             |
|   T3-TT  |        0.0202        |              9.60             |
|   T3-V2  |        0.0205        |              6.20             |
|   T3-V5  |        0.0205        |             0.644             |
|  T3-V10  |        0.0202        |              2.92             |
  
### Going forward

Seeing how these changes to the protocol are not working and I have to return the thermomixer first thing Monday morning, I think I should move on to either 1) RNA extractions from the histology cassettes or 2) DNA extractions from ctenida, mantle, or adductor tissue. I updated [this issue](https://github.com/RobertsLab/resources/issues/277) for a response.

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
