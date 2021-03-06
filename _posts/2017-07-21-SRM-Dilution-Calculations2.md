---
layout: post
comments: true
title: SRM Dilution Calculations 2
tags: DNR SRM mass-spec
---

## Revised for the last time

Today I went to UWPR to make the dilutions. Last night, Laura told me that she diluted our PRTC to 200 fmol/µL, so I had to account for that when making dilutions. The first thing I did was revise my dilution calculations with the new PRTC stock concentration. Because I needed to add more PRTC to each sample, I changed the injection volume from 1 µL to 2 µL for the 10:1 dilution. This means Laura has to pipet 0.375 µL of geoduck, which is less than 0.5 µL. Because Laura and I have pipetted this volume before and it's only for one sample, I think it is okay. Before I went down, I took 0.1 µL and 0.5 µL pipets from the lab.

**Table 1**. Revised dilution calculations

| **Vial** | **Dilution Ratio** | **µg to Inject** | **µL to Inject** | **µg oyster Needed** | **µg geoduck Needed** | **µL oyster needed ((µg/µL)x15 µL/1 µg/µL)** | **µL geoduck needed ((µg/µL)x15 µL/2 µg/µL)** | **PRTC added** | **µL  ACN** | **Total Volume (µL)** |
|:--------:|:------------------:|:----------------:|:----------------:|:--------------------:|:---------------------:|:------------------------------------------:|:-------------------------------------------:|:--------------:|:-----------:|:---------------------:|
|     1    |        10:1        |         1        |         2        |          0.9         |          0.1          |                    6.75                    |                     0.375                    |       1.875      |     6     |         15         |
|     2    |        7.5:1       |         1        |         2        |         0.87         |          0.13         |                     6.5                    |                     0.5                     |      1.875      |     6.125    |           15          |
|     3    |         5:1        |         1        |         2        |          0.8         |          0.2          |                      6                     |                     0.75                    |      1.875      |     6.35     |           15          |
|     4    |        2.5:1       |         1        |         2        |          0.6         |          0.4          |                     4.5                    |                     1.5                     |      1.875      |     7.125    |           15          |
|     5    |        1:2.5       |         1        |         2        |          0.4         |          0.6          |                      3                     |                     2.25                    |      1.875      |      7.875      |           15          |
|     6    |         1:5        |         1        |         2        |          0.2         |          0.8          |                     1.5                    |                      3                      |      1.875      |     8.625    |           15          |
|     7    |        1:7.5       |         1        |         2        |         0.13         |          0.87         |                      1                     |                     3.25                    |      1.875      |      8.875     |           15          |
|     8    |        1:10        |         1        |         1        |          0.1         |          0.9          |                     1.5                    |                     6.75                    |       3.75      |     3    |           15          |

I then created 25 µL of my pooled oyster solution by adding 5 µL of the samples I selected [here](https://yaaminiv.github.io/SRM-Assay-Day9/). 

- O14
- O39
- O96
- O124
- O137

Using the volumes in Table 1, I prepared the vials by adding my oyster sample, PRTC and ACN. When I got to Vial 6, it was difficult for me to pipet the 1.5 µL I needed, so I made 10 µL of the oyster pool. I finished up the vials and left in the freezer for Laura to add her samples to.

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
