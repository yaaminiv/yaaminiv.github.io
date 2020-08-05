---
layout: post
comments: true
title: WGBS Samples Part 4
tags: manchester labwork gigas-broodstock WGBS
---

## Checking samples on the BioAnalyzer

I need to [conduct an isopropanol precipitation with certain samples]() but before I do that, I figured I'd check the DNA quality on the BioAnalyzer. In case there were any samples with really bad quality, I could then replace them with different samples or decide to reduce my sequencing experimental design. Looking at the [sample concentrations](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/2018-10-09-Broodstock-DNA-Extractions/2018-10-09-DNA-Extraction-Results.csv), I decided to create 1:10 dilutions so the input DNA would be less than 10 ng. I then went to load the samples on a BioAnalyzer chip. I had to discard two chips in the preparation process: the syringe popped open before the one minute mark with the first chip, and I ran out of the gel dye matrix when working on the second chip. I needed 9 µL more of the gel dye matrix, but I wasn't sure if I could preserve the gel dye matrix in the fridge or if it would be okay while it sat outside for 45 minutes as I created a new matrix. To be safe, I tossed that chip.

I prepared a gel dye matrix with the new reagents that got delivered on Friday, and vortexed the matrix for 15 minutes at 2300 rcf. Then, I prepared and ran the chip! I'm 2/2 on successful chip runs today.

![Capture](https://user-images.githubusercontent.com/22335838/89466099-11789400-d728-11ea-9aee-d52c812a470d.PNG)

![Capture2](https://user-images.githubusercontent.com/22335838/89466101-13425780-d728-11ea-9f4d-70894d8df5f4.PNG)

**Figures 1-2**. Electopherogram and gel for *C. gigas* gonad DNA samples.

Most samples have peaks around 2000-3000 bp. Sample UK-04's peak is ~1000 bp, so I'm unsure if that will be an issue. I'll continue with the [isopropanol preciptation](https://yaaminiv.github.io/WGBS-Samples/) for 07-12, 11-T4, UK-02, UK-03, and UK-04, and then check quality again.

### Going forward

1. Complete isopropanol precipitations
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
