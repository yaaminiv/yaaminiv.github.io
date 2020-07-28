---
layout: post
comments: true
title: Sperm DNA MBD Enrichment Day 4
tags: virginica labwork sperm
---

## Re-shearing DNA and checking quality

Based on [this issue](https://github.com/RobertsLab/resources/issues/966), I needed to reshear samples and check quality on the BioAnalyzer. The first thing I did was check the concentrations of samples 9, 13, 31, and 63 with the Qubit. The concentrations were smaller than [my last readings](https://yaaminiv.github.io/Sperm-DNA-MBD-Enrichment-Day2/), which makes sense. My guess is that I didn't mix the samples properly before pipetting them into the Qubit tubes. I updated the concentrations in [this spreadsheet](). Based on 31's concentration, I don't think it has enough DNA for me to even try MBD with < 1 µg. I decided to drop this sample from my preparation!

While running the Qubit, I sheared samples 6, 12, 48, 57, and 59 for 5 cycles (30 seconds on, 30 seconds off at 4ºC) with the BioRuptor and took out the BioAnalyzer reagents to get to room temmperature. I then diluted the newly sheared samples and the ones I checked with the Qubit (9, 13, 63) to run them on the BioAnalyzer. For all samples except 57, I used a 1:20 dilution (1 µL sample, 19 µL DNAse-free water). Since sample 57 had a higher concentration, I used a 1:100 dilution (1 µL sample, 99 µL DNAse free water) like I did previously. I then prepared the BioAnalyzer chip. The first chip I started to prepare had an issue with the plunger not actually having any pressure applied, so I had to scrap it and use a new chip. At this point I realized I only had two chips left — the one I was about to use, and another one! I made a purchasing request for high sensitivity BioAnalyzer chips and reagents since I couldn't order the chips separately. Thankfully the plunger worked this time and I was able to prepare my chip.

When I looked at the results, the BioAnalyzer was unable to detect an upper marker for sample 13, but everything else ran well. However, I was unable to toggle between s and bp due to this blip! I took a screenshot of my previous BioAnalyzer run with the x-axis in seconds, then took screenshots of the chip I ran today. I'm hoping I can use the information from the previous run to guess the fragment length for this run of samples. I posted again in [this issue](https://github.com/RobertsLab/resources/issues/966) to confirm my thinking.

![Capture](https://user-images.githubusercontent.com/22335838/88716298-bb767180-d0d3-11ea-9a78-53a91fc7b17c.PNG)

**Figure 1**. Electropherogram from previous chip.

![Capture2](https://user-images.githubusercontent.com/22335838/88716339-c92bf700-d0d3-11ea-881c-c08aea6e0e9a.PNG)

![Capture3](https://user-images.githubusercontent.com/22335838/88716341-ca5d2400-d0d3-11ea-9bad-17dda66d1e76.PNG)

**Figures 2-3**. Electropherogram and gel from today's chip.

Hopefully I'm able to salvage the data from this chip! If I am, then here are my estimates for fragment length:

- 6: Doing something weird...will need to rerun. I remember having trouble figuring out if I actually added any sample to this well.
- 12: Peak around 60 seconds, which might be 200-300 bp
- 48: Peak around 60-100 seconds, which might be 200-600 bp
- 57: Peak around 60 seconds, which might be 200-300 bp
- 13: Peak around 60 seconds, which might be 200-300 bp (but there was also an issue with upper marker detection here)
- 9: Peak around 60-80 seconds, which might be 200-400 bp
- 59: Peak around 60 seconds, which might be 200-300 bp
- 63: Peak around 70 seconds, which might be 300-400 bp

Worst case scenario, I saved the dilutions so I can rerun a chip. I wonder if any other labs have BioAnalyzer chips I can steal until the ones we ordered come in...

### Going forward

2. Finish shearing and quality checks
3. Dilute samples to 25 ng/µL
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
