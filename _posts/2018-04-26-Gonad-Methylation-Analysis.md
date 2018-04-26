---
layout: post
comments: true
title: Gonad Methylation Analysis
tags: virginica bismark MBDSeq
---

## Time to analyze the *C. virginica* data

Now that my two papers do not require my constant attention, I can start analyzing the MBDSeq data from the *C. virginica* project. The goal is to see if experimental ocean acidfication drove differential gonad methylation in adult oysters. This lab notebook entry will outline my plan and link to important information I'll need down the road.

[Sam received the FASTQ files](http://onsnetwork.org/kubu4/2018/03/29/data-recived-crassostrea-virginica-mbd-bs-seq-from-zymoresearch/) and [saved them here](http://owl.fish.washington.edu/nightingales/C_virginica/). The sample IDs [follow numerical order](https://github.com/RobertsLab/resources/issues/220), and [are non-directional](https://github.com/RobertsLab/resources/issues/216).

Here's how I will process these samples:

1. FastQC
I previously used FastQC with some *O. lurida* transcriptome data, so I can follow the general steps in [this Jupyter notebook](https://github.com/yaaminiv/yaaminiv-fish546-2016/blob/master/notebooks/2016-10-13-oly-gonad-OA-part-1.ipynb).

2. [Bismark](https://rawgit.com/FelixKrueger/Bismark/master/Docs/Bismark_User_Guide.html)
The purpose of Bismark is to align my sample files with the *C. virginica* genome, then extract data from methylated areas. I will first test my Bismark pipeline with a subset of one data file. Once I know it works, I will run all my samples.

Now that I know what I'm doing, I should probably do it...

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
