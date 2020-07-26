---
layout: post
comments: true
title: MethCompare Part 25
tags: MethCompare
---

## Small tasks to complete the manuscript

We're officially done with our analyses! My tasks for this week include getting one additional piece of information (see [this issue](https://github.com/hputnam/Meth_Compare/issues/90)), [reviewing the abstract](https://github.com/hputnam/Meth_Compare/issues/89), updating the MBDBS section of the introduction, and tweaking figures.

Before I dove into writing, I reversed the figure legends of my stacked barplots so the order of the color blocks matched the stacked barplots:

![Screen Shot 2020-07-25 at 11 18 42 AM](https://user-images.githubusercontent.com/22335838/88469869-994ddb00-ceaa-11ea-9f92-5a39430f2fab.png)

![Screen Shot 2020-07-25 at 11 18 28 AM](https://user-images.githubusercontent.com/22335838/88469871-9b179e80-ceaa-11ea-9943-d1f5a2402a8c.png)

**Figures 1 and 2**. Stacked barplots with updated legends

I also generated a multipanel PCoA figure [using this script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.Rmd):

![Screen Shot 2020-07-25 at 12 59 15 PM](https://user-images.githubusercontent.com/22335838/88469874-a79bf700-ceaa-11ea-9900-09a15b59af89.png)

**Figure 3**. PCoAs for methylation status and genomic locations in both species.

For the manuscript, I tried condensing the pros and cons for MBDBS into a paragraph. It doesn't flow as well as I'd like, but I'm waiting for some feedback on what the paragraph should address before I make additional changes. I also reviewed the abtract and made suggestions to trim extra language. It's nice to be at the tail end of a #PandemicProject!

### Going forward

1. [Get DMC genomic locations](https://github.com/hputnam/Meth_Compare/issues/90)
2. Help write mansucript!
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
  
