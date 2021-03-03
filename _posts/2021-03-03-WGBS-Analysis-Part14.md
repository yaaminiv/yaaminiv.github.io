---
layout: post
comments: true
title: WGBS Analysis Part 14
tags: manchester gigas-broodstock bismark multiqc
---

## Reviewing `bismark` output

In the midst of my existential dread and ennui, I never reviewed my [`bismark`](https://github.com/FelixKrueger/Bismark) output! I was able to align samples to the genome, then ran [`multiqc`](https://multiqc.info/) to get summary statistics. I saved `bismark` output [here](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/bismark/), and have HTML reports in [this repo](https://github.com/RobertsLab/project-gigas-oa-meth/tree/master/output/04-bismark).

I then looked at [the MultiQC report](https://nbviewer.jupyter.org/github/RobertsLab/project-gigas-oa-meth/blob/master/output/04-bismark/multiqc_report.html). For all samples, alignment was around 62-63%, which is [consistent with the Hawaii samples](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part5/)! About 25% of the reads for each sample were duplicates that were removed from the alignments. This was greater than the Hawaii samples, but makes sense considering that I was using low-quality DNA from gonad in histology blocks. I noticed that samples 5, 7, and 8 had lower percent duplication than the rest of the samples, which is concerning since those are all from the ambient treatment.

There aren't any big red flags that I can see in the report, so I'll move forward with [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html) while also aligning to the new *C. gigas* genome.

### Going forward

1. Align to [new *C. gigas* genome](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part6/)
2. Identify DML with [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html)
2. Identify SNPs in WGBS data
2. Write methods
3. Write results
3. Identify DML
2. Determine if RNA should be extracted
3. Determine if larval DNA/RNA should be extracted

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
