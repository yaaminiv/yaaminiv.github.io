---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 48
tags: green-crab-wc TTR respirometry sequencher genotype
---

## Results from genotyping round 2

Vanessa did an incredible job taking the new genotype data through Sequencher! I took her work from [this spreadsheet](https://docs.google.com/spreadsheets/d/1B1tyeCI7F_T-l41144m6k_MEVhhU-XCHAEkr6PHoTpw/edit#gid=0) and updated [this data file](https://github.com/yaaminiv/wc-green-crab/blob/main/data/genotype/2023-crab-genotype.csv). Then, I decided to include the new data and revise TTR and respirometry analyses.

### Time-to-right

I returned to [this R Markdown script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/03-TTR-analysis.Rmd) to include additional genotype data in the TTR analysis. The main thing I needed to do was update some figures, but I made some modifications to the code that I think will make it easier when I have all the genotype data to consider. All the updated figures can be found [here](https://github.com/yaaminiv/wc-green-crab/tree/main/output/03-TTR-analysis/figures), but I specifically wanted to look at differences in TTR over time by genotype.

<img width="674" alt="Screenshot 2024-04-16 at 10 50 00 AM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/6ff2a292-6bc6-456d-aa73-ccd4c543bfe1">

**Figure 1**. Differences in TTR by genotype

Interesting that I have a CC crab! I must have sampled it later on, which is why I don't see it for the day 21 or day 42 plot. But, it's also nice to see that I have more data points that support the idea of a potential CT advantage in prolonged cold conditions. Carolyn suggested I plot it such that the three genotypes for each temperature are together:

<img width="674" alt="Screenshot 2024-04-16 at 12 25 50 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/a5605df8-e4cf-4e32-9f3a-8808786ff44e">

**Figure 2**. Differences in TTR by treatment

It's much clearer to see the differences in genotype! There's something happening with the TT genotype at 25ºC and 5ºC over time.

### Respirometry

In this [R Markdown script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/04-respirometry-analysis.Rmd), I imported the new genotype data. I already had genotypes for most of the respirometry crabs, but there were a few I missed in the initial sequencing round. I reran the models, finding that even though sex as added to the final model,  it often wasn't significant. Additionally, after chatting with Carolyn I decided to remove any interactions between treatment or day with genotype. While it's interesting to interrogate how physiological responses change with treatment, day, and genotype simultaneously, I just don't have the statistical power to do so with the respirometry crabs. I still kept genotype in the model, but removed all associated interaction terms. When testing the revised model for genotype and binary genotype (has C or T allele), I found that the binary genotype was still the model with the lowest AIC. The revised model output can be found [here](https://github.com/yaaminiv/wc-green-crab/blob/main/output/04-respirometry-analysis/all-slope-results-model-output.csv).

### Going forward

1. Work with Vanessa to leave contamination station
1. Re-extract and PCR samples with faint gel bands
1. Work with Vanessa to continue with Chelex extractions, PCRs, and gels for remaining crabs
4. Genotype remaining samples
4. Examine HOBO data from 2023 experiment
5. Determine best statistical approach for analyzing performance data
5. Demographic data analysis for 2023 paper
6. Update methods and results of 2023 paper

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
