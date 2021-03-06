---
layout: post
comments: true
title: SRM Dilution Calculations
tags: DNR SRM mass-spec
---

## My prior calculations were incorrect!

As I mentioned [previously](https://yaaminiv.github.io/SRM-Assay-Day9/), Laura and I need to run dilutions of our samples to confirm that as the concentration of protein in our samples decreases, the assay does not detect them as intensely. When I showed Emma my calculations, she suggested making dilutions based on protein concentrations (µg) as opposed to µL of sample added. Here are my final numbers:

**Table 1**. Dilution calculations

| **Vial** | **Dilution Ratio** | **µg to Inject** | **µL to Inject** | **µg oyster Needed** | **µg geoduck Needed** | **µL oyster needed ((µg/µL)x15 µL/1 µg/µL)** | **µL geoduck needed ((µg/µL)x15 µL/2 µg/µL)** | **PRTC added** | **µL  ACN** | **Total Volume (µL)** |
|:--------:|:------------------:|:----------------:|:----------------:|:--------------------:|:---------------------:|:------------------------------------------:|:-------------------------------------------:|:--------------:|:-----------:|:---------------------:|
|     1    |        10:1        |         1        |         1        |          0.9         |          0.1          |                    13.5                    |                     0.75                    |       1.5      |     N/A     |         15.75         |
|     2    |        7.5:1       |         1        |         2        |         0.87         |          0.13         |                     6.5                    |                     0.5                     |      0.75      |     7.25    |           15          |
|     3    |         5:1        |         1        |         2        |          0.8         |          0.2          |                      6                     |                     0.75                    |      0.75      |     7.5     |           15          |
|     4    |        2.5:1       |         1        |         2        |          0.6         |          0.4          |                     4.5                    |                     1.5                     |      0.75      |     8.25    |           15          |
|     5    |        1:2.5       |         1        |         2        |          0.4         |          0.6          |                      3                     |                     2.25                    |      0.75      |      9      |           15          |
|     6    |         1:5        |         1        |         2        |          0.2         |          0.8          |                     1.5                    |                      3                      |      0.75      |     9.75    |           15          |
|     7    |        1:7.5       |         1        |         2        |         0.13         |          0.87         |                      1                     |                     3.25                    |      0.75      |      10     |           15          |
|     8    |        1:10        |         1        |         1        |          0.1         |          0.9          |                     1.5                    |                     6.75                    |       1.5      |     5.25    |           15          |

For PRTC, our stock concentration volume is 0.5 pmol/µL. I changed the injection volume for the 10:1 and 1:10 dilutions from 2 µL to 1 µL to ensure we were pipetting volumes over 0.5 µL. This made it so that the final volume for the 10:1 injection was 15.75 µL, but Emma said that was okay.

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
