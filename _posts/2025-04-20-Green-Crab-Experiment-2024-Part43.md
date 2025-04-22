---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 43
tags: green-crab-cold mixed-effects-models
---

## A quick recap of things I've revised

Just so there's a record somewhere, here's what I've done!

### Posthoc tests and modifying figures

Carolyn suggested I look into posthoc tests! It was a lot more than I had capacity to figure out so I sadly needed to use ChatGPT. The transcript can be found [here](https://chatgpt.com/share/6806dc0c-7540-8008-baba-0a2aa20c169f). A quick summary of things that I needed to do:

- Modify timepoint to be categorial for Catlin and Julia's studies since they are proxies for temperature treatments
- Run posthoc tests with `emmeans`
- Lost my mind but managed to add text and asterisk labels highlighting significant differences
- Not related, but I modified the orientation of some plots! I also added some additional annotations to the temperature plots.

<img width="1084" alt="Image" src="https://github.com/user-attachments/assets/a4c5248c-d6cf-4fa9-9e68-529a17d62daf" />

<img width="1141" alt="Image" src="https://github.com/user-attachments/assets/45159cff-fe74-4b1a-a93a-ee56fbbd1b8f" />

<img width="1150" alt="Image" src="https://github.com/user-attachments/assets/943c327d-2616-4424-9389-0016f1aab0d8" />

![Image](https://github.com/user-attachments/assets/f0840279-d93d-45b5-8fdd-1237cd234547)

![Image](https://github.com/user-attachments/assets/9d83bc6a-b06e-4ef8-a7f5-c64b244f68fa)

<img width="653" alt="Image" src="https://github.com/user-attachments/assets/57ec18ce-b15f-4b53-ad19-d4d9ba512f61" />

**Figures 1-6**. Modified plots

### SST + map plot

Carolyn pulled down SST data for me and roughly plotted the data. I took that code, modified it, and figured out how to make a nice map with `ggplot` [in this script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/temperature-conditions/Cm-SG-ICB_SST_plot.Rmd)! I was very annoyed that I couldn't get the two plots to be the same size when I was sticking them together with `patchwork` and `cowplot`. But whatever. It's done.

<img width="544" alt="Image" src="https://github.com/user-attachments/assets/49ed9ea2-52cc-423e-8b07-3e8e573d2f73" />

**Figure 7**. Study site map and SST

### Supplementary material

I also tackled the supplementary material, particularly Julia's respirometry analysis. She sent me [her code](https://github.com/yaaminiv/cold-green-crab/blob/main/code/repeat-heat/04-respirometry-analysis-julia.Rmd) which I modified. While she imported data for both timepoints, she fully processed the data only for the final timepoint! I decided to forge ahead with the only final timepoint because I think it would answer the main question of how priming alters physiological responses.

<img width="1150" alt="Image" src="https://github.com/user-attachments/assets/a1a7aba3-d675-4aea-8181-3f3434028c08" />

<img width="1138" alt="Image" src="https://github.com/user-attachments/assets/e6afd06e-10b1-4301-b2f3-ee8eff26d020" />

**Figures 8-9**. Supplementary figure

### Going forward

1. Finalize manuscript
2. Submit the paper!
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
