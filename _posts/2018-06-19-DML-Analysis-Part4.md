---
layout: post
comments: true
title: DML Analysis Part 4
tags: virginica MBDSeq DML bedtools
---

## flankBed 101

It's not enough to understand where DMLs or CG motifs intersect with exons, introns and mRNAs! Regions up or downstream from mRNA regions may contain promoters or other transcription factors. These areas could also be regulated by methylation. To explore this, I identified [`flank` in `bedtools](http://bedtools.readthedocs.io/en/latest/content/tools/flank.html) [in this issue](https://github.com/RobertsLab/resources/issues/291) as a method to add 1000 bp to the beginning and end of an mRNA region. 

Before I could do this successfully, I had to [create a "genome file"](https://github.com/RobertsLab/resources/issues/294). For `flankBed`, the genome just needed to be a tab-delimited file with the chromosome name, start and stop positions. I used [NCBI information](https://www.ncbi.nlm.nih.gov/genome/gdv/browser/?context=genome&acc=GCF_002022765.2) to create the ["genome file"](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-11-DML-Analysis/2018-06-15-bedtools-Chromosome-Lengths.txt) in TextWrangler.

I used the following code in my [Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-06-11-DML-Analysis.ipynb) to generate the flanks:

`````
! /Users/Shared/bioinformatics/bedtools2/bin/flankBed 
-i C_virginica-3.0_Gnomon_mRNA.gff3 
-g 2018-06-15-bedtools-Chromosome-Lengths.txt 
-b 1000 \
> 2018-06-15-mRNA-1000bp-Flanks.bed
`````

I had [an issue](https://github.com/RobertsLab/resources/issues/296) running the code over the weekend, since I couldn't get any output. Once I restarted Jupyter and reran the code, it worked!

I took my [`flankBed ` output](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-11-DML-Analysis/2018-06-15-mRNA-1000bp-Flanks.bed) and ran it through `intersectBed` to find overlaps between the output, DMLs, and CG motifs.

`````
! /Users/Shared/bioinformatics/bedtools2/bin/intersectBed \
-wb \
-a 2018-06-15-mRNA-1000bp-Flanks.bed \
-b ../2018-05-29-MethylKit-Full-Samples/2018-05-30-DML-Locations.bed \
> 2018-06-19-mRNA-100bp-Flanks-DML.txt
`````

`````
! /Users/Shared/bioinformatics/bedtools2/bin/intersectBed \
-wb \
-a 2018-06-15-mRNA-1000bp-Flanks.bed \
-b C_virginica-3.0_CG-motif.bed \
> 2018-06-19-mRNA-100bp-Flanks-CGmotif.txt
`````

You can find the [flank-DML](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-11-DML-Analysis/2018-06-19-mRNA-100bp-Flanks-DML.txt) output on Github, but the CG motif one was too big. I'll upload that to OWL shortly. 

That's a wrap on the gonad methylation stuff until I'm back in town!

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
