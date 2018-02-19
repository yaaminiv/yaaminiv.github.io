---
layout: post
comments: true
title: Remaining Analyses Part 17
tags: DNR SRM
---

## I created a better figure!

In [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-15-DNR-Paper-Figure.R), I first separated my peptide abundance data by site. Then, I averaged peptide abundance at each site. I saved the datafram as a .csv [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-16-Average-Peptide-Data-by-Site.csv).

Using the [`dotchart`](https://www.statmethods.net/graphs/dot.html) function in R, I created two plots that I can use as the cornerstone of my DNR paper. The first figure I made included all of my data, which Brent suggested.

![allpeptides](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-16-All-Peptide-Abundances-Across-Sites.jpeg)

**Figure 1**. Average peptide abundance data across all sites.

Each protein is coded with the same color, and with different shapes indicating the constituent peptides. I also used this color scheme to indicate protein function:

- Oxidative stress: blue
- Heat shock: orange/red
- Acid-base balance: purple
- Drug resistance: pink
- Fatty acid metabolism: yellow
- Carbohydrate metabolism: brown
- Cell growth and maintenance: green

I didn't add a legend, since I thought I'd first get feedback on the figure, then make it better. The downside with this graph is that there are 37 peptides at 5 different locations in this one figure, so it's a little busy.

I decided to narrow down the figure to the peptides were differentially expressed. I highlighted the differentially expressed peptides in the figure below.

![diffexplist](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-06-Boxplots/2018-02-17-Differentially-Expressed-Peptides.jpeg)

**Figure 2**. List of differentially expressed peptides.

I noticed that heat shock protein wasn't included in this list since I revised p-values! There are 16 total differentially expressed peptides. I then used this dataset to remake my figure.

![diffexp](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-18-Differentially-Expressed-Peptides-Across-Sites.jpeg)

**Figure 3**. Differentially expressed average peptide abundance across all sites.

This figure is cleaner. We'll see what Steven, Brent and Emma think at our meeting on Tuesday!

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
