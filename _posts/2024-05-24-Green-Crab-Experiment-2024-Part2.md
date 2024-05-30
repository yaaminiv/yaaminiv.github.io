---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 2
tags: green-crab-cold restriction-digest
---

## Designing a restriction digest

It's time to design primers! The current SMC primers yield a product that's 126 bp. This small of a product can't be used with a restriction enzyme digest. If a restriction enzyme cuts at any of the SNPs, the resultant products won't really be distinguishable on a gel. It's also possible that the really small product is what's leading to all my contamination issues!

### Primer design

To design the primers, I used [Primer 3 Plus](primer3plus.com). I uploaded sequence for the SMC region Carolyn provided me. Then, I specified which SNP site I wanted to design primers for. I wanted a 500 bp window around the primers. I also messed around with some advanced settings based on suggestions from Sarah. I wanted a lower self complement (does the primer bind to itself) and pair complement (does the primer bind to the complementary primer) than the defaults:

<img width="906" alt="Screenshot 2024-05-24 at 2 03 11 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/3d499751-ad6b-4cf6-a4de-711c3dca559b">

**Figure 1**. Settings used for primer design.

For each SNP, I went through the first three primer pairs and recorded information like the distance from the primer to the SNP, % GC content (these are stronger bonds so they increase stability. 50-55% is ideal, 30-80% is good), primer length (18-30 bp is good), annealing temperatures (65ºC-75ºC is standard, and both the forward and reverse primers should have similar annealing temperatures).

**Table 1**. Various primer options for SNPs in the supergene region

|   **SNP**  | **L Primer Start** | **L Distance to SNP (bp)** | **L Primer Length** |         **L Primer (% GC)**        | **R Primer Start** | **R Distance to SNP** | **R Primer Length** |         **R Primer (% GC)**         | **Product Size (bp)** | **Tm (L/R/Pair)** | **Self Complement Score (L/R/Pair)** | **Pair Complement Score (L/R/Pair)** | **Hairpin Structure Tm (ºC)** | **Self Structure Tm (ºC)** |
|:----------:|:------------------:|:--------------------------:|:-------------------:|:----------------------------------:|:------------------:|:---------------------:|:-------------------:|:-----------------------------------:|:---------------------:|:-----------------:|:------------------------------------:|:------------------------------------:|:-----------------------------:|:--------------------------:|
| 2265 (G/A) |        2239        |             26             |          25         |   ACTGAGATCAAAAACAGTAAAGCCA (36%)  |        2835        |          570          |          20         |      GGCATCCTCTTCCATTCGCT (55%)     |          597          |   59.2/60.2/84.5  |                 0/0/0                |                 0/0/0                |             41 (L)            |             N/A            |
| 2265 (G/A) |        2231        |             34             |          26         | TGCAGAAAACTGAGATCAAAAACAGT (34.6%) |        2380        |          115          |          21         |    CCTCTTCCATTCGCTCCATGA (52.4%)    |          600          |   60.1/59.9/84.4  |                 0/0/0                |                 0/0/0                |            43.9 (L)           |             N/A            |
| 2265 (G/A) |        2225        |             40             |          24         |  CAGAAATGCAGAAAACTGAGATCA (37.5%)  |        2824        |          559          |          20         |      CCATTCGCTCCATGACTTCT (50%)     |          600          |   57.8/57.7/84.3  |                 0/0/0                |                 0/0/0                |              N/A              |             N/A            |
| 3231 (A/C) |        3211        |             20             |          20         |     AAGAAGCTTGCACCTCAGGG (55%)     |        3758        |          527          |          27         | AGAGGAAATTTACAAAAAGGAAAAGCC (33.3%) |          548          |   60.3/59.3/84.3  |               2.1/2.6/0              |                 0/0/0                |            38.9 (L)           |           2.1 (L)          |
| 3231 (A/C) |        3205        |             26             |          23         |   GTGTTTAAGAAGCTTGCACCTCA (43.5%)  |        3865        |          634          |          26         |  ATTTACAATTTACACTGGAAGCATTC (30.8%) |          661          |    59.7/57/83.2   |                2.1/0/0               |                0/0/2.1               |              N/A              |           2.1 (L)          |
| 3231 (A/C) |        3147        |             84             |          21         |    CCGCAAATATGAAGCCATCCA (47.6%)   |        3395        |          164          |          21         |    TGTTACCTCAGGTCCCTCTCA (52.4%)    |          849          |    59/59.6/82.6   |                 0/0/0                |                 0/0/0                |        34.4/37.1 (L/R)        |             N/A            |
| 3528 (C/T) |        3474        |             219            |          20         |     TGATGCTCAGCACAGGAAGG (55%)     |        4163        |          635          |          27         | CTTCCATACTTTAACTTCATGAGAACA (33.3%) |          690          |     60/57.8/80    |                0/0/5.7               |                0/0/1.7               |             43 (L)            |   5.7/1.7 (R, Pair/Self)   |
| 3562 (C/T) |        3474        |             185            |          20         |     TGATGCTCAGCACAGGAAGG (55%)     |        4163        |          601          |          27         | CTTCCATACTTTAACTTCATGAGAACA (33.3%) |          690          |     60/57.8/80    |                0/0/5.7               |                0/0/1.7               |             43 (L)            |   5.7/1.7 (R, Pair/Self)   |

Carolyn then helped me evaluate the primers. The fourth primer in the table has runs of 3-4+ nucleotides, which she said to avoid due to slippage. Since I'm designing a restriction enzyme digest, I really want the distance from the primer to the SNP to be > 100 bp. This way, if I use the SNP as a cut site, I will end up with bands I can distinguish on the gel. Based on the above criteria, we decided to order the primer that was applicable for both C/T SNPs. If using the either C/T SNP as a cut site, I will have a ~700 bp product for one homozygote, one ~200 bp and one ~500 bp product for the other homozygote, and one 200 bp band, one 500 bp band, and one 700 bp band for a heterozygote.

Once I receive the primers, I can run PCR and confirm that my product length is roughly what I'd expect! There's a chance that the product encompasses an intron-exon boundary. If this is the case, my product would end up much longer than expected since I'm designing using the transcriptome sequences. If this happens, I can sequence the PCR product and then design a better primer with a shorter product. While waiting for my primers, my next step is to determine if there's a good restriction enzyme that encompass the C/T SNP cut sites.

### Going forward

1. Check temperature of tanks
3. Identify candidate restriction enzymes for new primers
3. Develop restriction band digest for genotyping
4. Develop heart rate protocol

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
