---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 10
tags: killifish RRBS poseidon BAT
---

## Moving forward with summarizing methylation

After meeting with Neel, we decided that a good way forward would be to run samples through `BAT_summarize` since we know that step works. While `BAT_overview` would give us interesting results, we could do that manually if needed. The important step would be getting the `BAT_summarize` output through `BAT_DMRcalling`.

### Testing `BAT_DMRcalling`

Before proceeding, Neel suggested I take the bedGraph from my `BAT_summarize` test and put it through `BAT_DMRcalling` to ensure I can get DMR from the output. If I can't, then there's a bigger problem with the `BAT_summarize` output than I originally thought. I ran the following code to try calling DMR:

```
Singularity> BAT_DMRcalling -q ${ALL_POP}_metilene_N_S.txt -o /scratch/06-DMR/ -a N -b S
[INFO]	Fri Apr 29, 11:36:45, 2022	BAT_DMRcalling v0.1 started
[INFO]	Fri Apr 29, 11:36:45, 2022	Checking flags
[INFO]	Fri Apr 29, 11:36:45, 2022	Calling DMRs.
[INFO]	Fri Apr 29, 11:36:45, 2022	Filtering DMRs.
[INFO]	Fri Apr 29, 11:36:45, 2022	Wrote 0 DMRs with adj. p-value < 0.05, a minimum absolute difference >= 0.1, a minimum length [CpG] >= 10 and a minimum length [nt] >= 0
[INFO]	Fri Apr 29, 11:36:45, 2022	Bedgraph file containing DMR difference: /scratch/06-DMR_qval.0.05.bedgraph
Bed file containing unique DMR identifier and hypo/hypermethylated annotation: /scratch/06-DMR_qval.0.05.bed
[INFO]	Fri Apr 29, 11:36:45, 2022	Plotting DMR statistics (/scratch/06-DMR_qval.0.05.pdf).
```

I checked the output and it looked good, so the weird formatting is only an issue for the R script! This makes me think that the best solution to fix `BAT_overview.R` would be to edit the source code in the singularity container somehow. But that's for another day.

### Finishing `BAT_summarize`

Since the populations are very different genetically, it makes the most sense to investigate oxygen conditions within populations. Comparing populations at the same oxygen conditions would also show how different populations may respond to the same stressor. Neel and I decided on the following contrasts:

1. All New Bedford vs. all Scorton Creek samples, irrespective of oxygen condition (i.e. hypoxia, normoxia, outside control)
2. NB OC vs. SC OC
3. NB normoxia vs. SC normoxia
4. NB hypoxia vs. SC hypoxia
5. NB normoxia vs. NB OC
6. NB normoxia vs. NB hypoxia
7. SC normoxia vs. SC OC
8. SC normoxia vs. SC hypoxia

I originally tried running my code directly in my SLURM script, but ran into the same issue of not being able to assign variables properly. I created [this bash script for `BAT_summarize`](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-BAT-summarize.sh) that I called within [this SLURM script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-analysis.sh) with [this variable definition file](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-analysis-envfile.txt). I ended up with all the output needed, which I saved in [this directory](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/05-analysis/summarize).


### Going forward

1. Figure out `BAT_overview` error ([add line of code to make all column data numerical to BAT_overview.R?](https://stackoverflow.com/questions/67851786/edit-runscript-of-singularity-sif-container-after-building))
2. Investigate any issues with `BAT_filtering` output bedGraphs
3. Write methods and results
4. Obtain other important methylation landscape information
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
