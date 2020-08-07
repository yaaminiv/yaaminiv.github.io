---
layout: post
comments: true
title: WGBS Samples Part 6
tags: manchester labwork gigas-broodstock WGBS
---

## Ethanol precipitations

Today I did ethanol precipitations for samples 7, 11, U2, U3, and U6 following [this protocol](https://github.com/RobertsLab/resources/blob/master/protocols/ethanol_precipitation_DNA.md):

- I centrifuged the samples at room temperature
- After washing the pellet with 70% ethanol, I removed the supernatant before adding more 70% ethanol.
- I warmed the E.Z.N.A. Elution Buffer to 70ºC before adding it resuspend the pellet

I then ran the samples on the Qubit!

**Table 1**. Volume (µL) reagents added for ethanol precipitation and Qubit concentrations. S1 = 340.74, S2 = 37549.58

| **Sample** | **Treatment** | **Volume (µL)** | **NaOAC Added (µL)** | **100% Ice Cold EtOH Added (µL)** | **70% EtOH Wash Added (µL)** | **Concentration (ng/µL)** |
|:----------:|:-------------:|:---------------:|:--------------------:|:---------------------------------:|:----------------------------:|:-------------------------:|
|    07-T2   |      Low      |        40       |           4          |                 80                |              400             |            18.5           |
|    11-T4   |    Ambient    |        30       |           3          |                 60                |              400             |            20.4           |
|    UK-02   |    Ambient    |        45       |          4.5         |                 90                |              400             |            21.0           |
|    UK-03   |    Ambient    |        40       |           4          |                 80                |              400             |            14.2           |
|    UK-06   |    Ambient    |        45       |          4.5         |                 90                |              400             |            47.0           |

Samples 7 and U3 needed to be precipitated again, so I did just that before checking the concentration again. 

**Table 2**. Volume (µL) reagents added for ethanol precipitation and Qubit concentrations. S1 = 159.38, S2 = 16594.26

| **Sample** | **Treatment** | **Volume (µL)** | **NaOAC Added (µL)** | **100% Ice Cold EtOH Added (µL)** | **70% EtOH Wash Added (µL)** | **Concentration (ng/µL)** |
|:----------:|:-------------:|:---------------:|:--------------------:|:---------------------------------:|:----------------------------:|:-------------------------:|
|    07-T2   |      Low      |        20       |           2          |                 40                |              300             |            22.4           |
|    UK-03   |    Ambient    |        20       |           2          |                 40                |              300             |            11.4           |

Sample 7 was fine, but U3 had a lower concentration than it did previously! I started to get suspicious about DNA loss so I updated [sample concentrations and volumes](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/2018-10-09-Broodstock-DNA-Extractions/2018-10-09-DNA-Extraction-Results.csv). I definitely lost a considerable amount of DNA and I did not save any of the supernatant...fuck. I'm SOL for these samples unless I extract more DNA, but I wonder if there's a low-input WGBS option. At least I have the ability to re-extract DNA for 7, 11, U2 and U3 if needed, but who knows if I can get RNA from them too.

### Going forward

1. Figure out what to do about samples (either extract more or find a low-input WGBS option)
2. Prepare dilutions and run them on the BioAnalyzer
3. Extract RNA from gonad samples

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
