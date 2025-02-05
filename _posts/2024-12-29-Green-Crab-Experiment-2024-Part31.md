---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 31
tags: green-crab-cold
---

## Temperature conditions during the MA experiment

Carolyn suggested I have a plot of the temperature conditions during one of my experiments for my SICB talk to show the audience that the 4 hour timepoint occured during the ramp down! I examined the temperature conditions for the MA experiment in [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/MA/01-temp-conditions-MA.Rmd). I also started doing a similar thing with the WA data in [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/01-temp-conditions-WA.Rmd), but because some of the HOBO loggers had to be switched out it's more complicated so I'm saving that for another time.

The average temperature conditions for each tank can be found in [this file](https://github.com/yaaminiv/cold-green-crab/blob/main/output/MA/01-temp-conditions/temp-mean-sd.csv). The average temperature for the cold treatment was 5.39ºC ± 0.30ºC, and for the colder treatment was 1.66ºC ± 0.32ºC.

I then generated a plot to visualize everything!

<img width="1149" alt="Screenshot 2024-12-29 at 4 28 54 PM" src="https://github.com/user-attachments/assets/dc022fd2-dc23-449a-bc4b-299ca3724d7f" />

**Figure 1**. Temperature conditions for MA data

### Going forward

1. Analyze WA temperature datas
2. Finish genotyping WA samples
1. Troubleshoot QC gel discrepancies by rerunning MA CC samples
2. Redo all Chelex samples?
2. Identify samples for confirmation sequencing
3. Determine methods for comparing population responses
3. Troubleshoot lipid assay protocol
5. Conduct lipid assay for crabs of interest

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
