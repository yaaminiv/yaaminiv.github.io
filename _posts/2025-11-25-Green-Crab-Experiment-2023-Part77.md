---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 77
tags: green-crab-wc metabolomics PLSDA ASCA sPLS
---

## Wrapping up PLSDAs and ASCAs

Before I do any VIP analysis, I want to use ASCA to decompose the main temperature x time and temperature x genotype trends in the dataset. I'll then use the combined insight from the ASCAs and PLS-DAs to inform my VIP analysis workflow. Code is [here](https://github.com/yaaminiv/wc-green-crab/blob/main/code/05-metabolomics-analysis.Rmd), and all output is in [this folder](https://github.com/yaaminiv/wc-green-crab/tree/main/output/05-metabolomics-analysis/PCA).

### PLSDA

I [previously ran PLSDAs using genotype within temperatures](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part75/). While this was informative and based on perMANOVA results, I wonder if looking at both temperatures simultaneously will increase my sample size and allow me to pull out some robust trends. I focused on using the actual genotype instead of allele presence, since I would expect CC and CT crabs at 5ºC and CT and TT crabs at 25ºC to group together.

<img width="1791" height="1392" alt="Image" src="https://github.com/user-attachments/assets/7bd19ed5-ec6d-4fc4-ac52-3461b497a3a0" />
<img width="1806" height="1384" alt="Image" src="https://github.com/user-attachments/assets/c262cab0-c626-488a-869a-f1db4e123d43" />

**Figures 1-2**. PLSDA and sPLSDA for temperature and genotype.

In both the PLSDA and sPLSDA, I saw a strong effect of temperature along PC1. PC2 appeared to be the one discriminating the genotypes. At 5ºC, the CC crabs separated from the CT and TT crabs pretty strongly. I'm surprised this wasn't impacted too much by the putative outliers I saw in the PCA. Although, since removing that outlier didn't change the results, perhaps it shouldn't be surprising at all. At 25ºC, the separation between TT and other crabs is a bit clearer in the sPLSDA than the PLSDA. This was surprising to me since the TT and CC/CT crabs had very clear separation in the 25ºC-only PLSDA. One thing I noticed with the sPLSDA is that using 4 components had much lower BER than 2 components, but `mixOmics` still pushed 2 components as the best option (I think based on a slightly lower error bar). I still ran my sPLSDA with 4 components due to the overall lower BER and ability to use more variables. However, I also noticed that when I did this, the percent variance explained by PCs 3 and 4 were somehow higher than PCs 1 and 2! For this reason, I stuck to only looking at PCs 1 and 2. Ariana suggested making a scree plot, but a quick search seemed to suggest that scree plots are more appropriate for PCAs as opposed to PLSDAs.

### ASCA

My next (arguably more interesting) step was to use ASCAs. My goal was to use these to identify major trends in the data. I could then use the trends present in both the ASCA and PLSDA to guide VIP identification for pairwise trends. I figured because I have a lot of potential pairwise comparisons that this would be a conservative and useful approach to narrow those options down.

To run the ASCA for temperature x time, I initially thought of using both crab ID and genotype as random effects. However, I later settled on only using crab ID. Crab ID as a random effect would encapsulate both variation in individual genotype, as well as other factors such as sex and weight that could be important for including. I didn't want to use genotype as a random effect as well to avoid overfitting the model. The ASCA for temperature x time consistently showed a difference between early and late timepoints. PCs 1 and 2 explained more than 10% of the variance, with PC 1 depicting metabolites with higher abundance at 25ºC that decrease over time, and PC2 depicting metabolites that increase over time with higher abundance at 25ºC. Although PC 3 explained < 10% of the variance (~9.4%), it depicted metabolites that had higher abundance in one temperature earlier, and higher abundance in the other temperature later. I thought this flip-flopping trend was interesting, even though the metabolites weren't as strongly loaded onto the PCs and some of the prediction plots were messy. I may choose to investigate this in the future.

<img width="1083" height="1400" alt="Image" src="https://github.com/user-attachments/assets/ff14e66d-bef6-4905-aa45-cb14e6b917e5" />
<img width="1074" height="1399" alt="Image" src="https://github.com/user-attachments/assets/a2461306-7a4a-4966-813c-35755dfadf34" />
<img width="1076" height="1401" alt="Image" src="https://github.com/user-attachments/assets/33590c61-b8a6-40e8-8c05-cf3d68ac5dad" />

**Figures 3-5**. Effect plots for PCs 1-3 for temperature x time investigation.

I then did the ASCA for temperature x genotype. PC1 showed metabolties with higher abundance at 25ºC that also had slightly higher abundance in TT crabs at 25ºC. PC2 showed metabolites with higher abundance in CC crabs at 5ºC, and lower abundance in CC crabs at 5ºC. This PC matches up quite nicely with the separation of CC crabs I saw in the PLSDA. PC3 showed metabolites with higher abundance in TT crabs at 5ºC, and higher abundance in CT crabs and lower abundance in TT crabs at 25ºC. PC3 is definitely harder to intepret at the warmer temperature, especially because metabolites aren't as strongly loaded onto the PC.

<img width="1080" height="1397" alt="Image" src="https://github.com/user-attachments/assets/7feb7805-ec22-4b2a-9a94-739671744e87" />
<img width="1075" height="1398" alt="Image" src="https://github.com/user-attachments/assets/1101a3d2-6201-4236-a0e9-fdffc5f8f0ce" />
<img width="1076" height="1392" alt="Image" src="https://github.com/user-attachments/assets/6f53750d-190f-4250-ab30-75139731a300" />

**Figures 6-8**. Effect plots for PCs 1-3 for temperature x genotype investigation.

Based on both the PLSDA and ASCA, it seems like there is definitely an impact of "allelic mismatch" on the overall metabolome. This is interesting to see since I wouldn't expect differences in the supergene region to translate to whole metabolome differences, but this could be a product of different metabolome interactions that will be hard to quantify until I can match specific DEGs to metabolic pathways. There also is a strong separation between early and late timepoints. I think I'll focus on pulling out differences between these higher-order groups with the VIP analysis.

### Going forward

1. VIP analysis for major trends
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
