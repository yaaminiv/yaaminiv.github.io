---
layout: post
comments: true
title: Green Crab Experiment Part 36
tags: green-crab metabolomics
---

## Pairwise metabolomics enrichment

Time to continue revising enrichment analyses so they're pairwise! I conducted my analysis in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/05-metabolomics-analysis.Rmd). All enrichment output can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/metaboanalyst) and figures [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/metaboanalyst/figures).

<img width="1117" alt="Screenshot 2024-09-13 at 1 37 34 PM" src="https://github.com/user-attachments/assets/93cbfb0e-9689-498d-a61f-2e14b33ad4f5">

<img width="1117" alt="Screenshot 2024-09-13 at 1 37 44 PM" src="https://github.com/user-attachments/assets/6ec8a6ac-0cc8-42fb-a005-b867494b16a6">

<img width="1117" alt="Screenshot 2024-09-13 at 1 37 53 PM" src="https://github.com/user-attachments/assets/64c22a55-7ea7-480b-8589-ad8af1f326e8">

**Figures 1-3**. Enrichment results for metabolomics pairwise VIP

Turns out the only comparison with enriched pathways is 13ºC vs. 30ºC at day 22! The other comparisons don't have any enriched pathways, but that may be because there are fewer VIP.

I also made some barplots to visualize the metabolite abundance differences:

<img width="1146" alt="Screenshot 2024-09-13 at 1 40 01 PM" src="https://github.com/user-attachments/assets/0abeba76-4f8e-429e-a1d4-bf9b2b4eb0ae">

<img width="1146" alt="Screenshot 2024-09-13 at 1 40 12 PM" src="https://github.com/user-attachments/assets/a690d6ca-1424-49d0-b417-fcae5e231e35">

<img width="1146" alt="Screenshot 2024-09-13 at 1 40 20 PM" src="https://github.com/user-attachments/assets/e61fd9dd-35c8-4404-9bc1-f1ac9f08dd4c">

**Figures 4-6**. Bar plot of pairwise VIP abundances

### Going forward

1. Update methods
2. Update results
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
