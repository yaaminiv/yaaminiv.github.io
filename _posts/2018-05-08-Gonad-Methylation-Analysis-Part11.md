---
layout: post
comments: true
title: Gonad Methylation Analysis Part 11
tags: virginica bismark MBDSeq
---

## I am Jerry Gergich

![jerry](https://i.pinimg.com/originals/e7/ba/bc/e7babcb371caf0c4bd0b8c174913dfc1.png)

Today I'm really riding the struggle bus! In [this issue](https://github.com/RobertsLab/resources/issues/252), I found out that I forgot an important argument in my `bismark` alignment. I did not indicate that I had paired read data. I now have to invoke my inner Jerry Gergich and redo the work. I specified the -1 and -2 "mates" files and reran the code.

For my subset, I will also re-extract the methylated data and remake my reports. All of the file names will stay the same, so they should still be easy to find. While I wait for the alignment to finish on the subset, I'll start working with `methylKit`. Switching out the correct data will be simple!

P.S. Here's some evidence I'm visibly on that struggle bus ft. Steven's classic sass. Is the "Yep" referring to me figuring out the problem, or acknowledging that I'm not thinking clearly? I think it's both:

<img width="531" alt="screen shot 2018-05-08 at 10 51 34 am" src="https://user-images.githubusercontent.com/22335838/39781978-947dd6ec-52c5-11e8-8620-cfc59d529995.png">

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
