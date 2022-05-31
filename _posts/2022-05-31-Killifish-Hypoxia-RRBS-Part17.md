---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 16
tags: killifish RRBS poseidon BAT
---

## Removing L4 samples

Given the [weird coverage patterns in L4 samples](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part15/), Neel suggested I run through the workflow without these samples to see if I get different results. If I am able to identify more DMR without these samples (or even if I get different results generally), I will consider excluding them as outliers based on mapping and filtering differences. First, I ran the code with 500 maximum reads per locus, which are the current settings I used to identify DMR with the L4 samples. I also used 100 maximum reads per locus, which were the first settings I tried before experimenting with different filtering options.

#### `BAT_summarize`

I removed these samples from [my `BAT_summarize` code](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-BAT-summarize.sh): 5-S1, 20-N1, 20-S2, OC-S5. Then, I ran `BAT_summarize` and `BAT_overview`. All of the new `BAT_summarize` output is [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/05-analysis/summarize/no-L4), and the `BAT_overview` output is [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/05-analysis/overview). I didn't see a boost in methylation rate after removing those samples when looking through the `BAT_overview` output.

### `BAT_DMRcalling`

**Table 1**. Number of DMR per contrast using different settings

| **Contrast** | **Group 1** | **Group 2** | **MDP_max = 500, L4 included** | **MDP_max = 500, L4 excluded** | ***MDP_max = 100, L4 excluded** |
|:------------:|:-----------:|:-----------:|:------------------------------:|:------------------------------:|:-------------------------------:|
|  All samples |      N      |      S      |                0               |                0               |                0                |
|      OC      |      N      |      S      |                1               |                1               |                0                |
|      20      |      N      |      S      |                1               |                1               |                0                |
|       5      |      N      |      S      |                2               |                2               |                0                |
|       N      |      20     |      5      |               16               |                2               |                0                |
|       N      |      20     |      OC     |                2               |                0               |                0                |
|       S      |      20     |      5      |                1               |                1               |                0                |
|       S      |      20     |      OC     |                1               |                1               |                0                |

Looks like reverting back to 100 reads maximum per locus doesn't yield any DMR, even when removing L4 samples. I'm now unsure if I should proceed with removing L4 samples or not, but I'll ask Neel. But, I will see if any of the DMR I identified without L4 samples are the same as those with L4 samples, because that would lend more importance for those DMR.

### Going forward

<<<<<<< HEAD
1. Annotate DMR locations
2. Revise methylation landscape information
1. Update methods and results
3. Match DMR with RNA-Seq information
2. Start mapping with new genome
2. Try DMR identification with `bismark` and `methylKit`
=======
1. Determine if I should use the new genome for analysis
2. Ask Neel about missing values from group 1 and 2 for `BAT_summarize`
2. Confirm methylated and unmethylated definitions
3. Finish generating genome feature tracks
3. Annotate DMR locations
1. Update methods and results
2. Try DMR identification with `bismark` and `methylKit`
5. Start RNA-Seq analysis
>>>>>>> 312e9d5658fe418b8582f547975316d84240ddb8
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
