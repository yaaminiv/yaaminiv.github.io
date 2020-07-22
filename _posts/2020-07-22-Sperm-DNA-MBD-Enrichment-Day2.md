---
layout: post
comments: true
title: Sperm DNA MBD Enrichment Day 2
tags: virginica labwork sperm
---

## Additional sample preparation

After [yesterday's issue with the BioAnalyzer](https://github.com/RobertsLab/resources/issues/966), I knew I needed to rerun the samples. My goal today was to figure out how to get my samples to run properly on the BioAnalyzer, recover samples that evaporated, and shear my samples.

### Checking the BioAnalyzer

Sam pointed out that I should use the centrifuge on rcf instead of rpm. I created a new gel matrix and spun the sample at 2300 rcf for 15 minutes. This worked out well for me, as the entirety of the gel matrix made it through the filter! For samples 9, 13, 31, and 63, I added 44 µL of 70ºC E.Z.N.A. Elution Buffer and flicked the samples to mix the DNA with the buffer. I prepared another BioAnalyzer chip with my samples and ran it. I ended up with the same crappy results! The software couldn't detect the ladder or upper markers on any sample. There were also split peaks. Sam looked into this and found that my samples are likely too concentrated. Since they evaporated, there is too much salt content. Additionally, my sample concentrations were above the 10 ng/µL maximum for the high sensitivity assay. To confirm that the ladder wasn't degraded, Sam had me run a blank chip — only with markers and the ladder — while he watched to ensure I wasn't doing anything incorrectly. He pointed out that I should only go to the first stop, not the second, when dispensing solutions into the chip to not introduce any air. We ran the blank chip and things seemed okay! The only difference between this chip and the ones I ran previously were the fact that I went down to the second stop on the pipet when filling the chip. The air bubbles must have seriously messed with things. I updated [this issue](https://github.com/RobertsLab/resources/issues/966) with what I've learned so I don't forget it!

### Reconstituting samples

When preparing the chip this morning, I noticed that I had more samples that had partially evaporated. Sam suggested I bring the sample volumes up to 40 µL, then run them on the Qubit to get concentrations. I measured each sample volume with the pipet, then added E.Z.N.A elution buffer warmed to each sample. I prepared the Qubit dsDNA broad range mix (2786 µL buffer, 14 µL dye) and added 190 µL to each standard and 199 µL to each sample tube. I then added 10 µL of standard and 1 µL of sample to the correct tubes. After vortexing and incubating for 2 minutes, I ran the samples.

**Table 1**. Qubit readings for all samples after reconstitution. Standard 1 = 275.34 and Standard 2 = 33541.31

| **Sample ID** | **DNA Concentration (ng/µL)** | **Final Volume (µL)** | **Total DNA Yield (ng)** |
|:-------------:|:-----------------------------:|:---------------------:|:------------------------:|
|   L18A0006s   |               121             |           40          |           4840           |
|   L18A0007s   |              550              |           40          |           22000          |
|   L18A0009s   |              95.0             |           44          |           4180           |
|   L18A0012s   |              111              |           40          |           4440           |
|   L18A0013s   |              958              |           44          |           42152          |
|   L18A0023s   |              318              |           40          |           12720          |
|   L18A0031s   |              9.82             |           44          |          432.08          |
|   L18A0048s   |              32.6             |           40          |           1304           |
|   L18A0057s   |              1100             |           40          |           44000          |
|   L18A0059s   |              72.4             |           40          |           2896           |
|   L18A0063s   |              60.8             |           40          |          2675.2          |

### Shearing

I turned on the Diagenode BioRuptor and waited for it to reach 4ºC. Once it was at 4ºC, I transferred all of my samples to 0.5 mL tubes (available next to the BioRuptory). The BioRuptor has space for 12 samples, but I only have 11. Since it needs to be balanced, I also created a blank with 45 µL of water. I loaded all of my samples into the BioRuptor and ran 30 cycles (30 seconds on, 30 seconds off) with the knob switched to "L". Once my samples were done shearing, I placed them back in the 4ºC fridge. Tomorrow, I'll need to dilute an aliquot of these samples to less than 10 ng/µL so I can run it on BioAnalyzer effectively.

### Looking into MethylMiner specifics

The last thing I wanted to figure out today was whether or not I needed to dilute or reconcentrate my samples for the MethylMiner kit. My [calculations spreadsheet](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-MBDSeq-Labwork-Calculations.xlsx) has a column called "Volume for 25 ng/µL concentration (µL)," but my previous lab notebook entries don't say anything about diluting samples before I run them. I found this section of the [Invitrogen manual](https://github.com/RobertsLab/resources/blob/master/protocols/Commercial_Protocols/Invitrogen_MethylMiner_Manual.pdf):

![Screen Shot 2020-07-22 at 4 00 17 PM](https://user-images.githubusercontent.com/22335838/88239852-5ede0700-cc3a-11ea-857b-a13c25dcefe5.png)

With the exception of one sample (31), my other samples have higher concentrations than 25 ng/µL. So I don't need to dilute my samples, but I think doing so would allow me to pipet the same quantities for each sample. I'll confirm my thinking with Sam tomorrow.

### Going forward

1. Confirm fragment size
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
