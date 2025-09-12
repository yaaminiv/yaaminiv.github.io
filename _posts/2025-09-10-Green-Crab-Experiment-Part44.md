---
layout: post
comments: true
title: Green Crab Experiment Part 44
tags: green-crab TTR respirometry
---

## Minor revisions to TTR and respirometry analysis

We're going to pretend that I've actually thought about this since last December...YIKES. Anyways, today I revisited my code to ensure I actually accounted for demographic variables.

Turns out I did account for demographic differences in my [TTR code](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/03-TTR-analysis.Rmd)! I tossed all variables in my model and then used a backwards deletion approach to pick the best fit model. The best model for TTR included day and weight even though they were non-significant, so I just made sure that the model without those variables didn't have a lower AIC. I also used `emmeans` to conduct post-hoc tests for temperature. I guess they aren't strictly necessary for temperature since the model compares 13ºC vs. either 30ºC or 5ºC, but it did give me a value to compare 30ºC vs. 5ºC. The output from the post-hoc test can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/03-TTR-analysis/tukey-test-treatment.csv). Based on the statistical output, I made some slight changes to the statistical information in the figure. Initially I thought I could add some letters on top of the boxplot to distinguish differences, but because there was no treatment x day interaction, the visualization may be a bit harder to digest. So we're sticking with text in the white space.

<img width="1591" height="1236" alt="Image" src="https://github.com/user-attachments/assets/291c5471-eeee-4e7c-88fc-b79528c281e3" />

**Figure 1**. TTR figure

I also accounted for demographic differences in my [respirometry analysis](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/04-respirometry-analysis.Rmd)! The most parsimonious model was one that included treatment and day, but not the interaction nor any other demographic variables. I also conducted posthoc tests, and saved the output showing differences by [treatment](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/04-respirometry-analysis/tukey-test-treatment.csv) and [day](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/04-respirometry-analysis/tukey-test-day.csv). Again, adding letters to the figure for these post-hoc would be more confusing since there wasn't an interaction, so I settled for updating the stats on the figure. 

<img width="1590" height="1234" alt="Image" src="https://github.com/user-attachments/assets/f90bb0a6-c644-493a-aa3b-197c8e610a7d" />

**Figure 2**. Respirometry figure

And that's that! Next up: adding PERMDISP to my metabolomics and lipidomics analyses.

### Going forward

1. Add PERMDISP to clarify effects are due to centroid variance and not spread
2. Integrate physiology and -omics data with WCNA
3. Metabolomics and lipidomics DIABLO analysis
4. Outline discussion
5. Write discussion
5. Write abstract

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
