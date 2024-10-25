---
layout: post
comments: true
title: Green Crab Experiment Part 38
tags: green-crab metabolomics lipidomics
---

## Integrating all analyses

The last thing I want to do with analysis is integrate 1) the metabolomics and lipidomics data and 2) the physiological and -omics data.

### Paper notes

The first thing I did was parse through literature to figure out acceptable methods for these endeavors. When it comes to correlating physiological data with the -omic data, I think the WCNA is the best option. In addition to the qualitative treatment data, I can add average TTR or oxygen consumption data for an individual crab on day 3 or 22 before it was dissected. The answer wasn't as clear for integrating the metabolomics and lipidomics data. Once again I would like to note that I am annoyed studies don't publish their code or write methods in a way that it is easily reproducible!

Options for integrating different -omic analyses:

- Toss metabolites and lipids into the same WGCNA
  - We know that the same factors don't impact abundance
  - Detected using very different methods so is it appropriate to mash data?
- WGCNA module correlation through WGCNA tutorial requires the same genes to be identified in both networks
  - https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3024255/
  - https://www.biostars.org/p/301712/
  - https://web.archive.org/web/20170829062156/https://labs.genetics.ucla.edu/horvath/CoexpressionNetwork/WORKSHOP/2013/Miller/Miller1_ComparingMouseAndHumanBrain.pdf
  - I think this makes more sense when you consider the comparisons made in each of the papers. They are comparing information from different studies, so they would need the same molecules to allow that comparison to occur. That's not the use case I'm thinking of.
- Associating different modules with different input data types seems possible when looking at literature
  - Correlate modules and make a similar correlation heatmap to the one WGCNA produces when examining different -omic responses in the same samples
  - Lipidomics and proteomics: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7504646/
    - Identify modules associated with the same phenotype
    - Pearson correlation for all samples and samples only in a particular phenotype
    - Use Bonferroni correction
  - Transcriptomics and metabolomics: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6689076/
    - Calculate correlation and p-value between eigenvectors of two modules
    - Use integrated pathway analysis (IMPaLA) to identify pathways simultaneously manipulated at transcript and metabolite level
  - Both of the papers above have nice correlation matrix figures that I would like to reproduce
- Alternative analytical options
  - DIABLO with `mixOmics`: https://mixomicsteam.github.io/mixOmics-Vignette/id_06.html
    - Made to integrate multi-omic data
    - Can identify correlations between individual metabolites and lipids and relate that to abundance in different treatment conditions.
    - Plotting options: circos plot, network plot
  - OmicsNet: https://academic.oup.com/nar/article/50/W1/W527/6593602
    - GUI that will conduct 3D correlation analysis of any input molecular data. Looks cool, but I don't know how much I want to add yet another analysis instead of working through what I already have.
    - Seems to also have a 12-15 sample minimum per treatment, whereas I have 6 samples minimum per treatment

### The plan

Coindentally, I had a meeting with Ariana scheduled today! We talked through different options for integration analysis and I settled on the following plan:

1. Correlate metabolomic and lipidomic data with existing WCNA modules
  - Map lipids to day 22 metabolite modules, since only the day 22 contrasts provided any VIP
  - Use eigengene values from the `module_expression.csv` tables created in each analysis
2. Add TTR and oxygen consumption data into existing WCNA analyses
3. Use DIABLO to identify potential correlations between individual metabolites, lipids, and phenotypic data

### Going forward

1. Integrate metabolomics and lipidomics data
2. Integrate physiology and -omics data
3. Address comments on paper draft
4. Outline discussion
5. Outline abstract

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
