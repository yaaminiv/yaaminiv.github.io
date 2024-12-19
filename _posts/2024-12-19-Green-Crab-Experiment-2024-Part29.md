---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 29
tags: green-crab-cold TTR mixed-effects-models
---

## Preliminary WA TTR analysis with genotype

After mapping out the workflow with the [MA data](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part28/), I'm going to do the same thing with the subset of genotype data I have for my WA populations. I have genotypes for roughly half of the WA data, but importantly I have genotypes for the WA crabs alive through the end of the experiment. All analyses were done in [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/03-TTR-analysis-WA.Rmd).

### Adding genotype to the average TTR model

- Time, treatment, and their interaction were significant in the overall model
- Genotype and the presence of specific alleles was not significant for average TTR
- Statistical output can be found [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/ttr-model-comparison-stat-output.csv)

### Genotype in the threshold analysis

- Time and treatment were significant in the overall model, but not interaction
- Genotype and the presence of specific alleles was not significant for average TTR
  - This is surprising since the presence of the T allele was significant for the MA population! My guess is that this could be due to differences in genotype makeup in the population: MA had more CC and CT with few TT, while WA has a lot of CT with some TT and very little CC (so far)
  - There was also fewer instances of crabs not flipping for the WA population
  - I made lots of plots examining genotype-specific effects just for fun and consistency with the MA population
- Statistical output can be found [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/ttr-binomial-model-comparison-output.csv)

<img width="1360" alt="Screenshot 2024-12-19 at 3 13 35 PM" src="https://github.com/user-attachments/assets/c5b2605d-e763-40b3-91ec-165f655e2226" />

<img width="1360" alt="Screenshot 2024-12-19 at 3 13 51 PM" src="https://github.com/user-attachments/assets/be03debf-9da6-4559-b826-a1923880585c" />

<img width="1360" alt="Screenshot 2024-12-19 at 3 13 59 PM" src="https://github.com/user-attachments/assets/29db0dfa-847b-4d2f-9ba0-f2132f210494" />

<img width="1360" alt="Screenshot 2024-12-19 at 3 14 10 PM" src="https://github.com/user-attachments/assets/bed11d05-1ca0-4429-b13d-d24114876d6b" />

<img width="1360" alt="Screenshot 2024-12-19 at 3 14 17 PM" src="https://github.com/user-attachments/assets/722d18db-ab10-466e-81ed-b65e17eff229" />

**Figures 1-5**. Binary righting response plots

### Individual-level analyses

- Calculated Q10 for 28 WA crabs that were tracked throughout the experiment
- There was no clear impact of genotype on Q10 the way there was for the MA data
- Patterns of recovery or failed recovery were not apparent by genotype

<img width="1360" alt="Screenshot 2024-12-19 at 3 15 15 PM" src="https://github.com/user-attachments/assets/26ab69de-15ab-4b91-ad32-984ee9da7763" />

<img width="1360" alt="Screenshot 2024-12-19 at 3 15 24 PM" src="https://github.com/user-attachments/assets/9379c947-2227-4be2-9e57-5d6a0fc1e030" />

<img width="1360" alt="Screenshot 2024-12-19 at 3 15 40 PM" src="https://github.com/user-attachments/assets/29aec609-6793-448f-92bd-e19e8c42864a" />

<img width="1360" alt="Screenshot 2024-12-19 at 3 15 50 PM" src="https://github.com/user-attachments/assets/e89ec1a2-2c05-4d32-b50a-a1a9f4c2ba25" />

<img width="1360" alt="Screenshot 2024-12-19 at 3 16 02 PM" src="https://github.com/user-attachments/assets/07e62782-72a1-4693-87e1-80a0ef9e1f20" />

<img width="1360" alt="Screenshot 2024-12-19 at 3 16 13 PM" src="https://github.com/user-attachments/assets/2b251a6f-d661-4188-80ba-02c499517089" />

**Figures 6-11**. Individual-level figures

### Going forward

1. Determine if MA and WA populations are in Hardy-Weinberg equilibrium
2. Genotype WA samples
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
