---
layout: post
comments: true
title: Selecting SRM Targets Part 7
tags: DNR SRM
---

## WE HAVE OUR TARGETS.

After looking at [my work from yesterday](https://yaaminiv.github.io/Selecting-SRM-Targets-Part6/), Steven suggested that I hit 150 transitions and max out our capacity. However, I've already pored over both the short and long protein lists. He offered to help me search for proteins in the full Skyline output. I merged [my full Skyline output](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170524/2017-06-10-protein-areas-only-error-checked.csv) with annotations to get [this list](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_Skyline_20170524/2017-07-09-Full-Skyline-Output-Annotations.csv). I evaluated the following proteins that Steven identified as interesting:

<img width="913" alt="screen shot 2017-07-12 at 11 00 12 pm" src="https://user-images.githubusercontent.com/22335838/28152541-ede50c7a-6755-11e7-9869-a06194455abb.png">

<img width="907" alt="screen shot 2017-07-12 at 11 00 25 pm" src="https://user-images.githubusercontent.com/22335838/28152540-ede14bbc-6755-11e7-9300-11ecd10b0ba6.png">

Most of the peaks had interference or missing data, but there were a handful I added to my protein list. Just to recap, here's what I was looking for.

![unnamed-1](https://user-images.githubusercontent.com/22335838/28181196-91d0eeba-67bc-11e7-976a-2716159a9e58.png)

![unnamed-2](https://user-images.githubusercontent.com/22335838/28181197-91d22df2-67bc-11e7-8cdc-f04aab4b61c4.png)

**Figures 1-2**. A noisy transition in Figure 1 (highlighted in red) that when deleted, produces a much better looking peak in Figure 2.

![unnamed-3](https://user-images.githubusercontent.com/22335838/28181216-9fcb05fa-67bc-11e7-8454-b4b354210726.png)

**Figure 3**. A peak showing slight interference. I avoided this when possible. When interference was more extreme, I deleted the peptide.

![unnamed-4](https://user-images.githubusercontent.com/22335838/28181239-bfa598b8-67bc-11e7-9d2e-2c9ac7f3ba57.png)

**Figure 4**. An example of an ideal peak.

I then evaluated all of my proteins one last time:

<img width="1030" alt="screen shot 2017-07-12 at 11 01 59 pm" src="https://user-images.githubusercontent.com/22335838/28152582-254d9d76-6756-11e7-9cf2-a6dde7dbac9b.png">

And I finally had a list of 146 targets from 16 proteins!

<img width="544" alt="screen shot 2017-07-12 at 11 09 31 pm" src="https://user-images.githubusercontent.com/22335838/28152805-3252dd14-6757-11e7-964e-e8ab9060e092.png">

Aside from some last-minute lab preparation, I'm all set for starting my mass spectrometry runs tomorrow! *cue happy dance*

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
