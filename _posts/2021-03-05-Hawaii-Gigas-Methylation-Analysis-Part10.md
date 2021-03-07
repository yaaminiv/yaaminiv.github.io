---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 10
tags: hawaii gigas-ploidy
---

## WGS resequencing quote

I plan on extracting SNP data from my WGBS data, but Steven suggested I [get a quote for WGS resequencing](https://github.com/RobertsLab/resources/issues/1124). This would give us more robust genotype data that we could use in our methylation analysis. While there is the possibility Sam or I could [extract additional DNA](https://github.com/RobertsLab/resources/issues/1126), I first asked Sam if there was any leftover ctenidia DNA. He said there was and shared [this lab notebook post](https://robertslab.github.io/sams-notebook/2020/08/21/DNA-Isolation-and-Quantification-C.gigas-High-Low-pH-Triploid-and-Diploid-Ctenidia.html) with the yield for each sample. According to [this other post](https://robertslab.github.io/sams-notebook/2020/08/24/Sample-Submitted-C.gigas-Diploid-Triploid-pH-Treatments-Ctenidia-to-ZymoResearch-for-WGBS.html), he used 1500 µg of DNA for each sample. I quickly calculated how much DNA is left for each sample.

**Table 1**. DNA left for each ctenidia sample

| Sample_ID | Concentration(ng/uL) | Volume(uL) | Total_DNA (ng) | Amount Left |
|:---------:|:--------------------:|:----------:|:--------------:|:-----------:|
|  2N_HI_5  |         40.4         |     100    |      4040      |     2540    |
|  2N_HI_8  |         11.6         |     100    |      1160      |      0      |
|  2N_HI_9  |         32.3         |     100    |      3230      |     1730    |
|  2N_HI_10 |          61          |     100    |      6100      |     4600    |
|  2N_HI_11 |          21          |     100    |      2100      |     600     |
|  2N_HI_12 |         11.2         |     100    |      1120      |      0      |
|  2N_LOW_1 |         32.1         |     100    |      3210      |     1710    |
|  2N_LOW_2 |         32.5         |     100    |      3250      |     1750    |
|  2N_LOW_3 |          36          |     100    |      3600      |     2100    |
|  2N_LOW_4 |         40.2         |     100    |      4020      |     2520    |
|  2N_LOW_5 |         17.8         |     100    |      1780      |     280     |
|  2N_LOW_6 |         22.8         |     100    |      2280      |     780     |
|  3N_HI_2  |         29.6         |     100    |      2960      |     1460    |
|  3N_HI_3  |         71.8         |     100    |      7180      |     5680    |
|  3N_HI_5  |         29.3         |     100    |      2930      |     1430    |
|  3N_HI_8  |         38.9         |     100    |      3890      |     2390    |
|  3N_HI_10 |         38.3         |     100    |      3830      |     2330    |
|  3N_HI_11 |         52.3         |     100    |      5230      |     3730    |
|  3N_LOW_6 |         35.3         |     100    |      3530      |     2030    |
|  3N_LOW_7 |         43.6         |     100    |      4360      |     2860    |
|  3N_LOW_8 |         63.9         |     100    |      6390      |     4890    |
| 3N_LOW_10 |         54.4         |     100    |      5440      |     3940    |
| 3N_LOW_11 |         50.8         |     100    |      5080      |     3580    |
| 3N_LOW_12 |          52          |     100    |      5200      |     3700    |

Most samples have > 1000 µg of DNA, but there are three (2N_HI_11, 2N_LOW_5, 2N_LOW_6) that have less. Two samples (2N_HI_8 and 2N_HI_12) do not have any DNA left over, and they are both from the same treatment, so that could pose an issue. It's interesting that the samples with less DNA are all diploid!

In any case, I submitted quotes to [GENEWIZ](https://www.genewiz.com) and [Northwest Genomics Center (NWGC)](https://nwgc.gs.washington.edu/?q=what-we-do/request-a-quote). I asked how much DNA is required to for library preparation and sequencing. I spoke with Katie about WGS, and she suggested I base my quote on information from the [Illumina coverage calculator](https://support.illumina.com/downloads/sequencing_coverage_calculator.html). I'll require need ~22 billion bases of output for the NovaSeq. I don't know if GENEWIZ or NWGC use NovaSeq, but Katie thinks that sequencing will cost ~$6,000 for all samples.

<img width="933" alt="Screen Shot 2021-03-05 at 12 24 09 PM" src="https://user-images.githubusercontent.com/22335838/110176964-061e4180-7db9-11eb-8bff-e333e297b811.png">

I'll update [this issue](https://github.com/RobertsLab/resources/issues/1124) with information when I get it.

### Going forward

1. Try [BS-SNPer](https://github.com/hellbelly/BS-Snper) and [EpiDiverse](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
5. Investigate comparison mechanisms for samples with different ploidy in oysters and other taxa
4. Test-run [DSS](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#34_DMLDMR_detection_from_general_experimental_design) and [ramwas](https://bioconductor.org/packages/release/bioc/html/ramwas.html)
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)
6. Update methods
7. Update results

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
