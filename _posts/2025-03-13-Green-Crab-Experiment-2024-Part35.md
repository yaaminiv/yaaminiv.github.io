---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 35
tags: green-crab-cold TTR mixed-effect-models
---

## Revising WA TTR data

Now that I have all my WA genotypes, I can revise my TTR analysis! All analysis was done in [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/03-TTR-analysis-WA.Rmd).

### Mixed effects models

After loading in the [revised genotype data](https://github.com/yaaminiv/cold-green-crab/blob/main/data/WA/2024-crab-genotype-WA.csv) I noticed immediately that I FORGOT TO EXTRACT A CRAB. The row for crab 152 was hidden on my Google Sheet so I missed it. RIP.

I still looked at the new distribution of genotypes in the population. The population is not in Hardy-Weinberg equilibrium (p = 0.000036240950966106)

<img width="1032" alt="Image" src="https://github.com/user-attachments/assets/1a621233-de9f-408d-8f3c-4dce2758731b" />

**Figure 1**. Genotype distribution of WA population

In any case, I used the genotype data for all but one crab to revise the stats. As expected, there was no impact of genotype on [average TTR](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/ttr-model-comparison-stat-output.csv) or [failure to right](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/ttr-binomial-model-comparison-output.csv).

### Figures

My next step was to revise all my figures!

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/a687e2c6-0748-44fe-aa2f-3e6bdc4f1fa3" />

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/233f8d30-40a8-4471-b229-c8cfcf03c6e5" />

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/d0da8e67-4204-464f-bbcb-dbfc8cbb09a5" />

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/4799cfa0-d290-49bc-bfb4-cec64bbe3355" />

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/9c7938b5-3a85-455c-a349-27764b2d5365" />

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/aa22ab21-5b8e-4149-a826-7c20bdf8983e" />

**Figures 2-7**. Various visualizations for crabs that failed to right

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/8197cea9-25b8-4d17-9474-01c88fa3b9b7" />

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/6e81b114-e099-4470-b302-3f3d1bd42e11" />

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/5de97182-86fc-44dd-97bf-453b4ce5dac8" />

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/044e6069-d72d-45e8-a883-8e5669b9719f" />

**Figures 8-11**. Various visualizations for Q10 differences for 28 crabs followed throughout the experiment.

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/0c3e399e-0216-4848-a807-7b2cd81ef360" />

<img width="866" alt="Image" src="https://github.com/user-attachments/assets/2ea7206a-7e01-4232-9e88-13a0b68ea796" />

**Figures 12-13**. Various visualizations for individual-level differences for crabs followed throughout the experiment and some extras as well.

### Discrepancies between genotype sets

In making my figures, I noticed that [previous versions](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part29/) had TT crabs failing to right, but in this version none of my TT crabs failed to right. This means that there are crabs I initially called TTs that were actually CTs. To identify these crabs, I went to a [previous genotype commit](https://github.com/yaaminiv/cold-green-crab/commit/efe15e4682be6f60495380d8aa89a758c1f74593). Crabs 154, 156, and 215 were called TTs even though the [12/10 gel had faint bands](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part27/). When they were [redone on 2/26 and 2/28](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part33/), the CT band patterns were much clearer! Looking at the 2/28 gel, I also noticed that 257 was called a CT even though the gel looks like a TT with a false bottom band based on the spacing between the two bands. I will redo this PCR when I extract WA 152.

### Going forward

1. Write methods section in manuscript
2. Add MA results to manuscript
3. Add WA results to manuscript
3. Add Catlin's methods and results to manuscript
3. Genotype Catlin's samples
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
