---
layout: post
comments: true
title: DML Analysis Part 27 
tags: virginica MBDSeq IGV
---

## Resolving DML and DMR visualization issues

TL;DR Certain things are a dumpster fire so it's time to put out the dumpster fire.

### Quick overview

I looked at two different chromosomes to see what was happening. Here are some things I learned:

1. The gene background is hot flaming garbage. 

![1-genebackground](https://user-images.githubusercontent.com/22335838/54305681-0813e800-4585-11e9-9978-9907fbb01782.png)

![2-genebackground](https://user-images.githubusercontent.com/22335838/54305682-0813e800-4585-11e9-9a3b-ad8bedabc474.png)

It should just be CG motifs that have 3x coverage or more, but instead it includes multiple loci that aren't cytosines.

2. The DML overlap CG motifs better.

![3-DML](https://user-images.githubusercontent.com/22335838/54305683-0813e800-4585-11e9-95ab-bfed11f481fb.png)

![4-DML](https://user-images.githubusercontent.com/22335838/54305686-08ac7e80-4585-11e9-9f11-e49d8c23bdd7.png)

I mildly trust the DML track.

3. The 100 bp DMR are inconsistent

![7-DMR](https://user-images.githubusercontent.com/22335838/54305692-08ac7e80-4585-11e9-8eff-d0fb94eb5447.png)

![9-DMR](https://user-images.githubusercontent.com/22335838/54305694-09451500-4585-11e9-9f31-ce608b67d47b.png)

Sometimes DMR include mulitple CG motifs or DML, but sometimes they don't. I think using a step size of 100 bp may be influencing this (more on that below).

4. The 1000 bp DMR make no sense

![8-100-1000-DMR](https://user-images.githubusercontent.com/22335838/54305693-08ac7e80-4585-11e9-9df3-2965657305c4.png)

![10-DMR](https://user-images.githubusercontent.com/22335838/54305696-09451500-4585-11e9-85f3-6dc4671232db.png)

The 1000 bp DMR are heavily influenced by regions where one sample is hypermethylated in that region, but other samples aren't methylated there (or have no data). I did find an instance or two where multiple samples were hypermethylated in a 1000 bp DMR, but these were rare over the two chromosomes I looked at.

### [Generate 5x and 10x DML tracks](https://github.com/RobertsLab/resources/issues/613#event-2198949645)

At this point, I mildly trust DML. We're currently using 3x coverage for analyses, but previous papers have used 5x coverage. We decided to look at 5x and 10x DML tracks as well.

In this [R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd), I created 5x and 10x DML bedfiles (found [here](http://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/2019-03-07-Yaamini-Virginica-Repository/analyses/2018-10-25-MethylKit/)). Steven pointed out that he used `destrand = TRUE` in his `unite` command, and I did not. My current stranded output includes + or - indications for forward and reverse strands, but normal discussion of methylated loci does not include strandeness. According to the [`methylKit` manual](https://www.bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html#3_comparative_analysis), `destrand = TRUE` provides better coverage for CpG methylation. I created 5x and 10x coverage tracks using both `destrand = FALSE`, the default, and `destrand = TRUE`. To [visualize everything in my IGV session](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-07-IGV-Verification/2019-03-07-DML-and-DMR-Visualization.xml), I also created 5x sample coverage tracks, found [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2019-03-07-IGV-Verification).

1. Some DML were retained through all coverage types.

![1-all3](https://user-images.githubusercontent.com/22335838/54327380-2569a600-45c7-11e9-96f6-225f7667cf4c.png)

I didn't see much loss going from 3x to 5x DML tracks, but there were more DML lost from 5x to 10x.

2. Some DML were ony present in destranded 5x or 10x tracks.

![3-weird](https://user-images.githubusercontent.com/22335838/54327382-26023c80-45c7-11e9-8708-6fb789a576d5.png)

This is probably a result of the increased coverage in the destranded tracks.

3. I also found something really weird.

![4-weird](https://user-images.githubusercontent.com/22335838/54327383-26023c80-45c7-11e9-8987-5819029e73e4.png)

This CpG had a DML on the forward strand in the 3x, 5x, and destranded tracks, but on the reverse strand in the 10x track. Not sure how this can happen...

### Going forward

1. Figure out which DML track to use for remaining analyses and characterize DML locations in that track
2. Describe methylation irrespective of treatment
3. Work through gene-level analysis
4. Figure out what's going on with DMR
5. Figure out what's going on with the gene background

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
