---
layout: post
comments: true
title: Mass Spec Updates
tags: DNR mass-spec DIA
---

## Trouble struck at 8:16 p.m. on January 28.

I checked the progress of the mass spectrometry analysis on TeamViewer and saw that the blank that had just finished running had the weirdest looking raw data.

*Cue the horrible iPhone photos used in text conversations induced with slight panic*

![weirdblank](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/DNR/Lab-Notebook/massspecupdatejan28/weirdblank.JPG)

For reference, the previous blanks looked like this:

![previousblank](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/DNR/Lab-Notebook/massspecupdatejan28/previousblank.JPG)

I checked the previous sample, just to make sure nothing went wrong. It looked fine!

![previoussample](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/DNR/Lab-Notebook/massspecupdatejan28/previoussample.JPG)

The pressure also looked fine.

![pressure](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/DNR/Lab-Notebook/massspecupdatejan28/pressure.JPG)

So I checked the current sample that was running, and it was also doing something wonky in the midst of data collection.

![currentsample](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/DNR/Lab-Notebook/massspecupdatejan28/currentsample.JPG)

I talked with Emma and she agreed to check the progress in about 45 minutes in case things had changed.

*45 minutes later...*

The data collection seemed to be going smoothly, so I agreed to check the sample when it was done. If the blank that runs after that sample looked weird, or if the sample looked weird, then I would clear the active queue and stop running the samples.

*1 hour later...*

When I checked the sample again, data was collected, but it didn't seem to fully collect in the left tail of the distribution. Emma believes that there was a bubble in the column, so I will run this sample again.

![finalcurrentsample](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/DNR/Lab-Notebook/massspecupdatejan28/finalcurrentsample.JPG)

The current blank also seemed like it would follow the pattern of previous blanks, so all is good! 

*The next morning...*

Laura and I added 60 µL of ACN to the blank tube just in case. Everything ran smoothly overnight, so fingers crossed there are no more bubbles in the column!

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
