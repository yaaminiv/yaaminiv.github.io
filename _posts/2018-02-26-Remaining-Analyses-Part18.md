---
layout: post
comments: true
title: Remaining Analyses Part 18
tags: DNR SRM
---

## My better figure wasn't actually good

I went back to the [(digital) drawing board](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-15-DNR-Paper-Figure.R) to make some more options.

### Option 1: Dot charts

But this time, larger dots.
 
![allpeptides](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-16-All-Peptide-Abundances-Across-Sites.jpeg)

**Figure 1**. Average peptide abundance data across all sites.

![diffexp](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-18-Differentially-Expressed-Peptides-Across-Sites.jpeg)

**Figure 2**. Differentially expressed average peptide abundance across all sites.

### Option 2: Heatmaps

There aren't large differences in the colors themselves (especially when looking at the figure with all peptide abundance data), so it's not that exciting.

![allheatmap](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-26-All-Average-Peptide-Abundance-Heatmap.jpeg)

**Figure 3**. Average peptide abundance heatmap.

![diffexpheatmap](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-26-Average-Differentially-Expressed-Peptides-Heatmap.jpeg)

**Figure 4**. Differentially expressed average peptide abundance across all sites.

### Option 3: Bubble plot

I like Figure 6 more than Figure 5. If possible, I could scale all data by the smallest value to make the bubble sizes bigger and more dramatic.

![xSiteyPeptide](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-26-Bubble-Plot-xSite-yPeptide.jpeg)

**Figure 5**. Average normalized abundance across sites for each peptide. The points themselves are scaled such that larger points represent higher peptide abundance.

![xSiteyAbundance](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-26-Bubble-Plot-xSite-yAbundance.jpeg)

**Figure 6**. Average normalized abundance across sites. Points are scaled such that larger points represent higher peptide abundance, but all site data is grouped together. Points are color coded by peptide.

### Option 4: [Beeswarm plots](https://github.com/eclarke/ggbeeswarm)

Suggestion from Steven! I think it's a step up from the dot charts, but maybe not the best.

![beeswarm](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-26-Beeswarm-Plot-xSite-yAbundance.jpeg)

**Figure 7**. Beeswarm plot.

I wanted to make a volcano plot but it did not seem possible with my dataset. Perhaps I'm not searching for the right things in Google.

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
