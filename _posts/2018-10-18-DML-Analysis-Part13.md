---
layout: post
comments: true
title: DML Analysis Part 13
tags: virginica MBDSeq DML methylkit 
---

## IN PROGRESS: Different `mincov` values in `methylKit`

Using [this R Markdown file](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-11-MethylKit-Parameter-Testing.Rmd), I tested the effect of different `mincov` values on sample clustering and DMLs produced. After dicsussing methods in [this issue](https://github.com/RobertsLab/resources/issues/432), I went through this process with both [Steven's samples](http://gannet.fish.washington.edu/seashell/bu-serine-wd/18-04-29/) and [my own samples](http://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/2018-10-04-Bismark-Full-Samples-Revised-Parameters/).

### Steven's samples

All of my output from this analysis can be found [here](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-14-Steven-Samples). Below are some highlights:

**Figures 1-3**. Percent CpG coverage for all samples using a) `mincov = 1` b) `mincov = 3` or c) `mincov = 5`.

**Figures 4-6**. Percent CpG methylation for all samples using a) `mincov = 1` b) `mincov = 3` or c) `mincov = 5`.

**Figures 7-9**. Full sample CpG methylation clustering using a) `mincov = 1` b) `mincov = 3` or c) `mincov = 5`.

**Figures 10-12* PCA of full sample methylation using a) `mincov = 1` b) `mincov = 3` or c) `mincov = 5`.

I also wrote out differentially methylated loci that were at least 50% different between my treatment and control for [`mincov = 1`](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-14-Steven-Samples/2018-10-11-Steven-Samples-Differentially-Methylated-Loci-50-Cov1.csv), [`mincov = 3`](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-14-Steven-Samples/2018-10-11-Steven-Samples-Differentially-Methylated-Loci-50-Cov3.csv), and [`mincov = 5`](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-14-Steven-Samples/2018-10-11-Steven-Samples-Differentially-Methylated-Loci-50-Cov5.csv). I haven't dug into what the exact differences are between these files, but there are at least differences in the number of DMLs produced.

**Table 1**. The `mincov` metric, total number of loci produced, and the number of DMLs that were at least 50% different between treatment andc control samples. More restrictive `mincov` metrics produced less significantly different DMLs.

| **`mincov`** | **Total Loci** | **Number of Significantly Different DMLs** |
|:------------:|:--------------:|:------------------------------------------:|
|       1      |     1112085    |                    4904                    |
|       3      |     670301     |                    1398                    |
|       5      |     503780     |                     816                    |

One thing that was concerning about the pipeline is that I kept getting this error:

`````
glm.fit: fitted probabilities numerically 0 or 1 occurredglm.fit: fitted probabilities numerically 0 or 1 occurredglm.fit: fitted probabilities numerically 0 or 1 occurred
`````

### My samples

I went through the [`bismark` pipeline in my Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-10-04-Bismark-Full-Samples-Revised-Parameters.ipynb) to get my deduplicated and sorted files. Initially I tried using `bismark_methylation_extractor`, but I was unable to extract methylation data for all files before `genefish` ran out of space (again...RIP). I moved all my [large files to gannet](http://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/2018-10-04-Bismark-Full-Samples-Revised-Parameters/) and decided it probably wasn't worth extracting the methylation data from `genefish` since I already have the pipeline running on Mox. If I have some downtime, I can always change the code so I'm running `bismark_methylation_extractor` from gannet.

All output from `methylKit` testing for my samples can be found [here]().

**Figures 13-15**. Percent CpG coverage for all samples using a) `mincov = 1` b) `mincov = 3` or c) `mincov = 5`.

**Figures 16-18**. Percent CpG methylation for all samples using a) `mincov = 1` b) `mincov = 3` or c) `mincov = 5`.

**Figures 19-21**. Full sample CpG methylation clustering using a) `mincov = 1` b) `mincov = 3` or c) `mincov = 5`.

**Figures 22-24* PCA of full sample methylation using a) `mincov = 1` b) `mincov = 3` or c) `mincov = 5`.

{% if page.comments %}

<div id=“disqus_thread”></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page’s canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page’s unique identifier variable
};
*/
(function() { // DON’T EDIT BELOW THIS LINE
var d = document, s = d.createElement(‘script’);
s.src = ‘https://the-responsible-grad-student.disqus.com/embed.js';
s.setAttribute(‘data-timestamp’, +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href=“https://disqus.com/?ref_noscript”>comments powered by Disqus.</a></noscript>

{% endif %}

<script id=“dsq-count-scr” src=“//the-responsible-grad-student.disqus.com/count.js” async></script>
