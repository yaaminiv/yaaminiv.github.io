---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 12
tags: killifish RRBS poseidon BAT
---

## Identifying DMR

Penultimate step for methylation analysis: DMR identification! I ran the following code interactively to make sure I could call DMR and wouldn't [run into another error](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part11/).

```
BAT_DMRcalling \
-q /yaaminiv/killifish-hypoxia-RRBS/output/05-analysis/summarize/all_pop/all_pop_metilene_N_S.txt  \
-o /scratch/06-DMR/all_pop \
-a N \
-b S
```

I still got 0 DMR, but I had slightly more confidence in it knowing that the output files from `BAT_summarize` went through `BAT_overview` well. I created a [script for `BAT_DMRcalling`](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/06-BAT-DMRcalling.sh), [environmental variable file](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/06-DMR-envfile.txt), and [SLURM script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/06-DMR.sh).

My confidence was short-lived...because every single one of my contrasts yielded 0 DMR. This struck me as weird, but potentially related to the poor hierarchical clustering I saw. Even with bad hierarchical clustering, `methylKit` is able to pull out DML, so maybe there's some other setting that needs to be adjusted? I emailed Neel with my questions.

### Going forward

1. Write methods and results
4. Obtain other important methylation landscape information
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
