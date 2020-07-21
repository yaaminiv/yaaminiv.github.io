---
layout: post
comments: true
title: Sperm DNA MBD Enrichment
tags: virginica labwork sperm MethylMiner
---

## Prepping for *C. virginica* sperm MBD Enrichment

As expected, today was about getting things together for more intense lab days later this week. It was nice (and weird) to be back in FTR! The building feels like a time capsule: nothing has changed since I left in March.

First thing I did in FTR? Bug Sam! I asked him the questions I posed in [this issue](https://github.com/RobertsLab/resources/issues/965). He suggested I process my samples in two batches: one batch with a standard 4 µg input volume, and another batch with different input volumes. That way I'm not handling too many tubes on my own, and I won't make any mistakes. When I asked about the sample with < 1 µg total DNA, he agreed that concentrating this sample would be a waste of time since the concentration is already so small. I'll either process that sample as is, or choose not to use it since the input volume is small. He also showed me how to use the BioAnalyzer and BioRuptor.

### Checking quantities of MethylMiner kit

Based on Sam's feedback (and Steven's confirmation that I should do MBDBS unless there aren't enough reagents), I tracked down each MethylMiner kit component and confirmed I would have enough for this round of samples. I returned to [this spreadsheet](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-MBDSeq-Labwork-Calculations.xlsx) to update the total volume of DynaBeads, MBD-Biotin, and 1X buffer I would need. I also went through my previous [*C. virginica* gonad MBD](https://yaaminiv.github.io/Virginica-MBDSeq-Day4/) lab notebook post to discern how much glycogen and high elution buffer I'd need. My final approximations:

- 350 µL DynaBeads
- 250 µL Biotin
- 30 mL 1X Buffer (would need to dilute 5X buffer from kit)
- 40 mL High Salt Buffer
- 70 µL glycogen

I found the DynaBeads and buffers in the 4ºC fridge, the glygogen in the -20ºC, and the MBD-Biotin in the -80ºC (rack 12, column 3, row 2). I had to pipet the DynaBeads to estimate a volume, but I think I have enough reagents to get myself through this set of samples. Steven suggested I order more MethylMiner kits since I have more samples to process after I complete my extractions. I submitted a request to order 4-5 more MethylMiner kits to the #purchasing channel on Slack.

### Sample integrity with BioAnalyzer

Next, I needed to check my DNA integrity with the BioAnalyzer. I could assume that my DNA is in a prime state before shearing, but if it's already degrading, shearing may not be necessary. To use the BioAnalyzer, I followed the really nice instructions. First, I put the red and blue tubes out for 30 minutes to come to room temperature. Then, I added 15 µL of the blue tube (dye) to the red tube and vortexed it. I transfered the contents of this tube to a filter tube and centrifuged the filter tube for 15 minutes at 2300 rpm. After 15 minutes, there was still a good amount of blue dyed material above the filter. As a sanity check, I centrifuged it for 1.5 additional minutes at 2500 rpm. The maximum allowable speed is 2688 rpm, so I thought I could spin it fast for a short period of time to confirm that a decent amount of the gel matrix was too thick for the filter. There was still gel matrix above the filter, so I discarded the filter and covered the gel matrix in tin foil to protect it from light.

Next, I added 9 µL of the gel matrix to the appropriate slot. I then pushed the plunger until the lever held it in place. This took me two tries, but I got it to work. I then added 9 µL of the gel matrix to the other slots, 5 µL of the, and 1 µL of the ladder. Finally, I went to add in 1 µL of my samples. When doing this, I found that samples 13 (control), 31 (treatment), 63 (treatment), and 9 (treatment) were empty! There was no liquid at all. At first I chucked the tubes for 13 and 31, but then realized there may be a way to salvage the DNA and recovered them from the trash. I loaded the samples I could on the High Sensitivity DNA chip and ran the BioAnalyzer.

When I got my BioAnalyzer results, I was confused. It looked like most of my samples had really small DNA fragments (≤ 100 bp) unless I was interpreting the axes incorrectly:

**Figures 1-2**. BioAnalyzer electropherogram and gel.

I also got errors of a port malfunctioning even though data were collected from all wells:

**Figure 3**. BioAnalyzer errors.

I posted [this issue](https://github.com/RobertsLab/resources/issues/966) to understand my results.

### Resuspending samples

While the BioAnalyzer was running, I asked Sam if there was any way to resuspend the samples. He said that samples stored in the 4ºC fridge tend to evaporate if stored for too long (and my samples have been stored since last March). He recommended resuspending in the elution buffer from the EZNA kit since I used that kit to extract DNA. Then, I can flick the tubes a little and have DNA! I'll do this tomorrow since I'm pretty sure I'll need to rerun the BioAnalyzer anyways.

### Going forward

1. Get DNA out of evaporated samples
2. Rerun BioAnalyzer
3. Shear DNA and confirm fragment size
2. Prepare *C. virginica* sperm samples in two batches with MethylMiner kit
2. Complete ethanol precipitation with all *C. virginica* sperm samples
3. Investigate low-input MBDBS or WGBS kits for *C. gigas* gonad samples

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
