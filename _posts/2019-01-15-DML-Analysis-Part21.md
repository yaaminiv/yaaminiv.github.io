---
layout: post
comments: true
title: DML Analysis Part 21
tags: virginica MBDSeq
---

## Examining sample clustering

Something Shelly brought up at the end of last quarter is how odd my sample clustering is. [Previously](https://yaaminiv.github.io/DML-Analysis-Part13/), I compared dendograms and PCA plots for my samples using different `mincov` settings for `methylKit`. Of the settings I used, `mincov = 3` produced the best clustering and PCA output:

![dendogram](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-14-Steven-Samples/2018-10-11-Full-Sample-CpG-Methylation-Clustering-Cov3.jpeg)

![PCA](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-14-Steven-Samples/2018-10-11-Full-Sample-Methylation-PCA-Cov3.jpeg)

**Figures 1-2**. Dendogram and PCA plots for *C. virginica* gonad sequence data using `mincov = 3`.

She suggested I revist these plots to see if I could improve clustering by changing my alignment stringency in `bismark`. HJ mentioned looking at SNP data may also help explain my poor clustering. Looking at these plots again, I see that O1 is farther from the other treatment samples in the PCA, and very separated in the dendogram. This sample also had the [lowest mapping efficiency](https://yaaminiv.github.io/DML-Analysis-Part16/). I decided to see what happened to clustering if I removed that sample before looking into different alignments or SNPs.

![newdendogram](https://raw.githubusercontent.com/fish546-2018/yaamini-virginica/master/analyses/2019-01-15-Sample-Clustering/2019-01-15-Full-Sample-CpG-Methylation-Clustering-Cov3.jpeg)

![newpca](https://raw.githubusercontent.com/fish546-2018/yaamini-virginica/master/analyses/2019-01-15-Sample-Clustering/2019-01-15-Full-Sample-Methylation-PCA-Cov3.jpeg)

**Figures 3-4**. Dendogram and PCA plots for sequence data, omitting sample 1.

Without sample 1, the clustering in the PCA looked a bit better. The red samples are from the control treatment, while the blue samples are the high pCO<sub>2</sub> treatment. It could be that there's no coordinated methylation response to ocean acidification, or that alignment stringency or SNPs are affecting clustering. I have to do some more digging.

### Going forward

1. See how alignment stringency or SNPs affect clustering
2. Determine if a formal gene enrichment is necessary
3. If necessary, select the most appropriate gene enrichment method
4. Describe functions of most interesting genes with DML and DMR

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
