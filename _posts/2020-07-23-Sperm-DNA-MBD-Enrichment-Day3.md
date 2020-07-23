---
layout: post
comments: true
title: Sperm DNA MBD Enrichment Day 3
tags: virginica labwork sperm
---

## Another QC check before enrichment

Today I took my [sheared samples](https://yaaminiv.github.io/Sperm-DNA-MBD-Enrichment-Day2/) and ran them on the BioAnalyzer. I took a 1 µL aliquot of each of my samples and diluted it with 99 µL of E.Z.N.A Elution Buffer warmed to 70 ºC. As I loaded everything into the chip, I vortexed most reagents and only went to the first stop on the pipet. I forgot to briefly vortex samples 23, 12, 31, and 48 before loading them into the chip.

The ladder and my samples were picked up by this run! Based on the electropherogram, I can't tell if my samples are sheared sufficiently:

![Capture](https://user-images.githubusercontent.com/22335838/88323246-cd70a280-ccd6-11ea-91e2-8add7dde9aeb.PNG)

![Capture2](https://user-images.githubusercontent.com/22335838/88323250-cea1cf80-ccd6-11ea-9f03-5f1061c19c72.PNG)

 **Figures 1-2**. Electropherogram and gel from successful BioAnalyzer run.
 

The electropherograms look similar to one Sam ran on different *C. virginica* samples:

![Capture3](https://user-images.githubusercontent.com/22335838/88323253-cea1cf80-ccd6-11ea-8fb7-b89482953a87.PNG)

There's a chance that the shearing worked and my sample concentrations are just too high to be picked up by the high sensitivity assay. I posted my results in [this issue](https://github.com/RobertsLab/resources/issues/966) to get Sam's thoughts before I move forward.

Looking through [my old lab notebook posts](https://yaaminiv.github.io/Virginica-MBDSeq-Day3/), I confirmed that I did dilute my previous smaples to 25 ng/µL with DNAse free water! I updated that column in [my spreadsheet](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-MBDSeq-Labwork-Calculations.xlsx). I'll dilute my samples right before I start the MethylMiner kit process.

### Going forward

2. Dilute samples to 25 ng/µL
3. Prepare reagents for MethylMiner kit (1X Buffer) and label tubes
4. Process *C. virginica* sperm samples in two batches with MethylMiner kit
5. Complete ethanol precipitation with all *C. virginica* sperm samples
6. Investigate low-input MBDBS or WGBS kits for *C. gigas* gonad samples

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
