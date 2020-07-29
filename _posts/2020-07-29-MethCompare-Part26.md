---
layout: post
comments: true
title: MethCompare Part 26
tags: MethCompare bedtools
---

## DMC locations and cleaning up scripts

At our last meeting, we decided we wanted location information for DMC. I previously ran that analysis in [this script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Identifying-Genomic-Locations.ipynb), but since then I've updated the genome feature tracks and Mac has updated the DMC lists! Mac posted her revised DMC BEDfiles in [this issue](https://github.com/hputnam/Meth_Compare/issues/90) so I could characterize locations. Although she was mainly interested in two different DMC lists, I decided it was easy enough to run the analysis for all of them in case we wanted that information later.

The first thing I did was remove the old analysis BEDfiles and output from a different directory. Then, I changed the paths to the BEDfiles to the correct directory and modified my wildcards and loops to ensure the revised files were the only ones being used. Once I confirmed my old code still worked with the *P. acuta* DMC lists, I used the same code for *M. capitata*. I added all generated BEDfiles to the `.gitignore` and created lists of overlap counts for each genome feature.

I took those count files and generated tables in [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Identifying-Genome-Features-Summary.Rmd):

*M. capitata*:

- [DMC location counts](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Mcap/Mcap-DMC-Feature-Overlap-counts.txt)
- [DMC location percents](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Mcap/Mcap-DMC-Feature-Overlap-percents.txt)

*P. acuta*:

- [DMC location counts](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-DMC-Feature-Overlap-counts.txt)
- [DMC location percents](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-DMC-Feature-Overlap-percents.txt)

I also included the links to these tables in the issue for Mac to look at. While in the script, I also removed the stacked barplots I created for the upset plot data and the associated code. At this point, it's all supplemental material and having that information in a table format is probably the only way it will be useful. To round out my scritp cleaning I followed the instructions in [this issue](https://github.com/hputnam/Meth_Compare/issues/70) to update the [scripts README.md](https://github.com/hputnam/Meth_Compare/blob/master/scripts/README.md). I moved my scripts to the bottom since it's more important to run QC information before looking at CpG methylation status and genomic location.

### Going forward

2. Help write mansucript!
2. Look into program for mCpG identification

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
  
