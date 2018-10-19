---
layout: post
comments: true
title: DML Analysis Part 14
tags: virginica MBDSeq DML methylkit 
---

## Tiling window analysis in `methylKit`

Now that [I'm using `mincov = 3`](https://yaaminiv.github.io/DML-Analysis-Part13/), I wanted to try the [tiling window analysis in `methylKit`](https://bioconductor.org/packages/devel/bioc/vignettes/methylKit/inst/doc/methylKit.html#35_tiling_windows_analysis). I added code for the analysis to the end of [this R Markdown file](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-11-MethylKit-Parameter-Testing.Rmd).

A tiling window analysis will allow me to identify differentially methylated *regions* to complement the [differentially methylated *loci* I previously identified](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-18-Genefish-Samples/2018-10-18-Genefish-Samples-Differentially-Methylated-Loci-50-Cov3.csv). The analysis takes a given window (default is 1000 bp) and adds the number of C and T from each covered cytosine, and spits out a total number of C and T for each tile. I then takes a step (default is 1000 bp) and peforms the same analysis on the next tile.

I chose to do this analysis with three different window-step size combinations:

1. 100 bp window and step size
2. 1000 bp window and step size (the `methylKit` default)
3. 1000 bp window and 100 bp step size

![tiles100](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-19-Tiling-Analysis/2018-10-19-Full-Sample-CpG-Methylation-Clustering-Tiles100.jpeg)

![tiles1000](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-19-Tiling-Analysis/2018-10-19-Full-Sample-CpG-Methylation-Clustering-Tiles1000.jpeg)

![tiles1000step100](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-19-Tiling-Analysis/2018-10-19-Full-Sample-CpG-Methylation-Clustering-Tiles1000-Step100.jpeg)

**Figures 1-3**. Full sample CpG methylation clustering for 1) 100 bp window and step size 2) 1000 bp window and step size and 3) 1000 bp window and 100 bp step size.

![tiles100](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-19-Tiling-Analysis/2018-10-19-Full-Sample-Methylation-PCA-Tiles100.jpeg)

![tiles1000](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-19-Tiling-Analysis/2018-10-19-Full-Sample-Methylation-PCA-Tiles1000.jpeg)

![tiles1000step100](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-19-Tiling-Analysis/2018-10-19-Full-Sample-Methylation-PCA-Tiles1000-Step100.jpeg)

**Figures 4-6**. PCA of full sample methylation for 1) 100 bp window and step size 2) 1000 bp window and step size and 3) 1000 bp window and 100 bp step size.

These clustering plots are slightly different from same kind associated with DMLs:

![cluster](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-18-Genefish-Samples/2018-10-18-Full-Sample-CpG-Methylation-Clustering-Cov3.jpeg)

![PCA](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-18-Genefish-Samples/2018-10-18-Full-Sample-Methylation-PCA-Cov3.jpeg)

**Figures 7-8**. Full sample CpG methylation clustering and PCA of full sample methylation using `mincov = 3`.

When calcuating differential methylation, I didn't get any error about a glm not fitting. I did get an error when I was calculating differential methylation for individual loci.

**Table 1**. Window size, step size, total number of regions produced, and the number of DMLs that were at least 50% different between treatment and control samples. The number of regions and siginificantly different DMRs seem to be dictated by the window size, and not the step size.

| **Window Size (bp)** | **Step Size (bp)** | **Total Regions** | **Number of Significantly Different DMRs** |
|:--------------------:|:------------------:|:-----------------:|:------------------------------------------:|
|          100         |         100        |       217538      |                     162                    |
|         1000         |        1000        |       104144      |                     118                    |
|         1000         |         100        |       104144      |                     118                    |

I wrote out DMRs for each method in individual .csv files:

- [100 bp window and step size](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-19-Tiling-Analysis/2018-10-19-Genefish-Samples-Differentially-Methylated-Loci-50-Cov3-Tiles100.csv)
- [1000 bp window and step size](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-19-Tiling-Analysis/2018-10-19-Genefish-Samples-Differentially-Methylated-Loci-50-Cov3-Tiles1000.csv)
- [1000 bp window and 100 bp step size](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-10-11-MethylKit-Parameter-Testing/2018-10-19-Tiling-Analysis/2018-10-19-Genefish-Samples-Differentially-Methylated-Loci-50-Cov3-Tiles1000-Step100.csv)

### Going forward

I think it makes sense to have smaller-sized DMRs, so I'd rather continue wiht the 100 bp windows and steps. The 100 bp window and step PCA also has the best separation between ambient and treatment samples. I posted [this issue]() to get Steven's opinion.

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
