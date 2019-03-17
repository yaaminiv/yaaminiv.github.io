---
layout: post
comments: true
title: DML Analysis Part 28 
tags: virginica MBDSeq IGV
---

## Revisiting DML analyses

### Perfecting the DML track

Based on [this issue](https://github.com/RobertsLab/resources/issues/613), I revised the destranded 5x DML track to show methylation differences instead of a false forward strand indiciation. Turns out I applied `mutate` on the wrong dataframe in [my R Markdown script](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd)! My revised track, found [here](http://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/2019-03-07-Yaamini-Virginica-Repository/analyses/2018-10-25-MethylKit/2019-03-13-DML-Destrand-5x-Locations.bed), matched the track Steven generated.

![Untitled 2](https://user-images.githubusercontent.com/22335838/54398600-f8c19700-4677-11e9-9542-4ae670242154.png)

**Figure 1**. DML tracks in IGV with 5x and 10x coverage sample tracks.

This is also interesting because Steven used `filterByCoverage` after `processBismarkAln` to generate the 5x coverage track, while I used `mincov` within `processBismarkAln`. It is interseting to know that we (presumably) get the same DML locations. I decided to go forward with the destranded 5x DML track, since both Claire and Mac used 5x coverage for their analyses.

### CpG information

I returnedd to [my R Markdown script](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd) and got CpG coverage and methylation information for the 5x processed samples. 

![s9-cpgcoverage](https://raw.githubusercontent.com/fish546-2018/yaamini-virginica/master/analyses/2018-10-25-MethylKit/2018-10-25-Loci-Analysis/2018-11-07-Percent-CpG-Coverage-5xCoverage-Sample9.jpeg)

![s9-percentmethylation](https://raw.githubusercontent.com/fish546-2018/yaamini-virginica/master/analyses/2018-10-25-MethylKit/2018-10-25-Loci-Analysis/2018-11-07-Percent-CpG-Methylation-5xCoverage-Sample9.jpeg)

**Figures 2-3**. Example CpG coverage and percent methylation plots.

I also generated a correlation plots, full sample methylation clustering, PCA, and screeplots within `methylKit`.

![full-sample-correlation](https://raw.githubusercontent.com/fish546-2018/yaamini-virginica/master/analyses/2018-10-25-MethylKit/2018-10-25-Loci-Analysis/2019-03-14-Full-Sample-Pearson-Correlation-Plot-Cov5Destrand.jpeg)

![full-sample-clustering](https://raw.githubusercontent.com/fish546-2018/yaamini-virginica/master/analyses/2018-10-25-MethylKit/2018-10-25-Loci-Analysis/2019-03-14-Full-Sample-CpG-Methylation-Clustering-Cov5Destrand.jpeg)

![full-sample-PCA](https://raw.githubusercontent.com/fish546-2018/yaamini-virginica/master/analyses/2018-10-25-MethylKit/2018-10-25-Loci-Analysis/2019-03-14-Full-Sample-Methylation-PCA-Cov5Destrand.jpeg)

![full-sample-screeplot](https://raw.githubusercontent.com/fish546-2018/yaamini-virginica/master/analyses/2018-10-25-MethylKit/2018-10-25-Loci-Analysis/2019-03-14-Full-Sample-Methylation-Screeplot-Cov5Destrand.jpeg)

**Figures 4-7**. Full sample plots.

### Characterizing DML locations

I revisited my [`bedtools` Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb) to characterize the location of destranded 5x DML. I reset the variable path name (side note: HOLY FORK SUCH A HANDY TOOL I'M SO GLAD I USED IT) to the new destranded 5x track. I then went through the script and reran all of my DML code. All of the overlap locations can be found [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-11-01-DML-and-DMR-Analysis). I also reran my `flankBed` and `closestBed` analysis, and saved the output [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-14-Flanking-Analysis).

The track had 630 DML, instead of the 1398 in my 3x coverage track. Of the 630 DML, were 335 hypermethylated in the treatment and were 295 hypomethylated. Most of the DML, 576, were located in mRNA coding (genic) regions, with 1474 genes represented. In the genic regions, 388 DML overlapped with exons and 200 overlapped with introns. 61 DML overlapped with transposable elements generated using all species in the database, and 40 DML overlapped with transposable elements generated using *C. gigas* only. In 1000 bp flanking regions upstream and downstream of mRNA coding regions, 46 DML overlapped with upstream flanks and 50 overlapped with downstream flanks.

**Edit 2019-03-17**:

Steven suggested I count the number of DML that do not overlap with any features (i.e. are in unannotated intergenic regions). To do this, I used the `-v` argument in `intersectBed`, which reports "those entries in A that have no overlap in B. I also specified multiple files with `-b`: exons, introns, and transposable elements identified using *C. gigas* only. I don't need to include mRNA coding regions, since exons and introns are included in that file. I found that 39 DML did not overlap with my feature tracks. I saved the list of DML [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2019-03-17-No-Overlap-DML.txt). 

### Going forward

1. Conduct a proportion test for DML locations
2. Describe methylation irrespective of treatment
3. Update paper methods and results
4. Work through gene-level analysis
5. Figure out what's going on with DMR
6. Figure out what's going on with the gene background

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
