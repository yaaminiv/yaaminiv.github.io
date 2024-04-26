---
layout: post
comments: true
title: Green Crab Experiment Part 28
tags: green-crab metabolomics lipidomics
---

## Investigating specific metabolite and lipid pathways

It's been a while since I actively analyzed my metabolomics and lipidomics data! I've messed around with a few things here and there and done a lot of reading, but let's try and play around with some data again.

### Metabolomics

I started by thinking about my not-so-problem child, my metabolomics data. Based on previous literature, I identified a few pathways of interest:

- Glycolysis and TCA cycle: The WCNA trends suggest an increase in energy production at 30ºC. My respiration data also suggests that I have more oxygen uptake in the 30ºC crabs, so it would be interesting to see if that oxygen is used to meet internal energy demands!
- Anaerobic metabolism (lactic acid fermentation): Kinda the flipside of aerobic respiration. Are energy demands so high at 30ºC that there's a need for anaerobic respiration as well?
- Calcium channel signaling: Calcium ion channels are important for muscle contraction. In cold temperatures, Mg<sup>2+</sup> can block calcium channels, preventing movement. The 5ºC crabs showed slower righting responses, but that response got slightly faster over time, so I was interested in seeing if there were molecules involved in signaling that could help be indirectly assess channel blockage.

It's clear what molecules are involved in the first two processes, but the calcium channel process is a new one for me. So, I went to the literature. Some highlights:

- [Bootman and Bultynck (2020)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6942118/)
  - Uptake of Ca2+ by mitochondrial intimately links cellular signaling to metabolism and bioenergetics: cofactor for enzymes in the TCA cycle, promotes ATP production, metabolism can boost Ca2+ signaling as well
  - IP3 important for calcium signaling


### Lipidomics

### Going forward

1. Evaluate statistical methods
2. Repeat lipidomics WCNA with temperature and day separately
4. Look into MetaboAnalyst discussion comments
5. Determine how to get more common lipid names that match MetaboAnalyst formatting
7. Try RaMP enrichment instead of KEGG for lipid sets
6. Normalize for "captivity" effect
8. Determine if I can add physiology data to `mixOmics` analyses
9. Integrate metabolomics and lipidomics data

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
