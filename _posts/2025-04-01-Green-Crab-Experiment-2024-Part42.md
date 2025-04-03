---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 42
tags: green-crab-cold mixed-effects-models
---

## Analyzing Catlin's data

Let's get right to it.

### Temperature data

I analyzed the temperature data in [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/mult-pop-heat/01-temp-conditions-mult-pop-heat.Rmd). HOBO data can be found [here](https://github.com/yaaminiv/cold-green-crab/tree/main/data/mult-pop-heat/HOBO). All temperature calculations and statistical output can be found in [this folder](https://github.com/yaaminiv/cold-green-crab/tree/main/output/mult-pop-heat/01-temp-conditions). Some notes because I'm too tired to do this any other way:

- WA HOBO loggers were put in during the heat ramp so there was no acclimation information. However, the tanks were kept on the counter without any crabs for a few days after the experiment so I can use that to extrapolate acclimation temperatures.
- ME had acclimation and ramp information.
- THERE WAS NO NH HEAT SHOCK! When I looked at the temperature data, there was maybe a brief heat ramp but it came down pretty quickly. I will use the NH TTR data from timepoint 0 and use it just for parasite and genotype analysis.

![Image](https://github.com/user-attachments/assets/b2a00515-4772-4e53-a375-5952166c4224)

**Figure 1**. Temperature data

### TTR data

I re-analyzed Catlin's TTR data in [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/mult-pop-heat/03-TTR-analysis-mult-pop-heat.Rmd) using similar mixed effects model methods to every other experiment. A couple of notes up top, then I'll just get into results.

- The parasite data Catlin sent for NH is actually ME data! I emailed him to ask for the NH data, so I'll revise that when I get to it
- Carolyn suggested I calculate a different parasite intensity for each parasite species by dividing parasite count by carapace width. This will really only come into play for the NH data since ME crabs were all infected by metacircarial cysts
- For ME, I included metacircarial parasite intensity in the model and tested its significance. I will do a similar thing with NH crabs once I have the correct parasite counts.
- All statistical output can be found in [this file](https://github.com/yaaminiv/cold-green-crab/blob/main/output/mult-pop-heat/03-TTR-analysis/ttr-model-comparison-stat-output.csv)
- Average TTR by population at each timepoint can be found in [this file](https://github.com/yaaminiv/cold-green-crab/blob/main/output/mult-pop-heat/03-TTR-analysis/average-TTR-calculations.csv)
- Average TTR by timepoint and genotype for each population can be found in [this file](https://github.com/yaaminiv/cold-green-crab/blob/main/output/mult-pop-heat/03-TTR-analysis/average-TTR-calculations-genotype.csv)
- All exploratory figures can be found [here](https://github.com/yaaminiv/cold-green-crab/tree/main/output/mult-pop-heat/03-TTR-analysis/figures)
- All three populations were in Hardy-Weinberg equilibrium?!
- Only CC and CT crabs were tested in ME and NH. I'll need to delineate between pre-experiment mortality and during experiment mortality in the results.

Alright! As expected, time, but not genotype, presence of the C allele, nor presence of the T allele significantly impacted righting response in WA crabs. Crab ID accounted for < 1% of variance. Time and genotype impacted righting response in ME crabs, with crab ID explaining 27.82% of the variance in the final model. Based on these results, I made a multipanel figure that showed righting response before and after heat stress in WA and ME. I also created a supplementary figure for the NH data.

![Image](https://github.com/user-attachments/assets/62a10f21-7aa8-47dc-a46a-d2e3a07312d7)

**Figure 2**. Righting response figure

![Image](https://github.com/user-attachments/assets/85625777-0b8b-4a0a-aac9-97adefaab12e)

**Figure 3**. NH supplementary figure

There was a marginal impact of population on average TTR when I analyzed all the data together.

.....I think I just have to right now?!

### Going forward

1. Revise NH parasite data analysis
1. Add Catlin's methods and results to manuscript
2. Outline introduction
3. Outline discussion
4. Write introduction
5. Write discussion
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
