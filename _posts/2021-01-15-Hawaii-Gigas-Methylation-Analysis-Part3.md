---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 3
tags: hawaii gigas-ploidy mox trimgalore
---

## Revisiting trimming

After I posted [this issue](https://github.com/RobertsLab/resources/issues/1065), Sam tagged me in [another issue](https://github.com/RobertsLab/resources/issues/1054). Turns out some WGBS data Hollie received recently had the same issue: mainly a poly-G tail in the second paired read, which causes sequences to fail the per sequence GC content metric. When I reviewed the overrepresented sequences in the first paired reads, I noticed that there were still adapter sequences in these reads! When I mentioned this to Sam at Science Hour, he said that `trimgalore` and `cutadapt` usually need to be run twice on a set of samples. This is because only the most abundant primer sequence gets removed the first time, while the remaining adapters get removed the second time. Based on this discussion I decided to:

- Retrim trim all sequences a second time
- Manually remove the poly-G tail

I created [a new script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/01-trimgalore-2.sh). For the second round of trimming, the only part of my code that changed was the paths to the files to trim and the directories used. I included code to run `multiqc` on these files. Next, I set up code to manually trim out poly-G tails, write those files to a different subdirectory, and run `multiqc` on the files that after three rounds of trimming.

In the meantime, Sam said he'll contact Zymo since he sent in the sequencing request to begin with! Between him and Hollie, hopefully Zymo can give us more information.

### Going forward

1. Review MultiQC output after trimming
2. Get more information from Zymo about poly-G tails in samples 
2. Start `bismark`
3. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)
4. Identify methylation analysis framework beyond `methylKit`

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
