---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 73
tags: green-crab-wc metabolomics
---

## More PCA explorations

I revisited [this script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/05-metabolomics-analysis.Rmd) to see if there were different things I needed to consider when doing my genotype PCAs and perMANOVAs. All the output can be found [here](https://github.com/yaaminiv/wc-green-crab/tree/main/output/05-metabolomics-analysis/PCA).

The first thing I did was test the impact of outlier removal. Sample 5-075 (CC crab, day 42, 5ºC) looked like an outlier based on visual inspection. I ran the PCAs and perMANOVAs with and without this sample. Surprisingly, there was no impact on the actual statistical test result, so I can include this sample moving forward. I may consider removing this sample for the PLS-DA, but that is a later problem.

The next thing I did is test the impact of heterozygote vs. not heterozygote. I did this because I saw some dispersion differences in the CT crabs at each temperature, which could be indicative of increased fitness of heterozygotes in environmental conditions. There wasn't a significant impact of heterozygote or not in the perMANOVAs.

Finally, I tried to see if there were genotype differences within each temperature. Since I saw a significant impact of temperature, I was wondering if that strong main effect was masking any genotypic differences. Additionally, since I expect C and T alleles to be differently beneficial at each temperature, maybe breaking things down further would be worth it. There is still no significant imapct of genotype in the perMANOVAs.

It's clearer to me looking at the temperature-specific PCAs that there are overlapping centroids, but potentially stronger disperson differences. I'm not sure how to test for this statistically. My understanding is that a beta-dispersion test is only useful when you have groups with statistically different centroids, not for testing dispersion differences with similar centroids. I'll need to tease this out further.

### Going forward

1. Experiment with different methods for the temperature x genotype question
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
