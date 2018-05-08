---
layout: post
comments: true
title: Gonad Methylation Analysis Part 10
tags: virginica IGV MBDSeq
---

## Visualizing `bismark` outputs

I [started my IGV trials and tribulations yesterday](https://yaaminiv.github.io/Gonad-Methylation-Analysis-Part8/), and today I'm (maybe) struggling a little less.

My goal was to use IGV to visualize 1) my alignment output and 2) bedGraphs from methylation extraction.

### Alignment output

To visualize the alignment output, I first needed to `sort` and `index` the .bam output. I downloaded [`igvtools`](https://software.broadinstitute.org/software/igv/igvtools). In [this lab notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-04-27-Gonad-Methylation-Bismark.ipynb), I applied the `sort` and `index` command to one of my .bam files. I then uploaded this file to my [IGV file](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-05-07-Subset-Alignment.xml).

![untitled](https://user-images.githubusercontent.com/22335838/39776105-bf7f3266-52b4-11e8-8bbb-09a412330878.png)

**Figure 1**. IGV with .bam file.

I had to zoom to the single nucleotide level to view anything!

![untitled5](https://user-images.githubusercontent.com/22335838/39776108-bfac9774-52b4-11e8-85f6-dbaf4f96cebe.png)

**Figure 2**. Alignment at single nucleotide level.

I wanted to add in my other alignment files, but I didn't want to go through the pain of writing individual lines of code to convert the files (Sam tried to help me, but right now there doesn't seem to be a solution. Steven also mentioned that I should have used `deduplicate_bismark` after the alignment, which would have sorted and indexed all my files. You can see the issue [here](https://github.com/RobertsLab/resources/issues/248)).

### bedGraphs

I uploaded all of the bedGraphs from the methylation extractor (found [here]()) into my IGV file. 

![untitled6](https://user-images.githubusercontent.com/22335838/39776109-bfd4e800-52b4-11e8-9df4-615acba0ed1f.png)

**Figure 3**. bedGraphs showing methylation levels for each file.

There isn't any apparent difference between the two treatments. I looked at [Steven's lab notebook](https://sr320.github.io/The-Bismark-boat/) and he noticed the same thing. He also said that going to 100k made a difference. I have no idea what this means so I'll have to ask him.

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
