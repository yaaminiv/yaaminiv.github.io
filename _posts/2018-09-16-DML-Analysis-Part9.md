---
layout: post
comments: true
title: DML Analysis Part 9
tags: virginica MBDSeq DML gene-enrichment compGO
---

## Gene Enrichment Results from `compGO`

Mike fixed the [gene enrichment tool](https://meta.yeastrc.org/compgo_yaamini_oyster/pages/goAnalysisForm.jsp), so I got to use it! Good thing, becuase ReVIGO is currently down and I cannot generate gene enrichment visualizations any other way. All of this information can be found in [my R Markdown analysis notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-06-14-Gene-Enrichment-Analysis.Rmd).

The website expects inputs in the following format: sp|Q9C098|DCLK3_HUMAN

This means I don't need to unfold the specific code from anything. The tool generates both reports and images, all of which can be found in [this folder](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-16-compGO-Output). I can also do comparisons between two different lists, which is useful. I used a p-value of 1e-2, the default, for each enrichment.

### DML-mRNA overlaps

![bp](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-16-compGO-Output/compGO-DML-mRNA-BP-p1E-2.png)

![cc](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-16-compGO-Output/compGO-DML-mRNA-CC-p1E-2.png)

![mf](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-16-compGO-Output/compGO-DML-mRNA-MF-p1E-2.png)

**Figures 1-3**. Gene enrichment visualizations for biological processes, cellular components, and molecular functions.

### Exon and intron Uniprot codes

To get Uniprot codes for DML-Exon and DML-Intron overlaps, I used [`intersectBed`](https://bedtools.readthedocs.io/en/latest/content/tools/intersect.html) with the `-wao` argument to find overlaps between these two files and the mRNA gff. `-wao` allowed me to write out the overlaps between the DML-Exon or DML-Intron file with the mRNA and count the number of overlaps. If there was no overlap (which was unlikely), then the DML-Exon or DML-Intron file information was written out with NULL to indicate that particular exon or intron had no overlap in the mRNA file. I looked at the DML-Exon file in particular, and found it was really large. There seem to be a bunch of repeating lines. Not sure if that's just because several genome chunks lead to different mRNAs, and thus, different exon patterns, or if there was a problem with the `-wao` argument. This is something I will need to clean up after PCSGA.

### DML-Intron overlaps

![bp](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-16-compGO-Output/compGO-DML-Intron-BP-p1E-2.png)

![cc](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-16-compGO-Output/compGO-DML-Intron-CC-p1E-2.png)

![mf](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-16-compGO-Output/compGO-DML-Intron-MF-p1E-2.png)

**Figures 4-6**. Gene enrichment visualizations for biological processes, cellular components, and molecular functions.

### DML-Exon overlaps

![bp]()

![cc]()

![mf]()

### Biological processes comparison

![mrna-intron](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-16-compGO-Output/compGO-mRNA-Intron-Differences-BP-p1E-2.png)

![mrna-exon]()

![exon-intron]()

### Going forward


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
