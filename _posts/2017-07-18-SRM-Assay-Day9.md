---
layout: post
comments: true
title: SRM Assay Day 9
tags: DNR SRM mass-spec
---

## My samples are DONE.

The samples I needed to rerun to get data for (O01, O12, O22, O113, O118, O26 and O90) just finished running and all of them have data. Tomorrow, I will go to UWPR to prepare the oyster sample dilutions needed to create a dilution curve. The purpose of this is to verify that the assay is detecting peptides based on their presence in the autosampler vial. The oyster peptide detection should decrease as the concentration in the autosampler vial decreases, and vice versa.

Emma suggested pooling oyster samples to create my dilutions. I want to randomly pick oyster samples that I didn't have to remake and rerun. I also don't want to include my procedural blanks. Here are the samples I can choose from:

- O17
- O145
- O128
- O56
- O71
- O103
- O04
- O106
- O49
- O52
- O14
- O102
- O10
- O06
- O122
- O99
- O08
- O51
- O21
- O39
- O137
- O46
- O43
- O31
- O66
- O30
- O100
- O78
- O35
- O101
- O40
- O64
- O91
- O140
- O124
- O32
- O147
- O121
- O24
- O96
- O131
- O60

We want ten dilutions total. I first need to calculate how much of the pooled oyster protein sample I'll need in each vial, as well as PRTC.

**Table 1**. Volumes of solutions needed to create peptide dilutions.

| **Vial** | **Oyster Sample Volume (µL)** | **Volume PRTC (µL)** | **Remaining Volume (µL)** |
|:--------:|:-----------------------------:|:--------------------:|:-------------------------:|
|   **1**  |              7.5              |         1.89         |            5.61           |
|   **2**  |              6.7              |         1.89         |            6.41           |
|   **3**  |              5.9              |         1.89         |            7.21           |
|   **4**  |              5.1              |         1.89         |            8.01           |
|   **5**  |              4.3              |         1.89         |            8.81           |
|   **6**  |              3.5              |         1.89         |            9.61           |
|   **7**  |              2.7              |         1.89         |           10.41           |
|   **8**  |              1.9              |         1.89         |           11.21           |
|   **9**  |              1.1              |         1.89         |           12.01           |
|  **10**  |               0               |         1.89         |           13.11           |

This means I need 38.7 µL of oyster peptide sample to start with. If I used five samples, I would need 8 µL of peptide from each sample.

Randomly choosing five samples:

- O124
- O39
- O96
- O14
- O137

I will prepare my samples accordingly tomorrow! Before then, I also did some "maintenance" for my Skyline stuff:

- Exported my [complete 6/10 Skyline document](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170524/Gigas-6-10-DIA.sky.zip)
- Edited my 7/9 Skyline document to [reflect final transitions post-PRTC addition](http://owl.fish.washington.edu/spartina/DNR_Skyline_SRM_20170707/2017-07-10-FINAL-SRM-Transitions-with-PRTC/Gigas-7-10-Final-Transition-List.sky.zip)
- Updated any Jupyter notebooks or lab notebooks that needed these links

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

