---
layout: post
comments: true
title: Demultiplexing Update
---

## New MSConvert CLI, hot off the presses

Emma ran over a new version of `msconvert` Austin recoded to accomodate the idiosyncracies in our isolation windows. He generated a new .config file, found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_MSConvert_20170412/config_fix_20170413.txt). Below is a side-by-side comparison of two different .config files. The one on the left is the original code, and the one on the right is the modified code. 

![unnamed](https://cloud.githubusercontent.com/assets/22335838/25025335/b70a5d36-2056-11e7-9232-4a4ac436e04f.png)

I used the following code to convert one .raw file:

```
20170412_pwiz_testBuild_addMinWindowSize\msconvert.exe -c config_fix.txt 2017_January_23_envtstress_oyster1.raw
```

If this works, I'll start converting all of my files! Emma will also take my converted file and run it through PECAN using a [modified isolation scheme](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_MSConvert_20170412/2017_January_23_envtstress_oyster1_isoscheme.txt). You can track my progress with this [Jupyter Notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-04-12-Demultiplex-Raw-Files.ipynb).

One possible caveat is that Laura and I have different isolation schemes and isolation window sizes. It's possible that the .config file I'm using won't be compatible with Laura's data, and is only specific to my data for this experiment.

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
