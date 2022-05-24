---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 14
tags: killifish RRBS poseidon BAT
---

## Revisiting DMR identification

After talking to Neel, we decided to go back to `BAT_filter_vcf` step and see if changing some of the filtering settings would lead to DMR identification. By imposing more lenient restrictions at the filtering step, I may have more loci available for `metilene` to construct methylated regions.

### Checking code

Before I proceeded with re-filtering my called samples, I checked [the `BAT_summarize`](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-BAT-summarize.sh) code to make sure I didn't mix any samples up! I reviewed the code and fortunately (or unfortunately?) there were no obvious mistakes. I also reviewed the [pdf output from `BAT_overview`](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/05-analysis/overview) to see if the samples sequenced on L4 clustered together. Since I noticed they had more truncated patterns than the other samples, I wanted to make sure the truncation wasn't impacting my ability to call DMR. I didn't see any L4 samples cluster together in the dendograms.

### Refiltering samples

Alright, now that I checked everything else, it was time to re-filter samples. I tried the following settings:

- MDP_min = 10, MDP_max = 100
- MDP_min = 10
- MDP_min = 10, MDP_max = 500
- MDP_min = 10, MDP_max = 1000

Once samples were re-filtered, I ran them through `BAT_summarize`and `BAT_DMRcalling`. I used four contrasts for these tests: 20 vs. 5 and 20 vs. OC within each population. For `DMR_calling`, I used a minimum of 1 CpG per DMR just to see if I could get any methylated region detected first, then changed those settings if I identified DMR.

**Table 1**. Number of DMR identified with different filtering and DMR identification settings

| **Contrast** | **Group 1** | **Group 2** | **MDP_min = 10, MDP_max = 100, MR_min = .10, c = 10** | **MDP_min = 10, MDP_max = 100, MR_min = .10, c = 1** | **MDP_min = 10, c = 1** | **MDP_min = 10, c = 5** | **MDP_min = 10, c = 10** | **MDP_min = 10, MDP_max = 100, c = 1** | **MDP_min = 10, MDP_max = 500, c = 1** | **MDP_min = 10, MDP_max = 500, c = 5** | **MDP_min = 10, MDP_max = 500, c = 10** |
|:------------:|:-----------:|:-----------:|:-----------------------------------------------------:|:----------------------------------------------------:|:-----------------------:|:-----------------------:|:------------------------:|:--------------------------------------:|:--------------------------------------:|:--------------------------------------:|:---------------------------------------:|
|       N      |      20     |      5      |                           0                           |                           0                          |            16           |            16           |            16            |                    0                   |                   16                   |                   16                   |                    16                   |
|       N      |      20     |      OC     |                           0                           |                           0                          |            2            |            2            |             2            |                    0                   |                    2                   |                    2                   |                    2                    |
|       S      |      20     |      5      |                           0                           |                           0                          |            1            |            1            |             1            |                    0                   |                    1                   |                    1                   |                    1                    |
|       s      |      20     |      OC     |                           0                           |                           0                          |            1            |            1            |             1            |                    0                   |                    1                   |                    1                   |                    1                    |

Reviewing my [DMR output](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/06-DMR), it's clear that the same DMR were called regardless of the settings! Now that I know there are DMR in these datasets, I want to finalize my `BAT_filter_vcf` and `BAT_DMRcalling` settings so I can apply them to the other four contrasts.

### Going forward

1. Finalize `BAT_filter_vcf` and `BAT_DMRcalling` settings
2. Run all data through `BAT_filter_vcf`, `BAT_summarize`, `BAT_overview`, and `BAT_DMRcalling`
2. Update methods and results
4. Obtain other important methylation landscape information
5. Generate genome feature tracks
3. Annotate DMR locations
2. Try DMR identification with `bismark` and `methylKit`
5. Start RNA-Seq analysis
6. Create OSF repository for all intermediate files

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
