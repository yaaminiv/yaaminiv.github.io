---
layout: post
comments: true
title: DML Analysis Part 26
tags: virginica MBDSeq IGV
---

## Visualizing DML and DMR

Steven [asked me](https://github.com/RobertsLab/resources/issues/602) to visualize the DML and DMR files in IGV with the gene background and 3x and 10x coverage files for each sample. This way, we could check the integrity of DML and DMR.

### Tiling window analysis

Based on a cursory look at DMR locations, sometimes DMR were present even though there were no DML. We wondered if this was an artifact of the 100 bp size of the DMR. I went back to my [R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd) and redid a tiling window analysis. I [previously tested window size]() with `bismark` data from genefish. The 1000 bp window size, the `methylKit` default, produced less DMR than the 100 bp size, which is why I went with the 100 bp to begin with. I identified 1000 bp regions that were at least 50% different between treatments. I wrote out these regions as a [csv file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-Tiling-Analysis/2019-03-07-Differentially-Methylated-Regions-50-Cov3-Tiles1000.csv) and [BEDfile](http://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/2019-03-07-Yaamini-Virginica-Repository/analyses/2018-10-25-MethylKit/2019-03-07-1000bp-DMR-Locations.bed).

### Generating coverage tracks

I couldn't use bedgraphs `bismark` generated because they are only 1x coverage. In [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-03-07-Generating-Coverage-Tracks.ipynb) I generated coverage files needed to visualize sample methylation in IGV. First, I downloaded `cov.gz` files from gannet using `wget`. After unzipping the files, I used a for loop to sum coverage across loci in each sample, then isolate areas with 3x or 10x coverage. I saved the coverage files in [this folder](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2019-03-07-IGV-Verification).

### Visualization in IGV

I added the following tracks to [this IGV session](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2019-03-07-IGV-Verification):

- 3x coverage files for each sample
- 10x coverage files for each sample
- DML track
- 100 bp DMR track
- 1000 bp DMR track
- `methylkit unite` gene background

### Going forward

1. Go through this IGV file and verify DML and DMR!
2. Decide if DML or DMR should be a priority
3. Decide between 100 and 1000 bp DMR

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
