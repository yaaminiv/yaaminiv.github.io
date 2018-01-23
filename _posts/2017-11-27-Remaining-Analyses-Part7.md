---
layout: post
comments: true
title: Remaining Analyses Part 7
tags: DNR SRM
---

## Column comparisons

Emma [ran two of my samples](https://yaaminiv.github.io/Remaining-Analyses-Part4/) to see if she could "replicate my poor technical replication." Instead, she ended up with good technical replication. The oyster samples are closer to themselves rather than the other oyster. There still seems to be quite a distance between the two technical replicates in ordination space though, but I guess that doesn't matter (or could be due to the fact that she didn't normalize the data).

![nmds](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-27-Column-Comparisons/2017-11-16-NMDS-Technical-Replication-NonNorm.jpg)

**Figure 1**. Technical replication from Emma rerunning my samples.

She also provided us the [Skyline output](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-27-Column-Comparisons/2017-11-16-SRM-Results-Yaamini.csv). I could use this to compare the area data with the area data from my run.

Emma suggested that I calculate [predicted retention times by column](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-27-Column-Comparisons/2017-11-27-Predicted-SRM-Retention-Times-By-Column.xlsx). For the first column, I used PRTC retention times from oyster sample 4. My regression equation was 0.2734x + 11.52. I used this equation to predict retention times for my oyster peptides.

<img width="238" alt="screen shot 2017-11-27 at 1 51 01 pm" src="https://user-images.githubusercontent.com/22335838/33292650-dc48db46-d37e-11e7-9c17-44e89a32127d.png">

**Figure 2**. Predicted retention times for oyster peptides based on column 1 equation.

Using PRTC retention times from oyster sample 94, I got an equation of 0.2718 + 10.939 for column two.

<img width="145" alt="screen shot 2017-11-27 at 1 51 17 pm" src="https://user-images.githubusercontent.com/22335838/33292663-e512606c-d37e-11e7-9b93-35cb307cbe0b.png">

**Figure 3**. Predicted retention times for oyster peptides based on column 2 equation.

There's a slight difference in retention time from column 1 to 2, but not enough to where I would have picked the wrong peak. So now I'm really stumped and have no idea why my technical replication wasn't good.

Emma also suggested I find PRTC peptides where I'm confident in the concentrations and try and compare peak areas between column 1 and 2. Based on [screenshots from this lab notebook entry](https://yaaminiv.github.io/SRM-Analysis/), my PRTC peptides had relatively similar peak area magnitudes at the end of the first column and beginning of the second column (around samples 85-95).

So yeah, I'm stumped.

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
