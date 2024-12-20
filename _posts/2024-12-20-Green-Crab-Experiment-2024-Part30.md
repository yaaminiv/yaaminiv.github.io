---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 30
tags: green-crab-cold
---

## Hardy-Weinberg equilibrium for MA and WA populations

Alright, time to determine if my populations are in Hardy-Weinberg equilibrium! Generally, you'd calculate the proportion of the two alleles in each population:

> p = (# CC * 2) + (# CT) / (# crabs * 2)
> q = (# TT * 2) + (# CT) / (# crabs * 2)

For sanity check, p + q should equal 1! Then, you'd use those allele frequencies to calculate the expected proportion of each genotype you'd see in the population:

> Expected proportion of CC = p^2
> Expected proportion of CT = 2pq
> Expected proportion of TT = q^2

Finally, you'd compare the expected proportions versus observed proportions using a Chi-squared test. If a population is in Hardy-Weinberg equilibrium, the observed genotype proportions would not be significantly different than the expected genotype proportions.

Of course online calculators now exist for this, so instead of doing everything by hand, I plugged in the genotype counts into [this website](https://www.cog-genomics.org/software/stats).

**Table 1**. Hardy-Weinberg equilibrium *P*-values

| **Population** |      **P-value**      |
|:--------------:|:---------------------:|
|       MA       | 0.0000071191801649826 |
|   New Bedford  |    0.02291149613108   |
|    Eel Pond    |   6.6098836727653e-7  |
|       WA       |    0.00169236548011   |

So...none of my populations are in Hardy-Weinberg equilibrium. With the MA populations, it's interesting because there's such a low instance of TT crabs any way you slice the population:

<img width="972" alt="Screenshot 2024-12-20 at 11 30 20 AM" src="https://github.com/user-attachments/assets/27d4ca62-8fad-4824-af98-ffb653267900" />

<img width="972" alt="Screenshot 2024-12-20 at 11 30 33 AM" src="https://github.com/user-attachments/assets/80d6709a-0d8b-4c98-a3ad-26c1ba0c42c2" />

<img width="972" alt="Screenshot 2024-12-20 at 11 30 42 AM" src="https://github.com/user-attachments/assets/58f5bfd2-94dc-4b4d-b07b-60eaabb26828" />

**Figures 1-3**. Genotype pie charts for the full MA population, New Bedford, and Eel Pond

With the WA population, I have a low instance of CC crabs, but I haven't finished genotyping so the jury is still out. If I remember correctly, Julia's crabs were in Hardy-Weinberg equilibrium, but she also picked her crabs based on genotype to have a balanced experimental design.

<img width="972" alt="Screenshot 2024-12-20 at 11 30 54 AM" src="https://github.com/user-attachments/assets/0daffde8-68a6-482f-a994-f7360fe8c6a0" />

**Figure 4**. Genotype pie chart for the WA population

### Going forward

1. Finish genotyping WA samples
1. Troubleshoot QC gel discrepancies by rerunning MA CC samples
2. Redo all Chelex samples?
2. Identify samples for confirmation sequencing
3. Determine methods for comparing population responses
3. Troubleshoot lipid assay protocol
5. Conduct lipid assay for crabs of interest

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
