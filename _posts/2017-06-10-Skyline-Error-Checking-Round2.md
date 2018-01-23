---
layout: post
comments: true
title: Skyline Error Checking Round 2
tags: DNR DIA skyline
---

## This time it only took 4 hours!

Due to a combination of not working late at night, using the same proteins and transitions as [last time](https://yaaminiv.github.io/Skyline-Error-Checking/), and knowing what good and bad peaks look like, I was able to spot check 100 peptides in the [revised Skyline document](https://yaaminiv.github.io/Skyline-Attempt-3/). Because I was in a rush to finish and head to the airport, I didn't take any side-by-side comparison pictures. I do however, have revised error rates.

Ovearll, I think the new .blib improved Skyline peak selection. I haven't compared error rates so I don't have anything quantitative to base my assertion on, but I have a few observations:

- More instances of a peak being chosen correctly for all replicates
- Way less missing peaks
- Fewer noisy peak selections

The previous error checking document is [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_Skyline_20170512/error-checking/2017-05-13-error-checking.txt), revised error checking document can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_Skyline_20170524/error-checking/2017-06-10-error-checking.xlsx).

After error checking, I re-exported my Skyline reports.

![image-1](https://user-images.githubusercontent.com/22335838/27007885-1f7d493a-4e17-11e7-9933-7789ae1e47e0.png)

[Proteins and peak areas only](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170524/2017-06-10-protein-areas-only-error-checked.csv)

![image-2](https://user-images.githubusercontent.com/22335838/27007886-1f7e2ff8-4e17-11e7-8f90-831fd16a41b1.png)

[MSstats document, replicate not pivoted](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170524/2017-06-10-peptide-transition-results-MSstats-no-pivot-error-checked.csv)

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
