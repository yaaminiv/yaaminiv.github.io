---
layout: post
comments: true
title: MethCompare Part 7
tags: MethCompare
---

## Small tasks for future analyses

Since I don't have much to do in the way of analysis right now, I decided to tackle two smaller tasks that will inform how we proceed with our analysis.

### Union bedgraph code

Steven generated union bedgraphs with 5x data for [*M. capitata](https://github.com/hputnam/Meth_Compare/blob/master/scripts/10-Mcap-Canonical-Coverage-Track.ipynb) and [*P. acuta*](https://github.com/hputnam/Meth_Compare/blob/master/scripts/11-Pact-Canonical-Coverage-Track.ipynb). Hollie then imported the union files in R, calculated the average percent methylation at a locus for each method, then used that information to generate Circos plots in [this script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Circos_Meth_Plot.R). At our meeting last week, Hollie asked me and Mac to go over the code just to get another pair of eyes on it. I went through the Jupyter notebooks and got acquainted with `unionbedg`. That code seemed to be working fine, so I went through the portions of Hollie's R script where she imports the union files and calculates averages. As a sanity check, I looked at the range of methylation values at each step to make sure it did not exceed 100% or 1. I didn't see any outlier values! I also confirmed that the correct files or columns were used to delineate between the different methods. Unless I missed something or there is something at play before this script, it looks good to me! I posted my recap in [this issue](https://github.com/hputnam/Meth_Compare/issues/43).

### Exon annotations

Earlier, I found out that neither the *M. capitata* nor *P. acuta* genome annotations included UTR. Hollie suggested I look at other coral methylation papers that characterized exon methylation to see how they were defined. I looked at *Aiptasia* and *Stylophora pistillata* genome feature files used by Li et al. 2018 and Liew et al. 2018 respectively. Looking at the GFFs, I saw that 1) there were distinct exon and CDS annotations in both genomes and 2) the exon track included UTR information. 

![Screen Shot 2020-04-23 at 3 28 50 PM](https://user-images.githubusercontent.com/22335838/80156550-0e8ed480-8579-11ea-831c-554f3ffb2b6a.png)

![Screen Shot 2020-04-23 at 3 38 36 PM](https://user-images.githubusercontent.com/22335838/80156641-4ac23500-8579-11ea-9183-1349e09991e0.png)

**Figures 1-2**. *Aiptasia* and *S. pistillata* GFFs with exon and CDS information. The purple arrows and boxes indicate exons and respective CDS.

The authors that created these genomes are from the same institution, so I'm not surprised that both genomes have similar annotations and annotation criteria. I posted my quick recap in [this issue](https://github.com/hputnam/Meth_Compare/issues/40). I don't really know how we'll be able to compare our findings to these papers if we're unable to get UTR information for exons, but I guess we can always treat our CDS information like exons and look at CDS-intron boundaries.

### Going forward

4. Figure out how to meaningfully concatenate data for each method
5. [Generate repeat tracks for each species](https://github.com/hputnam/Meth_Compare/issues/23)
6. Rerun the pipeline with full samples once pan-genome output is assessed and find a way to generate tables programmatically
3. Create figures for CpG characterization in various genome features
6. [Update code for methylation frequency distribution figure](https://github.com/hputnam/Meth_Compare/issues/39)
3. Figure out methylation island analysis

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
