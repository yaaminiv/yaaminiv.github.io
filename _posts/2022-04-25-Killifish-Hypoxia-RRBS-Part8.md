---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 8
tags: killifish RRBS poseidon BAT
---

## Methylation extraction results

My `BAT_calling` job finished! It ran for about five days (not continuously) and processed about 4-5 samples with `BAT_calling` each day. On the last day, four samples were processed with `BAT_calling` and all samples were processed with `BAT_filtering`.

### `BAT_calling` output

For each sample, `BAT_calling` produced VCF files all methylation information in that sample, as well as a log file documenting progress. These logs don't have any interesting information, but they're now in [this folder](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/04-calling/called). The VCF files are in my home directory on `vortex`.

### `BAT_filtering` output

The more interesting output from this step in the workflow came from `BAT_filtering`. I filtered methylation data extracted with `BAT_calling` for CG methylation only, minimum 10 reads, maximum 100 reads, and a minimum methylation rate of 10%. Filtered CpG information was saved as a VCF and a BEDgraph. This step also produced pdfs for each sample with coverage and methylation information for all CpGs and filtered CpGs. The BEDgraphs and pdfs can be found [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/04-calling/filtered). One thing I may want to do when writing these results is to create similar plots for a combined BEDgraph with all my samples, so I can describe the broad methylation landscape. I may also want to get numbers for coverage and methylation rate for each sample instead of relying on the pdfs.

When looking at the coverage plot for filtered CpGs, I noticed that L4 samples had coverage plots that were similar between samples, but different from L2 and L3 samples. I think these may be differences in coverage by lane?

![Screen Shot 2022-04-25 at 11 51 30 AM](https://user-images.githubusercontent.com/22335838/165154385-d9dcb63a-05d7-467c-b270-c44d074d7d3c.png)

![Screen Shot 2022-04-25 at 11 51 10 AM](https://user-images.githubusercontent.com/22335838/165154391-8fc2fb2c-bd8a-4ed9-8852-3b1a420fe29d.png)

![Screen Shot 2022-04-25 at 11 50 50 AM](https://user-images.githubusercontent.com/22335838/165154399-a144214d-091f-4b33-b619-12549e38a228.png)

**Figures 1-3**. Coverage for filtered CpGs for representative L2, L3, and L4 samples.

L4 samples include those from SC hypoxia, and normoxia and outside controls for both populations. The [sample that was corrupted was from L4](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part2/), so there may be something going on with that lane in general. Fewer samples were sequenced on L4 compared to L2 and L4. I looked at [mapping statistics](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/03-mapping/stat/mapping-statistics.csv) for L2 and L3 vs. L4. The only thing that set L4 apart was a lower percentage of mapped multiple pair ends.

### Going forward

1. Ask Neel about differences between L4 and L2/L3
2. Review next BAT module
2. Run `BAT_analysis`

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
