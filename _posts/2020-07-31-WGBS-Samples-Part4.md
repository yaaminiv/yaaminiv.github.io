---
layout: post
comments: true
title: WGBS Samples Part 4
tags: manchester labwork gigas-broodstock WGBS
---

## More planning for WGBS

After [figuring out which samples to sequence](https://yaaminiv.github.io/WGBS-Samples-Part3/), I wanted to confirm I didn't need to do anything besides have 500 ng DNA at 20 ng/µL. In [this issue](https://github.com/RobertsLab/resources/issues/971), I confirmed I wouldn't have to shear DNA! My next step was checking DNA concentrations. First, I measured the volume of the DNA samples. Then, I checked concentrations with the Qubit. When I compared the concentrations I got when I initially extracted the DNA in [this table](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/2018-10-09-Broodstock-DNA-Extractions/2018-10-09-DNA-Extraction-Results.csv), I noticed that some samples had drastically different concentrations (06-T1, UK-01, UK-02, UK-03, UK-04). I ran these samples again, along with two others (UK-07, UK-08). I wanted to see if these two samples, which were from females at similar maturation stages, also had different DNA concentrations and yields that would make them usable for WGBS:

**Table 1**. Abbreviated sample volumes and revised concentrations. If a sample will be used for sequencing, I indicated whether or not I need to do an ethanol precipitation (Yes/No). If the total DNA in a sample is < 400 ng, I decided not to use it and added "N/A" in the ethanol precipitation column. Run 1: S1 = 233.79, S2 = 26196.20. Run 2: S1 = 286.00, S2 = 34707.88.

| **Sample** | **Treatment** | **Volume (µL)** | **Concentration 1 (ng/µL)** | **Concentration 2 (ng/µL)** | **Ethanol Precipitation?** |
|:----------:|:-------------:|:---------------:|:---------------------------:|:---------------------------:|:--------------------------:|
|    02-T1   |      Low      |        40       |              37             |             N/A             |             No             |
|    04-T3   |      Low      |        40       |             20.8            |             N/A             |             No             |
|    06-T1   |      Low      |        40       |             47.2            |             44.6            |             No             |
|    07-T2   |      Low      |        40       |             15.4            |             N/A             |             Yes            |
|    08-T2   |      Low      |        40       |             22.2            |             N/A             |             No             |
|    10-T3   |      Low      |        20       |             23.2            |             N/A             |             N/A            |
|    11-T4   |    Ambient    |        30       |             17.6            |             N/A             |             Yes            |
|    UK-01   |    Ambient    |        35       |             9.16            |             8.84            |             N/A            |
|    UK-02   |    Ambient    |        45       |             10.9            |             11.7            |             Yes            |
|    UK-03   |    Ambient    |        40       |             12.4            |             10.9            |             Yes            |
|    UK-04   |    Ambient    |        40       |             66.8            |             31.6            |             No             |
|    UK-06   |    Ambient    |        45       |             19.6            |             N/A             |             Yes            |
|    UK-07   |    Ambient    |        40       |             N/A             |             5.86            |             N/A            |
|    UK-08   |    Ambient    |        40       |             N/A             |             8.02            |             N/A            |

I added these results to [my spreadsheet](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/2018-10-09-Broodstock-DNA-Extractions/2018-10-09-DNA-Extraction-Results.csv) and calculated a conservative total DNA yield in each sample based on the lowest DNA concentration I got today. From those concentrations, calculated yields, and balancing sex and maturation stage, I narrowed down my list to 10 samples. I added the information for possible RNA extractions [found in this noteboo post](https://yaaminiv.github.io/WGBS-Samples-Part3/) for reference:

**Low**:

- 02-T1
- 04-T3
- 06-T1: No tissue left for RNA extraction
- 07-T2
- 08-T2: Less than half of tissue sample left for RNA extraction

**Ambient**:

- 11-T4
- UK-02: Less than 500 ng
- UK-03: Less than 500 ng
- UK-04
- UK-06: Less than half of tissue sample left for RNA extraction

### Going forward

1. Prepare dilutions and run them on the BioAnalyzer
2. Complete isopropanol precipitations
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
