---
layout: post
comments: true
title: Gonad Methylation Analysis Part 6
tags: virginica bismark MBDSeq
---

## The end of a pipeline test

Yesterday I encountered a `gunzip` error when aligning sequences with `bismark`. I [opened a issue](https://github.com/RobertsLab/resources/issues/236) and documented everything in my [lab notebook post](https://yaaminiv.github.io/Gonad-Methylation-Analysis-Part5/). Steven said that I shouldn't worry about it because I got .bam files! Today, I moved on in my [Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-04-27-Gonad-Methylation-Bismark.ipynb) to the `bismark_methylation_extractor` step. I successfully used the following parameters:

![untitled](https://user-images.githubusercontent.com/22335838/39413440-1ecd0ab8-4bdf-11e8-9855-4131fd39c6e3.png)

Once again, I encountered a `gunzip` error:

![untitled](https://user-images.githubusercontent.com/22335838/39413467-8f4648ae-4bdf-11e8-922c-0ac19c6232e5.png)

And like last time, I have outputs! You can find them in [this folder](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-04-27-Bismark/2018-04-29-Methylation-Extractor-Output). I ignored the error and moved on in the pipeline to the `bismark2report` step.

This step is fairly simple if you don't want to customize the command. I used the following parameters:

1. Path to `bismark2report`
2. --dir + path to output directory

The reports generated can be found in [this folder](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs).

The last part of the pipeline is `bismark2summary`. I'm not sure how this differs from `bismark2report`, but I'm gonna use it anyways. It generated a report that can be found as a [.txt file](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/bismark_summary_report.txt) and [.html report](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/bismark_summary_report.html).

The next steps are to understand the outputs and start the full pipeline. I posted [this issue](https://github.com/RobertsLab/resources/issues/239) to get Steven's advice.

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
