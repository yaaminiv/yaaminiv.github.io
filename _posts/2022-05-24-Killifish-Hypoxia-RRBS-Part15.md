---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 15
tags: killifish RRBS poseidon BAT
---

## Revised DMR identification

I'm moving forward with 10 reads minimum and 500 reads maximum for `BAT_filter_vcf` and default settings for `BAT_DMRcalling`. Time to apply these settings to all my data!

### `BAT_filter_vcf`

I updated [my code](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/04-BAT-calling-filtering.sh) to include the revised filtering settings. The reason I still want to include a maximum for number of reads is because I wanted to avoid regions with excessive PCR duplication. Once I obtained filtered bedGraphs, I sorted them and remove extra chromosome columns.

The revised filtering output can be found in [this folder](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/04-calling/filtered). I'm still seeing the weird coverage graphs in L4 samples when compared to others:

**Figures 1-2**. Coverage for filtered CpGs in L3 and L4 samples

![Screen Shot 2022-05-24 at 12 08 34 PM](https://user-images.githubusercontent.com/22335838/170082342-ea5df6dd-7618-4559-b281-34caace3266f.png)

![Screen Shot 2022-05-24 at 12 08 29 PM](https://user-images.githubusercontent.com/22335838/170082341-3f206b20-8ccf-480d-942a-340ab48c2bdf.png)

I'm a bit concerned about using 500 reads max at an individual locus given the weird L4 graphs, but I'll go through the rest of the workflow and decide later. It may be good to scale down the number of reads (ex. 200, 300, 400) and see if I can still call these DMR while being more conservative if I am concerned about the pattern in L4 samples.

### `BAT_summarize` and `BAT_overview`

The output for `BAT_summarize` and `BAT_overview` with the re-filtered samples can be found [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/05-analysis). Looking at the pdfs, I didn't see anything notable beyond average methylation differences between treatments.

### `BAT_DMRcalling`

Now the good stuff...DMR! The output for every contrast can be found in [this folder](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/06-DMR).

**Table 1**. Number of DMR per contrast

| **Contrast** | **Group 1** | **Group 2** | **DMR** |
|:------------:|:-----------:|:-----------:|:-------:|
|  All samples |      N      |      S      |    0    |
|      OC      |      N      |      S      |    1    |
|      20      |      N      |      S      |    1    |
|       5      |      N      |      S      |    2    |
|       N      |      20     |      5      |    16   |
|       N      |      20     |      OC     |    2    |
|       S      |      20     |      5      |    1    |
|       s      |      20     |      OC     |    1    |

I'm not surprised there were no DMR identified when all samples were compared between populations: that introduced a lot of variability. Now that I have a list of DMR, the next step is to view them in IGV and determine if the DMR are adjacent to any genes or other genome features.

### Going forward

1. Update methods and results
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
