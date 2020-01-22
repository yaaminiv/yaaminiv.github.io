---
layout: post
comments: true
title: DML Analysis Part 11
tags: virginica MBDSeq DML bismark methylKit
---

## Testing `methylKit` parameters

But first, I quick side story about `bismark`:

### `bismark`

I wanted to `bismark` on my full samples in [this Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-10-04-Bismark-Full-Samples-Revised-Parameters.ipynb). However, when I tried the methylation extractor step, I ran into [this issue](https://github.com/RobertsLab/resources/issues/414). Turns out the hummingbird hard drive was maxed out. I spent the past few days [moving large files](https://genefish.wordpress.com/2018/10/11/update-to-project-virginica-oa-repository-contents/) to [this gannet directory](http://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/). I then removed the files from my local repository (and hummingbird). There are two takeaways from this process:

1. All large files should live on an external server. No point keeping them in a local repository if they cannot be synced with Github
2. I should run `bismark` on Mox

Before starting in on Mox and armed with more storage, I thought I could play around with `methylKit` on hummingbird. I finished the `bismark` pipeline [in this notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-10-03-Bismark-Parameter-Testing.ipynb).

### `methylKit`

There are two things I need to test in `methylKit`:

1. **Different coverage metrics**: [Previously](https://yaaminiv.github.io/Gonad-Methylation-Analysis-Part18/), I used a minimum coverage of 1. That is not good! Steven said to try 3x coverage, and after my [PCSGA talk](), Mac suggested I try 5x coverage. I’ll try 1x, 3x, and 5x coverage with the [new `-score_min` parameter](https://yaaminiv.github.io/DML-Analysis-Part10/) and see how it affects clustering.

2. **Tiling window analysis**: The [`methylKit user guide`](http://bioconductor.org/packages/3.7/bioc/vignettes/methylKit/inst/doc/methylKit.html#35_tiling_windows_analysis) has an in-built tiling window analysis that can pick out differentially methylated *regions*. So far, I’ve picked differentially methylated *loci*. Having DMRs will improve our exploratory analysis.

### Different coverage metrics

I started in on testing different coverage in [this R Markdown file](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-11-MethylKit-Parameter-Testing.Rmd). Plots depicting Percent CpG methylation and coverage for each file can be found [in this folder](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-10-11-MethylKit-Parameter-Testing). I think it could be useful to create a multipanel plot to display this in the future.

Because the subsets do not have enough data, I couldn't test 5x coverage. I also couldn't get any farther than creating methylation and coverage plots.

### Going forward

Here are my priorities:

1. Become a registered user on Mox
2. Create a script to run `bismark` on the full samples
3. Find other `-score_min L,0,-1.2` data to play around with for `methylKit` parameter testing
4. Figure out code for some multipanel plots
5. Figure out tiling

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
