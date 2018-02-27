---
layout: post
comments: true
title: Remaining Analyses Part 19
tags: DNR SRM
---

## Digital drawing board: Round 2

Steven commented on [my previous post](https://yaaminiv.github.io/Remaining-Analyses-Part18/) and said he liked the heatmap option! In my  [R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-15-DNR-Paper-Figure.R) I played around with row clustering and color schemes. There are three decisions that need to be made:

1. All data vs. differentially expressed subset
2. Row clustering vs. no row clustering
3. Color schemes

### Decision 1: All data vs. differentially expressed subset

[all-1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-26-All-Average-Peptide-Abundance-Heatmap.jpeg)

**Figure 1**. All data, now row clustering, standard color scheme

[subset-1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-26-Average-Differentially-Expressed-Peptides-Heatmap.jpeg)

**Figure 2**. Subset, no row clustering, standard color scheme

### Decision 2: Row clustering vs. no row clustering

[all-2](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-All-Average-Peptide-Abundance-Heatmap-Options2.jpeg)

**Figure 3**. All data, row clustering, standard color scheme

[subset-2](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-Average-Differentially-Expressed-Peptides-Heatmap-Option2.jpeg)

**Figure 4**. Subset, row clustering, standard color scheme
{% if page.comments %}

### Decision 3: Color schemes

#### Blues

[all-3](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-All-Average-Peptide-Abundance-Heatmap-Option3.jpeg)

[subset-3](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-Average-Differentially-Expressed-Peptides-Heatmap-Option3.jpeg)

#### Purples

[all-4](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-All-Average-Peptide-Abundance-Heatmap-Option4.jpeg)

[subset-4](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-Average-Differentially-Expressed-Peptides-Heatmap-Option4.jpeg)

#### Purple-Blue

[all-5](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-All-Average-Peptide-Abundance-Heatmap-Option5.jpeg)

[subset-5](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-Average-Differentially-Expressed-Peptides-Heatmap-Option5.jpeg)

#### Grey

[all-6](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-All-Average-Peptide-Abundance-Heatmap-Option6.jpeg)

[subset-6](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-Average-Differentially-Expressed-Peptides-Heatmap-Option6.jpeg?raw=true)

#### Purple-Green

[all-7](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-All-Average-Peptide-Abundance-Heatmap-Option7.jpeg)

[subset-7](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-Average-Differentially-Expressed-Peptides-Heatmap-Option7.jpeg)

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
