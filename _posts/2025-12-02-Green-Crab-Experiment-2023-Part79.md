---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 79
tags: green-crab-wc metabolomics VIP
---

## VIP analysis

Now that I finalized all my PLS-DAs, I get to identify VIP molecules! Overall workflow is to conduct a PLSDA with contrasts of interest, then to take any identified VIP and run pairwise t-tests to determine their significance.

### Early vs. late

I ran four contrasts to understand the influence of temperature and time on the metabolome:

1. Early 5ºC vs. Late 5ºC
2. Early 25ºC vs. Late 25ºC
3. Early 5ºC vs. Early 25ºC
4. Late 5ºC vs. Late 25ºC

These four trends were well-supported in the PLSDA and ASCA. I had several VIP identified for all four contrasts! The output can be found [here](https://github.com/yaaminiv/wc-green-crab/tree/main/output/05-metabolomics-analysis/PLSDA).

**Table 1**. Number of VIP for each contrast

|       **Contrast**       | **Total VIP** | **Higher in Condition 1** | **Higher in Condition 2** |
|:------------------------:|:-------------:|:-------------------------:|:-------------------------:|
| Early 5ºC vs. Early 25ºC |       69      |             1             |             68            |
|  Late 5ºC vs. Late 25ºC  |       59      |             13            |             43            |
|  Early 5ºC vs. Late 5ºC  |       44      |             38            |             6             |
| Early 25ºC vs. Late 25ºC |       47      |             43            |             4             |

I'll need to dig into pathways for these compounds when I do my enrichment tests. The fact that there are VIP with higher abundances at 25ºC and earlier on in the experiment is also consistent with what I saw in the ASCA.

### Temperature x genotype

.....there were no VIP! I found this pretty surprising, considering the clear differences between CC and CT/TT crabs at 5ºC in both the PLSDA and ASCA. The ASCA also showed some interesting trends of the CT crabs having different metabolite abundance than the other genotypes at 25ºC. Given that the ASCA and PLSDA are global tests and the pairwise t-tests are smaller-scale, I thought I would at least see some pairwise abundance differences between genotypes. Even with this surprising result, my next step is pretty clear: investigate any pathways associated with metabolites strongly loaded onto the ASCA PCs.

### Going forward

1. Metaboanalyst for VIP and ASCA compounds
3. Index, get advanced transcriptome statistics, and pseudoalign with `salmon`
4. Clean transcriptome with `EnTap` and `blastn`
4. Identify differentially expressed genes
5. Additional strand-specific analysis in the supergene region
2. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper
6. Start methods and results of 2023 paper

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
