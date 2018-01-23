---
layout: post
comments: true
title: Remaining Analyses Part 3
---

## I have significant results?!

In my [boxplot script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-06-Boxplots/2017-11-06-Protein-Area-Boxplots-after-Integration.R), I performed a one-way ANOVA using sites to see if there were significant differences in protein expression for my peptides. I also did a post-hoc Tukey Honest Significant Difference test to see where the differences were coming from. I saved all ANOVA and Tukey HSD p-values [in this .csv file](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-06-Boxplots/2017-11-06-OneWayANOVA-TukeyHSD-by-Site-pValues.csv).

![Annotated-results](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-06-Boxplots/2017-11-06-Annotated-Results.jpeg)

**Figure 1**. Peptides found to be significant in one-way ANOVA, with accompanying significant p-values from Tukey HSD. The p-values are color-coded based on significance at 0.01, 0.05 or 0.10 level.

One interesting thing to note is that just because a peptide was significant doesn't mean that all peptides in that protein were found to have significantly different expression patterns based on site. All of my significant results came from pairwise comparisons between Willapa Bay and the other four sites. I can explore the proteins significant at the 0.05 level and below further when I get my environmental data.

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
