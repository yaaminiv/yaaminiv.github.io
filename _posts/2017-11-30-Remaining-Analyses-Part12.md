---
layout: post
comments: true
title: Remaining Analyses Part 12
---

## Some more regressions

I tried to write a for loop in [this R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Biomarker-Scatterplots/2017-11-06-Peptide-Abundance-versus-Biomarker-Scatterplots.R) to make a table with each peptide vs. biomarker comparison, R-squared value, and slope......but I'm hardcore struggling with it. I wrote a for loop within a for loop to create all of the plots, but now I can't write another for loop to take all of the information I'm generating and put it in a new dataframe. I'm going to keep trying though!

After looking at my [peptide vs. biomarker regressions](https://yaaminiv.github.io/Remaining-Analyses-Part11/), Steven suggested I make the same plots for each site. I used the R script linked above to do that. The plots can be found in these folders:

[Case Inlet](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Biomarker-Scatterplots/2017-12-01-Case-Inlet-Scatterplots)

[Fidalgo Bay](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Biomarker-Scatterplots/2017-12-01-Fidalgo-Bay-Scatterplots)

[Port Gamble Bay](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Biomarker-Scatterplots/2017-12-01-Port-Gamble-Scatterplots)

[Skokomish River Delta](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Biomarker-Scatterplots/2017-12-01-Skokomish-River-Scatterplots)

[Willapa Bay](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Biomarker-Scatterplots/2017-12-01-Willapa-Bay-Scatterplots)

Once I figure out how to get my triple for loop to work, I'll make a table for this information too. Now I guess I'll wait for our meeting with Micah and Alex to see what to do next.

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
