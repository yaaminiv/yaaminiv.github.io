---
layout: post
comments: true
title: MethCompare Part 24
tags: MethCompare bedtools
---

## Another (and hopefully final) iteration of union bedgraph analyses

Last night, Shelly pointed out that there were different versions of union begraphs in [this issue](https://github.com/hputnam/Meth_Compare/issues/43). Steven originally created union bedgraphs, but in one of our meetings we realized that there were duplicate entries in each file. Shelly removed those duplicates and saved another version of files. I knew about these versions, but only used the older version with duplicates for my analysis! Even though my `bedtools` code was written such that overlaps with genome features are only listed once, the presence of duplicates would affect the proportions of highly methylated, moderately methylated, and lowly methylated loci. I changed the file path I used to source the union bedfiles in [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union.ipynb) and reran the script. Then, I took that output and ran it through [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd) to generate my figures and count information. As expected, no drastic changes to the methylation status stacked barplots and no change at all to the genomic location barplots. I updated the figures in the manuscript and started working on the results. Now that we have the correct input and all the code is (hopefully) refined, perhaps I won't have to run this again in the future...

### Going forward

2. Look into program for mCpG identification

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
  
