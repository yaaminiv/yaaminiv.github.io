---
layout: post
comments: true
title: DML Analysis Part 10
tags: virginica MBDSeq DML bismark
---

## Testing `bismark` parameters

After meeting with Steven this week, we decided the best course of action was to revise the analysis pipeline and document why we chose the settings we did. The first part of this revision is to go back to `bismark.` When I [ran my full samples through `bismark`](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-05-22-Gonad-Methylation-Full-Samples.ipynb), I used the default setting for `-score_min`. This lead to a ~20% mapping efficiency. That's not great.

The `-score_min` option dictates how strict the alignment is. The default option is L,0,-0.2, which is pretty stringent. In this [Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-10-03-Bismark-Parameter-Testing.ipynb), I tested 3 different `-score_min` options: L,0,-0.6; L,0,-0.9; L,0,-1.2. Samples were run in the following order:

<img width="854" alt="screen shot 2018-10-04 at 10 47 55 am" src="https://user-images.githubusercontent.com/22335838/46492640-fef74180-c7c2-11e8-82f8-419cfa6ed36e.png">

Therefore, the first mapping efficiency listed belongs to sample 10, second to sample 1, etc. I kept this in mind and generated the following table:

**Table 1**. Mapping efficiency (%) based on different `-score_min` settings.

| **Sample** | **L,0,-0.6** | **L,0,-0.9** | **L,0,-1.2** |
|:----------:|:------------:|:------------:|:------------:|
|      1     |     15.5     |     20.2     |     28.8     |
|      2     |     32.4     |     40.2     |     49.8     |
|      3     |     37.2     |     45.3     |     53.6     |
|      4     |     36.0     |     44.7     |     52.9     |
|      5     |     34.6     |     42.9     |     51.7     |
|      6     |     36.7     |     45.0     |     53.8     |
|      7     |     34.6     |     42.9     |     51.4     |
|      8     |     31.7     |     39.0     |     47.6     |
|      9     |     33.0     |     41.2     |     49.9     |
|     10     |     36.6     |     44.9     |     53.0     |

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
