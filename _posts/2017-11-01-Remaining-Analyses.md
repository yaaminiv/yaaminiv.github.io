---
layout: post
comments: true
title: Remaining Analyses
tags: DNR SRM
---

## Squeezing every last bit of information out of my data

As I outlined in my [November Goals](https://yaaminiv.github.io/November-Goals/), this week I'm playing around with my dataset to see if I can pool my eelgrass and bare samples, and repeating my NMDS and ANOSIM analyses. I know Steven said to focus on protein-level responses, but I'm curious...

### Step 1: Rerun NMDS and ANOSIM without pooling

[Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-11-01-NMDS-ANOSIM-for-Cluster-Analysis-after-CV-and-Sample-Filtering.R)

Site only ANOSIM: R = 0.01339, Significance: 0.326 
Eelgrass/Bare only ANOSIM: R = 0.02932, Significance: 0.168 
Site and eelgrass ANOSIM: R = 0.00386, Significance: 0.41

Nothing significant, but here's the pattern I see in that NMDS:

![NMDS](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-11-01-NMDS-Norm-Analysis-Averaged-Annotated.jpeg)

**Figure 1**. Annotated NMDS plot

My groups overlap because there's still a lot of variability within my groups. This could be due to the [alternative splicing](https://yaaminiv.github.io/Correlating-Technical-Replicates-Part10/) I'm seeing within my protein abundances.

### Step 2: Pool eelgrass and bare samples

There has been no indication that my eelgrass samples have significantly different protein expression results from my bare samples (see above eelgrass-only ANOSIM results). So I pooled them. Again nothing significant, but it's nice looking at a less-distracting picture.

[Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-11-01-NMDS-ANOSIM-for-Cluster-Analysis-after-CV-and-Sample-Filtering-and-Pooling.R)

ANOSIM: R = 0.01339, Significance: 0.321 

![NMDS2](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-11-01-NMDS-Norm-Analysis-Averaged-Pooled.jpeg)

**Figure 2**. Annotated NMDS plot after pooling bare and eelgrass samples

I'm just go ahead and say we're about done here. No more NMDS and ANOSIMs!

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
