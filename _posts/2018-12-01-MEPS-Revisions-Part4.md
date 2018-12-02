---
layout: post
comments: true
title: MEPS Revisions Part 4
tags: DNR SRM MEPS-Revisions
---

## Using `simper` and new figures

I spent most (yes......most............RIP) of the past two days revising my ordinations and creating ordination figures. Sidenote: I also ordinated means and variances separately as per Julian's suggestion.

### `simper`

During my meeting with Julian on Friday, he suggested I use `simper` to identify loadings that contribute to any significant ANOSIM results. The code is as follows:

```
simper(dataUsedInANOSIM, group = groupUsedInANOSIM) #I used the same code I did for ANOSIM, but just changed "grouping" to "group"
```

Since FB-WB and SK-WB were the significant contrasts in my protein abundance data, I decided to use those contrasts consistently for my [protein abundance NMDS](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-ANOSIM-for-Cluster-Analysis.R), [mean data NMDS](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2018-11-30-Mean-Environmental-Data-NMDS.Rmd), and [variance data NMDS](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2018-11-30-Variance-Environmental-Data-NMDS.Rmd).

Here are the protein abundance `simper` results...

<img width="662" alt="screen shot 2018-11-30 at 1 01 59 pm" src="https://user-images.githubusercontent.com/22335838/49341786-2de53480-f607-11e8-88d4-e1244da8bcb6.png">

<img width="663" alt="screen shot 2018-11-30 at 1 02 19 pm" src="https://user-images.githubusercontent.com/22335838/49341787-2de53480-f607-11e8-8a54-a342dee194d9.png">

...and my thought process for selecting loadings to display:

![56539232733__1a692e89-7bc2-400c-bfd5-9ef81f773aff](https://user-images.githubusercontent.com/22335838/49341843-06429c00-f608-11e8-86c3-5fd89652204b.JPG)

In essense, I identified the first 10 peptides in the `simper` output for the FB-WB and SK-WB contrasts. I then selected the peptides that were common between the two.

For the mean and variance `simper` output, I again looked at the FB-WB and SK-WB contrasts. I took the first 5 dates in the `simper` output for each environmental variable that was significant in an ANOSIM (dissolved oxygen and temperature for means; pH, dissolved oxygen, salinity, and temperature for variance). The later outplant dates were the ones differentiating FB-WB, while the earlier dates were more important for distinguishing SK-WB.

![56538954001__4cf036a2-3d26-48bb-9913-17fff8acdeb5](https://user-images.githubusercontent.com/22335838/49341845-0e9ad700-f608-11e8-9d8a-660a676bb16a.JPG)

![56538870793__a80d0866-d225-44f7-bbc3-75892a6ce1b2](https://user-images.githubusercontent.com/22335838/49341846-10649a80-f608-11e8-9460-8ae7aab282f8.JPG)

In [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-12-01-Final-Ordination-Plot.Rmd), I cobbled together a multipanel plot with all of my ordination plots! It still needs a little work for the inner margins, not cutting thigns off, and legend display (maybe under each ordination set...?), but I was too tired after 3 hours of plot manipulation to do anything else *shrugs*. The pdf can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-12-01-Multipanel-Ordination.pdf), and a screenshot is below:

<img width="828" alt="screen shot 2018-12-02 at 8 13 06 am" src="https://user-images.githubusercontent.com/22335838/49342028-25422d80-f60a-11e8-92c9-2d72d5d5cc3c.png">

**Figure 1**. Multipanel ordination plot.

### Displaying environmental data

I wanted a better plot for my environmental data, so I made one over Thanksgiving! Note how it uses the same colors for each site as the multipanel plot :wink: The code can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2018-11-20-Environmental-Data-Line-Graph.Rmd).

![envdata](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2018-11-20-Environmental-Data-Line-Graph.jpeg).

**Figure 2**. Environmental variables by site and habitat.

### Protein abundance heatmap

As per reviewer suggestion, I changed the colors on my protein abundance heatmap ([code here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-15-DNR-Paper-Figure.R)). In InDesign, I changed which peptides and proteins were marked as signficantly different. I know there's a way I could do this in R, but it was 8 p.m. and I was in SAFS on a Saturday so...*shrugs*.

One important thing to note with the heatmap is that there are different proteins and peptides marked as significantly different! Most of the proteins are the same as what was in my original analysis, but some have been removed. Protein disulfide isomerase 1 and 2 are now identified as significantly different. This is something I need to alter in my results and discussion.

![heatmap](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-12-01-Average-Peptide-Abundance-Across-Sites-NamePeptide-InDesignMod-Heatmap.png)

### Going forward

1. Update manuscript with new methods, results, and figures
2. Tweak discussion
3. Address remaining reviewer comments
4. Send to co-authors!

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
