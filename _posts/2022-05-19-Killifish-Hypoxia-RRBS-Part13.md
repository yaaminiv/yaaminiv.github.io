---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 13
tags: killifish RRBS poseidon BAT
---

## Messing around with DMR identification

Originally when I [tried identifying DMR, I got 0 DMR for all contrasts](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part12/). I'm going to mess around with some things to see if I can figure out why I'm not getting any DMR.

### Investigating 0 DMR result

Neel's first suggestion was to see how many methylated regions `metilene` identified between treatment conditions prior to filtering for DMR. When I opened the output files, they were empty! This suggests that the initial methylation region identification failed. I'm not sure if it didn't work because of the input files (potential outliers could warp this) or because of other user-defined settings. One potential setting would be the need for regions to contain at least 10 Cs. The [`BAT_DMRcalling`](https://www.bioinf.uni-leipzig.de/Software/BAT/dmrs/#output_files) manual suggests that this is done post-processing. To confirm this, I reran `BAT_DMRcalling` with `-c 1` to indicate a minimum of 1 C per region. Once again, `metilene` was unable to call methylated regions, so there is an issue with the input files or the `metilene` calling itself.

### Looking at `metilene` input

The next thing I did was look at the differences in mean methylation rates between groups for all my contrasts. It stands to reason that if there aren't large differences in mean methylation, then I'm not going to get any differentially methylated regions. I used the following `awk` commands to identify maximum and minimum methylation rate differences between groups based on [code I found on Stack Overflow](https://stackoverflow.com/questions/41210739/get-maximum-value-of-column):

```
(base) [yaamini.venkataraman@poseidon-l1 all_pop]$ awk -v idx=4 'NR==2 || $idx>max{max=$idx} END{print max}' all_pop_diff_N_S.bedgraph
0.0755
(base) [yaamini.venkataraman@poseidon-l1 all_pop]$ awk -v idx=4 'NR==2 || $idx<min{min=$idx} END{print min}' all_pop_diff_N_S.bedgraph
-0.132166666666667
```

**Table 1**. Maximum and minimum methylation rate differences between groups for all contrasts

| **Contrast** | **Group 1** | **Group 2** | **Maximum Difference** | **Minimum Difference** |
|:------------:|:-----------:|:-----------:|:----------------------:|:----------------------:|
|  All Samples |      N      |      S      |         0.0755         |   -0.132166666666667   |
|      OC      |      N      |      S      |          0.435         |         -0.4175        |
|      20      |      N      |      S      |          0.415         |          -0.6          |
|       5      |      N      |      S      |    0.498333333333333   |          -0.66         |
|       N      |      20     |      5      |    0.646666666666667   |   -0.663333333333333   |
|       N      |      20     |      OC     |         0.5425         |         -0.5975        |
|       S      |      20     |      5      |         0.3525         |          -0.41         |
|       s      |      20     |      OC     |          0.42          |          -0.38         |

With the exception of the all sample comparison, the contrasts do have loci with mean methylation rate differences larger than 0.1. If there are multiple of those loci near eachother, then there probably could be a DMR formed. Perhaps there's an outlier sample I need to remove? I don't know the best way to identify outliers, so I'll talk to Neel about that. Another thing I need to consider is an issue with `metilene` code or the input file.

### Going forward

1. Determine issue with DMR calling
2. Try DMR identification with `bismark` and `methylKit`
2. Write methods and results
4. Obtain other important methylation landscape information
5. Generate genome feature tracks
3. Annotate DMR locations
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
