---
layout: post
comments: true
title: Gonad Methylation Analysis Part 12
tags: virginica methylKit MBDSeq
---

## Reproducibility rodeo

My goal was to run my samples through [Steven's methylKit R code](https://sr320.github.io/MethylKittens/). The first thing I did was copy his code into [this R script](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-05-01-MethylKit/2018-05-01-MethylKit-Analysis.R). I then added more descriptive names for the objects, and commented out each line of code so I understood what was happening.

I was able to run `processBismarkAln` on my deduplicated.sorted.bam files! However, I could not `unite` these files. That's when I posted a [Github issue](https://github.com/RobertsLab/resources/issues/254). After some back-and-forth, Steven suggested I download some of [his dedup.sorted.bam](http://owl.fish.washington.edu/halfshell/bu-serine-wd/18-04-27/) and run those files through the R script. By using his files, I could figure out if the `unite` issue stemmed from my files, or from software differences on the Mac mini.

I created [a new R script](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-05-01-MethylKit/2018-05-09-Steven-Test-Files/2018-05-09-MethylKit-Analysis-Steven-Test-Files.R) and downloaded dedup.sorted.bam files 1 and 10 (i.e. one from each treatment). I was able to successfully `unite` these files and go through the entire script! While I couldn't identify any DMRs between the two files, I did come up with two reasons why Steven's files worked and mine did not:

1. Steven's files are trimmed, but mine are not
2. Steven's files used the first 1,000,000 as a subset, but mine used the first 10,000

Either way, Steven said I should move on to the full pipeline and validate everything. Sam and Steven both pointed me to [this directory](http://owl.fish.washington.edu/Athaliana/20180411_trimgalore_10bp_Cvirginica_MBD/), which has the trimmed files. He also said that I should remove the `-score_min` argument. This will make `bismark` run the default setting for alignment mismatches, and allow us to compare the alignment results. Time to start a computer process that will take several days to run!

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
