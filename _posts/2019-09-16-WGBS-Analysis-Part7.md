---
layout: post
comments: true
title: WGBS Analysis Part 7
tags: manchester gigas-broodstock
---

## Further DML annotations

After my practice talk, I returned to annotating DML output because I need some sort of gene information to present at PCSGA. In [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb), I downloaded [this `blast` output](https://eagle.fish.washington.edu/cnidarian/oyster_v9_Gene_node2_blastout), originally hosted at [this OWL link](http://owl.fish.washington.edu/halfshell/bu-wikispaces/genefish/mainSpace/crassostreome.html). Unlike the previous `blast` output I tried working with, it's not truncated. I merged the `blast` output with my DML-gene overlaps using `awk`, since I couldn't get a `join` command to work. I then tried assigning GOslim terms to GOterms, but I encountered a script error. At this point, it was 10:30 p.m. and I needed to finish my talk, so I'll return to this before WSN.

### Going forward

6. REVISE MY TALK
7. Present preliminary results at PCSGA
8. Figure out how to clean up results for WSN and ASLO
9. Potentially sequence more samples...?
10. Return to *C. virginica* data and finish. that. paper.

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
