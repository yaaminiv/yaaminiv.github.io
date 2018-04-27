---
layout: post
comments: true
title: Gonad Methylation Analysis Part 3
tags: virginica bismark MBDSeq
---

## Quality: controlled

Turns out there was a typo in the IP address I was using to connect to genefish. Whoops. Now that I'm in the office, I was able to complete my FastQC analysis! You can see my process in [this Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-04-26-Gonad-Methylation-FastQC.ipynb).

On the Mac mini, I located the files I needed from Owl and ran the command-line version of FastQC. The results can be found in [this folder](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-04-26-Gonad-Methylation-FastQC). There are two outputs: an .html file, and a .zip file. The .html files can be viewed by dragging the file from Finder into a web browser. To compare FastQC reports, I ran MultiQC. The interactive .html output can be found [here](http://htmlpreview.github.io/?https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-26-Gonad-Methylation-FastQC/multiqc_report.html). In general, I think my data has relatively good quality and I can proceed to Bismark.

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
