---
layout: post
comments: true
title: Gonad Methylation Analysis Part 13
tags: virginica bismark MBDSeq
---

## The whole enchilada (but better this time)

Now that I've tested the entire `bismark` to `methylKit` pipeline on a 10,000 read subset for each *C. virginica* sequencing sample, I can begin validating the full pipeline. Steven already ran the full samples on this pipeline in [this notebook](https://github.com/sr320/nb-2018/blob/master/C_virginica/17-OAKL-fullpipe-mk.ipynb). My job is to reproduce his workflow.

I opened my [full pipeline Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-05-04-Gonad-Methylation-Full-Samples-Bismark.ipynb). Since I'm using trimmed files from [this directory](http://owl.fish.washington.edu/Athaliana/20180411_trimgalore_10bp_Cvirginica_MBD/), the first thing I wanted to do was test my `find` and `xargs` commands to ensure I isolated the right files.

<img width="932" alt="screen shot 2018-05-09 at 1 45 41 pm" src="https://user-images.githubusercontent.com/22335838/39838828-50129d10-538f-11e8-8d0f-05df6947990f.png">

**Figure 1**. Testing `find` and `xargs` on a new set of filenames.

Then, I piped the `find` and `xargs` output into `bismark`. Unlike Steven, I did not set a parameter for `-score_min`. In our conversation today, we decided that I should run the alignment with the default parameter so we can compare our results. The default is L,0,-0.2 (instead of L,0,-1.2 used by Steven).

<img width="869" alt="screen shot 2018-05-09 at 1 48 28 pm" src="https://user-images.githubusercontent.com/22335838/39838985-adf946ae-538f-11e8-8a2d-dc376fc2ef91.png">

**Figure 2**. Description of each line of code, along with the actual command.

It took me a bit of finnagling to get that code to work because of some typos! I'm going to jot down a few reminders for myself:

- Always check to see if each line has a "\"
- Use tab complete to ensure lack of typos and the correct path to files
- Verify you're using the correct comand
- Test small bits of code before putting everything together

I'll check on the alignment progress tomorrow, but my guess is that this will take 2-3 days to complete. Once the alignment is finished, I will deduplicate, sort, and index the resulting .bam files.

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
