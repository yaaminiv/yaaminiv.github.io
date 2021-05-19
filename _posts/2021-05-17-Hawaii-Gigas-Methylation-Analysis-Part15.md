---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 15
tags: hawaii gigas-ploidy DML
---

## Investigating methylation data

Quick notebook post to update my progress characterizing this dataset! I followed workflows I established with the Manchester data to characterize the general methylation landscape and understand DML locations.

### Characterizing methylation landscapes

I used [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/06-General-Methylation-Landscape.ipynb) to obtain methylated, sparsely methylated, and unmethylated CpG loci with at least 5x coverage in my sequencing data. [Similar to the Manchester dataset](https://yaaminiv.github.io/WGBS-Analysis-Part26/), I created a union BEDgraph to concatenate percent methylation across samples, then used this union file and individual sequence files for downstream analyses. As I used `intersectBed` to identify methylated, sparsely methylated, and unmethylated loci for 25 files, then determined the genomic location of the loci in those output files, I realized I was producing a lot of intermediate output files! When I go visualize this data, I need to find a way to easily take file line counts and turn into a data table. But that's a problem for future Yaamini.

### DML locations

Now to DML locations! Based on [my `methylKit` results](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part14/), I decided to use a 25% methylation difference to define differential methylation. I first converted the DML lists into BEDfiles in [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/07-Genomic-Location-of-DML.ipynb), and visualized the DML and 5x sample BEDgraphs in [this IGV session](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_07-DML-characterization/dml.xml). I color-coded the samples so that light blue = 2N + high pH, light purple = 3N + high pH, dark blue = 2N + low pH, and dark purple = 3H + low pH. I looked at a couple of the DML and saw the patterns matched pretty well with what I thought a pH- or ploidy-DML should look like. After having a quick look, I used `intersectBed` to find overlaps between pH- and ploidy-DML, and discern the genomic location of DML. Interestingly, only two DML overlapped between treatments! As expected, a majority of DML were found in genic regions. I also looked at overlaps between DML and C/T SNPs I filtered in [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/05-BS-SNPer.ipynb). Again, I need to find an efficient way to take line counts from my `intersectBed` output files and format them in a table!

### Going forward

1. Test-run [DSS](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#34_DMLDMR_detection_from_general_experimental_design) and [ramwas](https://bioconductor.org/packages/release/bioc/html/ramwas.html)
1. Try [EpiDiverse/snp](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
1. Run `methylKit` randomization test on `mox`
5. Investigate comparison mechanisms for samples with different ploidy in oysters and other taxa
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)
6. Update methods
7. Update results
8. Create figures

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
