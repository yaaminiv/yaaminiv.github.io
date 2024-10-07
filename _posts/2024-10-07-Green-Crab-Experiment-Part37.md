---
layout: post
comments: true
title: Green Crab Experiment Part 37
tags: green-crab metabolomics lipidomics
---

## Digging into enrichment data

It's time to come up with my credible conspiracy theories for why I see the enrichment patterns I do!

### Metabolomics

The only contrast with enriched processes was 13ºC vs. 30ºC at day 22, where valine, leucine, and isoleucine metabolism were enriched. I went through a bunch of papers to understand this, and added my notes and citations to the draft manuscript. Valine, leucine, and isoleucine are branched chain amino acids. Overall, BCAA are responsible for the general health of muscles, stress response, and energy production. Leucine is also involved in the mTOR signaling pathway, which is important for immune response and cell metabolism. Several crustacean stress response studies have found enrichment of this process or alternation to abundance of these metabolites in response to stress. For example, ammonia-tolerant shrimp have lower levels of BCAA when compared to ammonia-sensitive shrimp, likely due to increased energy demand for maintaining homeostasis. Sea cucumbers exposed to heat stress conditions demonstrate upregulation of branched-chain aminotransferases (BCAT), which are responsible for breaking down BCAA. Similarly, I see reduced abundance of valine, leucine, and isoleucine in my data, where day 3 at 30ºC has the lowest abundance, with a slight increase at day 22. Taken together, I think this means that energy demand is outpacing supply. If crabs require additional energy to maintain homeostasis at 30ºC due to quicker righting response and higher oxygen consumption rates, then they need more energy from BCAA.

When comparing 13ºC vs. 5ºC at day 22, there were no enriched processes. The top process was arginine biosynthesis. However, arginine was not detected in any of my samples! Arginine is important for protein catabolism. When proteins are broken down for energy, it releases ammonia that can be harmful for the cell. Arginine is used to "clean up" the harmful ammonia. Interestingly, protein catabolism releases heat in addition to energy. Could arginine biosynthesis suggest that protein catabolism would be important not only for energy, but for maintaining internal heat to an extent? Several crustacean papers examining cold tolerance suggest that arginine is commonly converted into ornithine, which is then converted into proline, an cryprotectant. However, crabs at 5ºC at day 22 had lower levels of ornithine and proline abundance, suggesting that proline is not being used as a cryoprotectant. However, proline can be converted into alanine to release 14 ATP, and there was elevated abundance of alanine at 5ºC! Since crabs at 5ºC weren't eating, potentially protein catabolism and this conversion from arginine to alanine could be providing necessary energy.

The last contrast I examined was day 3 vs. day 22 for 5ºC. Again, no processes were enriched, but the top process was valine, leucine, and isoleucine degradation. Given what I know about BCAA and their importance for energy production in stressful elevated temperatures, it is possible that the top process being BCAA degradation indicates a shift from BCAA to arginine-associated energy production.

### Lipidomics

The lipidomics story is a lot more straightforward! It's all about manipulating glycerophospholipids to ensure proper bilayer fluidity and permeablility. There are several papers that temperature induces rapid changes in cell membrane fatty acid composition. This is supported by my work, since only temperature was a defining factor in membrane composition. The top term for both the 13ºC vs. 30ºC and 13ºC vs. 5ºC contrast was "endoplasmic reticulum." I think this is showing up because the ER itself is made up of a continuous glycerophospholipid bilayer! When looking at PC and PE VIP, I noticed that changes in VIP abundance seemed to follow the overall theory that lipids with double bonds were needed for fluidity at 5ºC and single bonds were needed for structure at 30ºC. I'll need to eventually quantify this so it's easier to digest.

### Going forward

1. Determine if I can add physiology data to `mixOmics` analyses
9. Integrate metabolomics and lipidomics data
3. Address comments on paper draft
4. Outline discussion
5. Outline abstract

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
