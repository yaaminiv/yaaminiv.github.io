---
layout: post
comments: true
title: Remaining Analyses Part 4
---

## Investigating those bad samples

Based off of Emma's advice from our [environmental data meeting](https://yaaminiv.github.io/Environmental-Data-Meeting/), I looked at when the technical replicates for samples I removed were run. I found that the second technical replicate for these samples were run just before we changed the column! File 87 was the [first sample file I checked and felt confident with](https://yaaminiv.github.io/SRM-Assay-Day6/) after we changed the column.

| **Sample ID** |        **Site**       | **Habitat** | **Replicate 1** | **Replicate 2** |
|:-------------:|:---------------------:|:-----------:|:---------------:|:---------------:|
|    **O006**   |       Case Inlet      |   Eelgrass  |        24       |        71       |
|    **O014**   |       Case Inlet      |     Bare    |        9        |        85       |
|    **O049**   |      Fidalgo Bay      |   Eelgrass  |        2        |        70       |
|    **O052**   |    Port Gamble Bay    |     Bare    |        3        |        76       |
|    **O071**   |    Port Gamble Bay    |   Eelgrass  |        10       |        69       |
|    **O103**   | Skokomish River Delta |   Eelgrass  |        18       |        81       |
|    **O122**   |      Willapa Bay      |     Bare    |        6        |        77       |
|    **O128**   |      Willapa Bay      |     Bare    |        12       |        80       |
|    **O145**   |      Willapa Bay      |   Eelgrass  |        11       |        68       |

The two samples with the [worst technical replication](https://yaaminiv.github.io/Correlating-Technical-Replicates-Part9/) based on CV filtering are O14 and O128. Both of these samples had 100 transitions where the technical replicates had a coefficient of variance greater than 20. I'm going to give the remaining ctenidia samples to Emma, since her labmate offered to rerun them for me and see if we can replicate the poor technical replication.

**Edit 11/13/17 at 12:10 p.m.**

Based on the conversation in [this issue](https://github.com/RobertsLab/project-oyster-oa/issues/40), I'm going to pick 2 samples I did not remove from my analysis to rerun. This will give us a good idea if the techncial replication issue was a result of the column, or if there was something else involved and we need to rerun the entire set. I'll pick O35 and O131.

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
