---
layout: post
comments: true
title: Remaining Analyses Part 20
tags: DNR SRM
---

## This series has (finally) come to an end

I showed Steven, Emma, and Brent my [heatmap options](https://yaaminiv.github.io/Remaining-Analyses-Part19/) and we decided to include all peptide abundance data, not to cluster rows, and to go with the purple-green color scheme.

![draftfigure](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-All-Average-Peptide-Abundance-Heatmap-Option7.jpeg)

I sent the figure to my friend with red-green confusion, and he said he could understand it! In my [R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-15-DNR-Paper-Figure.R), I started playing around with other modifications.

The first thing I did was switch out complicated protein names for common names. I created a [.csv file](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-Protein-Peptide-CommonName.csv) that matched protein name, common name, and peptides. I also added an asterisk to the end of each peptide that exhibited differential abundance. I used the common name and peptide to identify rows in the heatmap. I also added a title and tried making some of the font size bigger. `pheatmap` is not a package that's easy to manipulate if you want to change the aesthetics of your plot. I'll probably have to do more work in Powerpoint, Photoshop or InDesign (which sucks because that's not reproducible).

For now, here's my figure!

![currentfigure](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-02-15-DNR-Paper-Figure/2018-02-27-Average-Peptide-Abundance-Across-Sites-NamePeptide-Heatmap.jpeg)

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


