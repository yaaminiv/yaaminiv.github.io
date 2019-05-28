---
layout: post
comments: true
title: FROGER Recap
tags: FROGER
---

## FROGER Recap

Last week, I hopped over to Rhode Island for the [Functional Re-annotation of Oyster Genomes with Epigenetic Resources (FROGER)](https://github.com/hputnam/FROGER) workshop. Our goals were to 1) use epigenetic resources to annotate the *C. virginica* genome and 2) compare whole genome bisulfite sequencing (WGBS), reduced representation bisulfite sequencing (RRBS), and MBD-BS methods for a single *C. virginica* sample. We expected to write a methods comparison paper while we were there, but data turned out to be really poor-quality for making any useful comparisons.

To contribute to the first goal, I created a long non-coding RNA track with associated exons. I used code I'd been working with for my own analyses and created [this Github-friendly Markdown document](https://github.com/hputnam/FROGER/blob/master/analyses/2019-05-21-lncRNA/2019-05-21-lncRNA-Description.md) with my code. Once I created the lncRNA track, I helped the groups calculating CpGoe ratios for genes, coding sequences, exons, chromatin-associated proteins, 1000 kb windows throughout the genome. I worked with the code [this Markdown document](https://github.com/hputnam/FROGER/blob/master/analyses/CpGoe/20190521_CpGOE_methods.md) and in [this `gannet` directory](https://gannet.fish.washington.edu/spartina/2019-05-21-FROGER/). I was able to test the different code chunks and run most of the pipeline with all five categories. At one point, I ran into an issue trying to use `sed` and `join` functions not available on OSX. [Sam suggested](https://github.com/RobertsLab/resources/issues/694) I download Homebrew and specify `gsed` and `gjoin`. This worked!...but it also caused an issue with `gawk` as I was generating sample-specific CpGoe counts for genes and windows while testing the concatenation code with the CAP tracks. I wrestled with this for a full day until I realized the code still worked when I changed `gawk` to `awk`! Shelly suggested I run the concatenation code with the CAP tracks on `mox`, so she set that up for me. Hopefully, I can finish the rest of the concatenation on `mox` in before I leave for Friday Harbor.

While I was toying with CpGoe code, I was also able to participate in discussions about genome feature tracks and `bismark` and `methylKit` parameters. Jon Puritz posted his code for creating genome feature tracks [here](https://github.com/hputnam/FROGER/wiki/Genome-Sequence-Files-and-Feature-Tracks). He used `complementBed` and `intersectBed` to create an intron track, which is what Sam suggested in [this issue](https://github.com/RobertsLab/resources/issues/692). I'm going to use his code to finalize my *C. virginica* tracks. When mapping the data we did have to the *C. virginica* genome, Mac pointed out that the alignment stringency I set for my samples was potentially too loose. She explained that looking at CHC and CHG methylation rates was important in addition to mapping efficiency. Shelly and I tried to figure out why we both decided on `L,0,-1.2` instead of `L,0,-0.6` (what FROGER used for mapping), but we couldn't remember our conversations. We're going to bring this up at the next lab meeting.

I've never been a part of a working group before, so FROGER was a great experience! It was interesting to see how other people approach similar analyses and learn from a group of experts. I'm looking forward to getting more data and continuing a virtual FROGER working group to conduct the method comparisons.

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
