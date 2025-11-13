---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 72
tags: green-crab-wc metabolomics
---

## PCA explorations

I'm launching into the metabolomics analysis for this project. I think it'll be mostly straight-forward since I have a good workflow from the 2022 pilot experiment. However, I have different sets of samples that I sequenced that I need to consider for my questions:

1. Are there temperature-specific differences in the metabolome?

> To answer this question, I would compare 15ºC vs. 5ºC and 25ºC, irrespective of time. This first question is more of a "sanity check" since I already know temperature has an outsized impact on the metabolome from the pilot experiment!

2. Are there different metabolic pathways activated over time in each temperature treatment?

> I can't answer this question by looking at all metabolomes, since my 15ºC control is not specific to any timepoint (control crabs were taken from throughout the experiment to use as a representative sample). Therefore, I just need to look at 25ºC and 5ºC for days 1 onwards.

3. Do different supergene genotypes have varied metabolome responses to chronic temperature exposure?

> This is where I can compare control crabs with the crabs sampled at day 42, taking into account the crab's genotype. Essentially, this would be a temperature x genotype interaction test, since the control is both day 0 and 15ºC.

I'm not going to go over the methods a bunch since they are similar to what I did for the pilot experiment. Will just highlight any challenges and the results. All output can be found in [this folder](https://github.com/yaaminiv/wc-green-crab/tree/main/output/05-metabolomics-analysis/PCA).

### Are there temperature-specific differences in the metabolome?

Unsurprisingly, yes! I found a significant impact of temperature on the metabolome for both all metabolites and known metabolites. Both centroid differences and dispersion differences were significant.

### Are there different metabolic pathways activated over time in each temperature treatment?

I did find significant impacts of temperature, time, and their interaction when looking at days 1, 7, 26, and 42. This result isn't surprising since I saw a significant impact of time and the interaction between temperature and time in the pilot experiment as well. When only at known metabolites, there were no significant dispersion differences, which suggests that any differences between groups are just due to centroid differences.

When making the PCAs, I saw the temperature trend was really strong:

<img width="1603" height="1240" alt="Image" src="https://github.com/user-attachments/assets/600a4934-7653-4551-9d56-37d2ff47f29d" />

**Figure 1**. Metabolomes over temperature and time

Metabolomes at days 1 and 7 were more similar, whereas metabolomes at day 26 and 42 were similar. It seems like the big transition happens somewhere between day 7 and 26. That inflection point between "early" and "late" metabolomes may be something to consider in downstream analyses like the PLSDA and ASCA.

### Do different supergene genotypes have varied metabolome responses to chronic temperature exposure?

I first plotted the data for all temperature treatments (5ºC, 15ºC, and 25ºC) in a PCA:

<img width="1321" height="1036" alt="Image" src="https://github.com/user-attachments/assets/ecc03910-9fab-422a-b32d-5a2ec9e9c82c" />

**Figure 2**. PCA of metabolomes by temperature and genotype

Looking at this, it seems like a lot of the variation was between 15ºC and the two experimental temperatures. Based on this, I redid the PCA looking just at 5ºC and 25ºC, which would show me if there were genotype-specific responses to temperature by the end of the experiment.

<img width="1332" height="1032" alt="Image" src="https://github.com/user-attachments/assets/1a037508-5a8d-4e73-8206-6af1a5525ac7" />

**Figure 3**. PCA of metabolomes by genotype at 5ºC and 25ºC

While there is a good amount of overlap between the genotypes, it looked like there may be dispersion differences associated with the alleles. In 5ºC, CC and CT crabs had more dispersion whereas TT crabs did not, and CT and TT crabs had more dispersion in 25ºC than the CC crabs. This is interesting considering CC are homozygous for the cold-adapted supergene and TT are homozygous for the warm-adapted supergene. So of course, I plotted the PCAs by allele presence:

<img width="1333" height="1034" alt="Image" src="https://github.com/user-attachments/assets/d66025c7-8db2-439e-b6ec-4d5c16d0bf6a" />

**Figure 4**. PCA of metabolomes by presence of the C allele at 5ºC and 25ºC

<img width="1332" height="1037" alt="Image" src="https://github.com/user-attachments/assets/a7e327e3-661f-4609-be2d-3b02b6bc6879" />

**Figure 5**. PCA of metabolomes by presence of the T allele at 5ºC and 25ºC

In hindsight, I think that the visual differences are probably due to this one CC outlier crab at 5ºC. But I didn't quite pay attention to that when I was moving through this analysis...oops. So let's proceed with my delusion, shall we?

Unsurprisingly, there was a significant impact of temperature on the metabolome, but not any genotype variable nor the interaction between a genotype variable and temperature. Womp womp. I have a couple of thoughts on my next step:

- Remove that outlier crab from 5ºC (I think it's a CC crab) and repeat the analysis. If the outlier removal changes the results, I will need to think about it more. If it doesn't, then perhaps I keep it in the dataset.
- I want to add a genotype component to the temperature x time analysis. I don't think I have a large enough sample size at each timepoint to test CC vs. CT vs. TT, but I may be able to test presence of each allele. Things to marinate on...

I can, however, move forward with doing a PLS-DA for the temperature x time question. I don't think I'm going to bother with a PLS-DA for the control vs. 5ºC or 25ºC for this preliminary analysis since I am more interested in interactive effects for this experiment.

### Going forward

1. Experiment with different methods for the temperature x genotype question PCAs
2. PLS-DA for temperature x time question
2. Create a plan for RNA-Seq analysis
3. Assemble transcriptome
4. Identify differentially expressed genes
5. Strand-specific analysis
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
