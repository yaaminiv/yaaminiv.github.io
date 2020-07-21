---
layout: post
comments: true
title: Sperm DNA Extractions Part 5
tags: virginica labwork sperm WGBS
---

## Requantifying *C. virginica* sperm DNA extractions

I thought I was done with my sperm DNA extractions, but turns out I did my Qubit quantifications incorrectly (thanks for reading my notebook and pointing that out Sam). I was pipetting 200 µL of the master solution into each Qubit assay tube, when I should have had 200 µL total (master solution AND sample) in the assay tube. I popped into the lab to quickly finish this off!

### Quantificiation

**Step 23**. Obtain dsDNA BR standards from the fridge. Label assay tubes for each sample and two standards.
  - There were some off-brand assay tubes next to the Qubit. I figured they were for use with the Qubit, so I used them for some of my samples once I was out of Qubit brand tubes.

**Step 24**. Prepare the master solution, using a 1:200 ratio of dye to dsDNA BR buffer. Each standard and sample needs 200 µL of solution.
  - I had two standards and eleven samples, so I needed 2600 µL of solution.
  - I used 2786 µL buffer and 14 µL dye (2600 µL solution * 0.5 / 100 = 14 µL dye; 2800 µL solution - 14 µL dye = 2786 µL buffer).

**Step 25**. For each standard, pipet 190 µL master solution and 10 µL of the standard into the assay tube.

**Step 26**. For each sample, pipet 199 µL master solution and 1 µL of the sample into the assay tube.

**Step 27**. Use Qubit to quantify yield

### Results

**Table 1**. Sample ID, concentration, and total DNA yield. Standard 1 read at 257.82, and Standard 2 at 26678.08. Asterisks indicate the sample tube used was not a Qubit brand tube.

| **Sample ID** | **DNA Concentration (ng/µL)** | **Final Volume (µL)** | **Total DNA Yield (ng)** |
|:-------------:|:-----------------------------:|:---------------------:|:------------------------:|
|   L18A0006s   |               96              |           44          |           4224           |
|   L18A0007s   |              432              |           43          |           18576          |
|   L18A0009s   |              46.4             |           44          |          2041.6          |
|   L18A0012s*  |              73.6             |           44          |          3228.4          |
|   L18A0013s   |              186              |           44          |           8184           |
|   L18A0023s   |              344              |           43          |           14792          |
|   L18A0031s   |              9.24             |           44          |          406.56          |
|   L18A0048s*  |              25.8             |           43          |          1109.4          |
|   L18A0057s*  |              163              |           44          |           7172           |
|   L18A0059s*  |              55.6             |           44          |          2446.4          |
|   L18A0063s*  |              99.4             |           44          |          4373.6          |

### Going forward

1. Prepare these samples for whole genome bisulfite sequencing!

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
