---
layout: post
comments: true
title: Gonad Histology Update 4
tags: manchester histology
---

## Another classification revision

I met with Brent last week to go through my histology classifications. He recommended that I review the some specimens with the microscope. 

### Retaking histology images

I read another paper he recomended, Coe 1932, to understand the differences between primary ovogonia and spermatagonia. I read the paper, as well as some guides on tissue types from Carolyn Friedman, then had some dedicated microscope time. Here are the revised classifications and some notes. The classifiation spreadsheet can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/2018-02-27-Gigas-Histology-Classification.csv), and the new images I took can be found [here](https://github.com/RobertsLab/project-oyster-oa/tree/master/images/Manchester/Gigas-gonad-histology/2017-02-04-Sampling/2018-03-13-Preexperiment-Retakes) for pre-experiment sampling and [here](https://github.com/RobertsLab/project-oyster-oa/tree/master/images/Manchester/Gigas-gonad-histology/2017-04-08-Sampling/2018-03-13-Postexperiment-Retakes) for post-experiment sampling. Shoutout to Grace for helping me set up the iPhone + microscope contraption!

- Gigas_02: Stage 1 Female. Need to verify that it's not male. Ovogonia are closer to the walls of the acini, so I'm pretty sure it's female
- Gigas_04: Same as above
- Gigas_05: Looking at the primary sex cells, they're farther away from the walls of the acini. This may be male? There's also a chance that I didn't find any acini and I'm looking at the digestive gland or intestines instead. Need to clarify with Brent.
- Gigas_06: Stage 1 Female
- Gigas_07: No acini structure, so Stage 0
- Gigas_08: Same as above
- Gigas_10: Same as above
- Gigas_15: Found some spermatazoa! No acini structure, so it's a spent oyster. Stage 4 Male!
- Gigas_18: No acini structure, so Stage 0
- Gigas_20: Same as above
- 4-T3: Same as above
- 5-T3: Same as above
- 9-T2: Same as above
- 10-T3: Same as above
- 12-T6: Spermatazoa but no acini structure. Stage 4 Male
- UK-03: No acini structure, so Stage 0
- UK-05: Spermatazoa but no acini structure. Stage 4 Male

### Gonad maturation analyses

Not much changed from my [previous analysis](https://yaaminiv.github.io/Gonad-Histology-Update3/). Sex is still the only factor that explains differences in maturation. However, I now have different mature and immature classifications between treatments. None of my low pH animals were mature. In my ambient pH treatment, six individuals were immature and four were mature. When I was building my binomial GLM using stepwise regression methods, I got a p-value of 0.03 when I had a Mature ~ Treatment model. However, Sex was a more significant factor, so I added that in first to create my base model. When I used `add1` to identify which covariates to add, none of them were significant! Guess I don't have to change the story in my NSA presentation.

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
