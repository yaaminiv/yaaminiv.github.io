---
layout: post
comments: true
title: Gonad Methylation Analysis Part 17
tags: virginica bismark MBDSeq
---

## Note to self: Always double check things

- I forgot to change the code in my [subset](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-05-22-Gonad-Methylation-Subset.ipynb) and [full sample](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-05-22-Gonad-Methylation-Full-Samples.ipynb) notebooks so that `bismark_methylation_extractor` ran on the files I produced instead of those in the `dignore` folder. I switched the code and everything still works!
- I thought I double checked what `bismark_methylation_extractor` outputs needed to be in the `.gitignore` but I left out several *deduplicated.txt files that were well over 100 MB. My mistake took me three days and [one Github issue](https://github.com/RobertsLab/resources/issues/276) to figure out. Whoops. Now I know how to use the Github command line, [add things to my `.gitignore`](https://www.tilcode.com/how-to-quickly-add-lines-to-gitignore-using-the-command-line/), and effectively undo commits to make Github Desktop happy.

I finished up the methylation extraction, HTML report, and summary report steps! I then started `methylKit` on the full samples to ensure reproducibility. When I'm finished, I'll create BEDfiles and start to understand where differentially methylated loci are located and waht the gene functions are.

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
