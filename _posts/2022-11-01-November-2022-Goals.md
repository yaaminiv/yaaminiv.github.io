---
layout: post
comments: true
title: November 2022 Goals
tags: goals
---

<img width="467" alt="Screen Shot 2022-09-13 at 2 52 14 PM" src="https://user-images.githubusercontent.com/22335838/200045212-73b51791-e23f-4785-842c-33a20a72e675.png">

Fall is in full swing! I focused on analyses for different conference presentations this month.

### [October Goals Recap](https://yaaminiv.github.io/October-2022-Goals/)

**Virginica Methylation and Expression**:

- [Conducted SPLS analysis for gene methylation and expression without tuning](https://yaaminiv.github.io/CEABiGR-Part7/)
- [Conducted predominant isoform ad transcriptional noise analyses](https://yaaminiv.github.io/CEABiGR-Part8/), and found some interesting results!
- Presented results at EPIMAR!

**Killifish Methylation and Expression**:

- [Reviewed methylation landscape analysis](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part22/)
- Didn't test `BAT_DMRcorrelating` to match RRBS and RNA-Seq data because only one DMR was located in a gene, and that gene was not differentially expressed
- I was missing information to conduct the pathway analysis, so I looked at HIF and AHR pathways instead
- Tracked down SNPs information for populations, but couldn't find pre-existing SNP information for Scorton Creek
- Gave a talk at the APS Comparative Physiology Conference!

**Green Crab Pilot Experiment**:

- Emailed Sara and Mikayla to determine which parts of the analyses they were interested in working on

**Other**:

- [THE OCEAN ACIDIFICATION AND REPRODUCTION REVIEW IS PUBLISHED!](https://doi.org/10.3389/fmars.2022.977754)

### November Goals

This month, I want to focus on postdoc projects so I can work towards getting a publication ready soon.

**Coral Transcriptomics*:

- Run a WGCNA
- Dig into gene functions for assigned categories
- Update methods and results

**Killifish Methylation and Expression**:

- Continue BAT workflow with new genome
- Conduct formal pathway analysis for RNA-Seq data
- Review existing lab data
- Determine protocol for SNP identification
- Conduct in silico RRBS digestion
- Characterize DEG that overlap between populations
- Quantify gene expression variability
- Visualize DEG data

**Green Crab Pilot Experiment**:

- Analyze survivorship data by treatment and sex
- Analyze time-to-right data by treatment, sex, weight, and integument color
- Update methods and results
- Determine workflow for respiration data analysis
- Determine workflow for metabolomics sample processing

**Other**:

- Review supplementary material for eelgrass transcriptomics paper
- Distribute results of postdoctoral mentorship experiences survey
- Launch the postdoctoral mentoring program

### Deep Freeze

Things that are not immediate enough to be on the backburner, so they're stuck in the deep freeze.

**Hawaii Gigas Methylation**:

I will revisit this paper in December.

- Address Rajan's edits
- Return to `methylKit` for better interpretation
- Extract SNPs with `EpiDiverse` and create a relatedness matrix
- Look at methylation islands and non-methylated regions
- Examine overlaps between DML and other epigenomic datasets from Sascha

**Zebrafish DNMT**:

I'll think about this once I have a robust killifish paper draft.

- Identify a stress condition to use for the zebrafish based on disease factors or conditions that are known to induce changes in methylation
- Determine physiological endpoints to assess for embryos
- Construct a project timeline

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
