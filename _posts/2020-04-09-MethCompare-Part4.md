---
layout: post
comments: true
title: MethCompare Part 4
tags: MethCompare bedtools
---

## Modifying the CpG characterization pipeline

Once I finished [validating genome feature tracks](https://yaaminiv.github.io/MethCompare-Part3/), I decided to turn back to my one true love: characterizing CpGs in files (lol I've spent too much time 1) doing this kind of analysis and 2) alone because pandemic).

### Reformatting the CpG characterization script

After counting overlaps between genome feature tracks and CG motifs, I figured I might as set up code for *P. acuta* characterization in [the Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.ipynb). Originally, I only set up *M. capitata* code to test the pipeline since those were the subsets we had. I broke the script down into multiple steps, and within each step I had planned to switch between species. To differentiate between *M. capitata* and *P. acuta* files, I decided to append the species name to the end of the file. When I tried to set up the code for *P. acuta*, I couldn't figure out how to select only *P. acuta* bedgraphs to add the species name too. At this point, I decided that maybe my method was too complicated and I should just have subfolders for each species. So...that's what I did.

For each species, I first create a species-specific subdirectory. Next, I count the overlaps between CG motifs and each genome feature. I then download coverage files into the subdirectory to proceed with characterizing methylated, sparsely methylated, and unmethylated CpGs and figure out where they're located in the genome. I also removed the methylation island code I had since that didn't really fit with the rest of the script.

### Going forward

2. [Rerun the pipeline with full samples once all samples on `gannet` are validated](https://github.com/hputnam/Meth_Compare/issues/38)
3. [Update code for methylation frequency distribution figure](https://github.com/hputnam/Meth_Compare/issues/39)
4. Figure out how to meaningfully concatenate data for each method
5. [Look into exon annotations in Liew and Li papers](https://github.com/hputnam/Meth_Compare/issues/40)
3. Figure out methylation island analysis

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
