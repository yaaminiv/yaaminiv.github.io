---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 44
tags: green-crab-cold mixed-effects-models
---

## Investigating impact of demographic variables

We got reviewer comments back! Overall most of them are targeted at the text itself, and not any analyses. In any case, Carolyn and I both agree that the modeling framework should be revised, so that's what I'm doing.

Here are the scripts I'm working with:

- [Catlin's experiment](https://github.com/yaaminiv/cold-green-crab/blob/main/code/mult-pop-heat/03-TTR-analysis-mult-pop-heat.Rmd)
- [Julia's experiment](https://github.com/yaaminiv/cold-green-crab/blob/main/code/repeat-heat/03-TTR-analysis-julia.Rmd)
- [MA experiment](https://github.com/yaaminiv/cold-green-crab/blob/main/code/MA/03-TTR-analysis-MA.Rmd)
- [WA experiment](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/03-TTR-analysis-WA.Rmd)

For all scripts, I did the same three-part analysis:

1. Use a mixed-effects model and LRT analysis to determine if any demographic variables (sex, integument color, carapace width, weight, missing swimmers) significantly impacted righting response
2. Include only significant demographic variables in a model to understand impact of explanatory variables (time, temperature, their interaction) on righting response to avoid overfitting
3. Include only significant explanatory variables and demographic variables in a model to understand impact of genotype (genotype, presence of the C allele, presence of the T allele)

The results were the same for all populations: the demographic variables had no significant effect on righting response! None of the explanatory variable results changed, except that genotype is no longer significant for the ME population in Catlin's experiment after multiple correction. Looks like I have three "near miss" impacts of genotype on phenotype, which is going to make streamlining the discussion a bit easier (I hope).

All output can be found here:

- [Catlin's experiment](https://github.com/yaaminiv/cold-green-crab/tree/main/output/mult-pop-heat)
- [Julia's experiment](https://github.com/yaaminiv/cold-green-crab/tree/main/output/repeat-heat)
- [MA experiment](https://github.com/yaaminiv/cold-green-crab/tree/main/output/MA)
- [WA experiment](https://github.com/yaaminiv/cold-green-crab/tree/main/output/WA)

I learned two things during this analysis:

1. I updated my R and R Studio to the latest versions before reworking my code. In the new version, `pairs` is no longer needed to look at contrast information from `emmeans`.
2. If there is a coding error that says two models were fit to different datasets, that means that there is a missing value somewhere in your dataset that is messing with you. Turns out there were a handful in the [MA dataset](https://github.com/yaaminiv/cold-green-crab/blob/main/data/MA/time-to-right-MA-clean.csv), which I was able to fix.

Based on the results (and Reviewer 2's suggestion to highlight genotype-specific results for the cold temperature experiment), I revised some figures.

![Image](https://github.com/user-attachments/assets/6394271e-1526-4843-8d49-99edac234123)

![Image](https://github.com/user-attachments/assets/f5ce4f35-9183-45fa-be22-df5968276eae)

![Image](https://github.com/user-attachments/assets/14a38cce-866d-4f2f-8f99-cbaf80c628ea)

**Figures 1-3**. Revised figures

### Going forward

1. Address reviewer comments
2. Submit the revision!
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
