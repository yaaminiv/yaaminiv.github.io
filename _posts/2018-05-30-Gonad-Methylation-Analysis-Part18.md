---
layout: post
comments: true
title: Gonad Methylation Analysis Part 18
tags: virginica methylkit MBDSeq
---

## Preparing for discoveries

Before I can understand where differentially methylated loci (DML) are located within the *C. virginica* genome, I first need to identify DMLs! I used [this R script](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-05-29-MethylKit-Full-Samples/2018-05-29-MethylKit-Analysis-Full-Samples.R) to identify DMLs that were at least 50% different between control and treatment (high pCO<sub>2</sub>) samples ([.csv here](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-05-29-MethylKit-Full-Samples/2018-05-30-Differentially-Methylated-Loci-50.csv)). Looking at the PCA was interesting, as the clustering was not as tight as I expected.

![PCA](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-05-29-MethylKit-Full-Samples/2018-05-30-Full-Sample-Methylation-PCA.jpeg)

**Figure 1**. Principal Components Analysis of methylated regions in samples. 

O2-5 (ambient conditions) are more closely clustered than any of the oysters from treatment conditions. It's possible that there are organismal differences in methylation responses, or that we just didn't have a large enough sample size to deal with this variation.

In the last part of the script, I saved my DML information as a BED file. I mimicked [Steven's code](https://sr320.github.io/OALK-to-the-browser/) to do this. [BED files](https://genome.ucsc.edu/FAQ/FAQformat.html) have chromosome ID, start, and stop positions that I can use to pare down information about my DMLs. I can then compare the DML location with other important genomic features using [bedtools](http://bedtools.readthedocs.io/en/latest/). The [`intersect`](https://www.biostars.org/p/13516/) tool seems especially useful. I know we covered `bedtools` in the [2016 Bioinformatics class](https://github.com/sr320/course-fish546-2016/wiki), so I'll review those notes!

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
