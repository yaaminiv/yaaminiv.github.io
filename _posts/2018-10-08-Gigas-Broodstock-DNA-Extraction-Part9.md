---
layout: post
comments: true
title: Gigas Broodstock DNA Extraction Part 9
tags: manchester labwork gigas-broodstock
---

## Sample extraction: Day 2

Kaitlyn lead DNA extractions today while I bounced between lab, classes, and other obligations! She extracted DNA from the following samples:

Low pH:

- 1-T3
- 7-T2
- 8-T2
- 10-T3
- 3-T1

Ambient pH:

- UK-06
- 11-T4
- UK-05
- UK-08
- UK-02

### Methods

Kailtyn and I followed [this protocol](https://github.com/RobertsLab/resources/blob/master/protocols/DNA-Extraction-from-Histology-Blocks.md). Here are some notes:

- Step 1: Kaitlyn carved out the tissue from histology blocks while I was in class! She scraped the tissue out [for the first batch](https://yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part8/), so I know the technique between batches is consistent. She noted that some of the samples from this batch were shallower, so we may not be able to extract RNA from those samples.

**Table 1**. Mass of samples used for DNA extractions.

| **Sample ID** | **Mass (mg)** |
|:-------------:|:-------------:|
|      1-T3     |      19.7     |
|      7-T2     |      19.9     |
|      8-T2     |      19.8     |
|     10-T3     |      19.7     |
|      3-T1     |      20.2     |
|     UK-06     |      20.0     |
|     11-T4     |      19.8     |
|     UK-05     |      19.7     |
|     UK-08     |      20.0     |
|     UK-02     |      20.2     |

![img_9542](https://user-images.githubusercontent.com/22335838/46627478-6b3db200-caef-11e8-8a78-69c43886ee0e.jpg)

**Figure 1**. Histology blocks after carving out tissue.

- Step 9: Ethanol was evaporated out for 10 minutes
- Step 10: Kaitlyn noted we were low on Buffer TD1
- Step 15: For the first incubation-vortex set, the temperature was 56.9ºC instead of 56ºC. I loosened the foil and allowed some air flow, which brought the temperature to 56ºC.
- Step 17: Samples incubated an extra 3 minutes while waiting for the heat block to reach 80ºC
- Step 18: During the fourth incubation-vortex set, UK-O8 and (possibly) 3-T1 leaked.
- Step 23: Some of 8-T2 spilled before centrifuging the sample
- Step 27: Samples were placed in the fridge for quantification on Tuesday

### Results

Today's DNA concentrations were more consistent than batch 1. Summary information can be found in Table 1. The same information, along with phenotypic data, can be found in [this file](https://github.com/RobertsLab/project-oyster-oa/blob/6cf11ef90159df249473e3c2ae4130a695b65bf4/data/Manchester/2018-10-2018-Broodstock-DNA-Extractions/2018-10-09-DNA-Extraction-Results.csv).

**Table 1**. Tissue mass (mg), DNA concentration (ng/µL), and total DNA eluted (ng) for each sample extracted. Treatments and extraction batches are also indicated.

| **Sample ID** | **Treatment** | **Extraction Batch** | **Tissue Mass (mg)** | **DNA Concentration (ng/µL)** | **Total DNA Eluted (ng)** |
|:-------------:|:-------------:|:--------------------:|:--------------------:|:-----------------------------:|:-------------------------:|
|      2-T1     |      Low      |           1          |          19.7        |              31.2             |            1560           |
|      4-T3     |      Low      |           1          |          19.8        |              14.4             |            720            |
|      5-T3     |      Low      |           1          |          20.0        |              9.12             |            456            |
|      6-T1     |      Low      |           1          |          20.0        |              17.9             |            895            |
|      9-T2     |      Low      |           1          |          19.6        |              15.9             |            795            |
|      1-T3     |      Low      |           2          |          19.7        |              7.78             |            389            |
|      3-T1     |      Low      |           2          |          20.2        |              6.40             |            320            |
|      7-T2     |      Low      |           2          |          19.9        |              10.8             |            540            |
|      8-T2     |      Low      |           2          |          19.8        |              19.3             |            965            |
|     10-T3     |      Low      |           2          |          19.7        |              18.3             |            915            |
|     12-T6     |    Ambient    |           1          |          20.0        |              5.64             |            282            |
|     UK-01     |    Ambient    |           1          |          19.7        |              63.0             |            3150           |
|     UK-03     |    Ambient    |           1          |          19.8        |              11.6             |            580            |
|     UK-04     |    Ambient    |           1          |          20.0        |              9.26             |            463            |
|     UK-07     |    Ambient    |           1          |          19.9        |              6.40             |            320            |
|     11-T4     |    Ambient    |           2          |          19.8        |              15.7             |            785            |
|     UK-02     |    Ambient    |           2          |          20.2        |              10.5             |            525            |
|     UK-05     |    Ambient    |           2          |          19.7        |              9.26             |            463            |
|     UK-06     |    Ambient    |           2          |          20.0        |              16.0             |            800            |
|     UK-08     |    Ambient    |           2          |          20.0        |              7.86             |            393            |

### Going forward

1. Run all samples on the Bioanalyzer
2. Determine the appropriate bisulfite sequencing prep assay
3. Figure out if I can extract RNA from remaining samples

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
