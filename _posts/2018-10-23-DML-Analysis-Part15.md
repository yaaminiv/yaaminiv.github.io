---
layout: post
comments: true
title: DML Analysis Part 15
tags: virginica MBDSeq DML bedtools 
---

## Preliminary `bedtools` analysis

In [this R Markdown file](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-11-MethylKit-Parameter-Testing.Rmd), I wrote code to create BEDfiles for the [DMLs](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-22-DML-Locations.bed) and [DMRs](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-22-DMR-Locations.bed) I identified.

My next step was to characterize the location of DMLs and DMRs. I did by revising code in a previously existing [Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-06-11-DML-Analysis.ipynb). I counted the number of DMLs and DMRs, as well as the number of overlaps between each of those features with exons, introns, and mRNA coding regions. I exported .csv files with the intersection of these features in [this folder](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-10-22-DML-Analysis). One thing I noticed was that there were equal numbers of hypermethylated and hypomethylated DMLs. These DMLs were primarily in the mRNA coding regions, and more were in exons than introns. However, there were significantly more hypomethylated DMRs than hypermethylated DMRs, and more DMRs in introns than exons. This could hint at some function for alternative splicing.

For my presentation to the Lotterhos lab, I also cherry-picked some genes what were differentially methylated. Cilia and flagella genes, fatty acid desaturation genes, and multidrug resistance protein genes were hypermethylated, while heat shock protein and MAP kinase genes were hypomethylated.

One thing that came up while Steven and I were looking at my results is that Steven is getting way less DMLs and DMRs than I am. We quickly walked through our `methylKit` code and found no discrepancies. This is something we will need to sort out!

### Going forward

1. I will update the [*C. virginica* gonad methylation paper](https://docs.google.com/document/d/1gOMJrnhs4D-jCKWlJK2tm0Z27IrSqMkmc7K1pDBmqi0/edit#heading=h.r39if6ga2q0r) with reproducible methods and preliminary results. Steven will use this information to reproduce my work and hopefully figure out why we're getting different results.
2. I need to figure out how to do a proper gene enrichment and flanking analysis
3. Ready scripts for sample analysis when my Mox job finishes running

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
