---
layout: post
comments: true
title: Selecting SRM Targets Part 3
---

## MSstats works!

After a few weeks, I finally figured out how to use MSstats to identify differentially expressed proteins and peptides. This [Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-06-22-Selecting-SRM-Targets-with-MSstats-Part-2.ipynb) has my workflow, but here are the highlights:

### Step 1: Export data from Skyline

At first, I was exporting data from Skyline to meet the specifications for `dataProcess` in the [MSstats manual](https://bioconductor.org/packages/release/bioc/vignettes/MSstats/inst/doc/MSstats-manual.pdf). By following that route, I ran into an error saying that I had duplicate proteins and needed to specify a "Fraction" value. Better yet, there was no information about peptide fractions anywhere relating to MSstats! After a bunch of Googling, I found this handy function [`SkylinetoMSstatsFormat`](https://rdrr.io/bioc/MSstats/src/R/SkylinetoMSstatsFormat.R). I exported three separate Skyline reports to use this function, but with different BioReplicate and Condition information: 1) Bare vs. Eelgrass, 2) Sites only and 3) Sites and Eelgrass condition.

![unnamed-2](https://user-images.githubusercontent.com/22335838/27842639-90c1709c-60c0-11e7-8062-167e796ed7c7.png)

**Figure 1**. Columns needed for `SkylinetoMSstatsFormat`.

![unnamed-3](https://user-images.githubusercontent.com/22335838/27842651-aa492a46-60c0-11e7-9f16-c7e0098fd022.png)

**Figure 2**. I saved the columns needed for `SkylinetoMSstatsFormat` under Export >> Report >> "SkylinetoMSstats".

![unnamed-4](https://user-images.githubusercontent.com/22335838/27842702-2bf7f37e-60c1-11e7-8585-77d55538a0cc.png)

**Figure 3**. Example of Condition and BioReplicate settings, this one for both Site and Eelgrass condition pairwise comparisons.

### Step 2: Use MSstats

The commands I used to conduct pairwise comparisons: `SkylinetoMSstatsFormat` >> `dataProcess` >> `groupComparison`

I then used `groupComparisonPlots` to graphically represent my data.

I found no differentially expressed proteins when doing pairwise comparisons between Bare and Eelgrass sites, but I did find differentially expressed proteins when looking at just sites, and when accounting for both site and eelgrass conditions!

Next step: examine annotations to see what kinds of proteins are differentially expressed.

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
