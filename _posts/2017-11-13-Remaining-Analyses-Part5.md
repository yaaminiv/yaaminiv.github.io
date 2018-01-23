---
layout: post
comments: true
title: Remaining Analyses Part 5
---

## I've got the power

(Maybe).

I added a section to my [boxplot R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-06-Boxplots/2017-11-06-Protein-Area-Boxplots-after-Integration.R) for power analyses. I did two things: 1) calculate the  power of my experimental design based on theoretical effect sizes and 2) calculated the detectable effect sized based on theoretical power values. To do this, I used the [`pwr` R package](https://www.statmethods.net/stats/power.html).

**Calculating power**:

<img width="778" alt="screen shot 2017-11-13 at 12 20 53 pm" src="https://user-images.githubusercontent.com/22335838/32747287-24db6030-c86d-11e7-8cc9-bd0a2aabf132.png">

*Conclusion: I can detect a variety of effect sizes at very low power*

**Calculating effect size**:

<img width="797" alt="screen shot 2017-11-13 at 12 20 58 pm" src="https://user-images.githubusercontent.com/22335838/32747289-24fc0574-c86d-11e7-89ed-9c8fe15ab6ef.png">

*Conclusion: If I have power, I can detect quite a bit*

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
