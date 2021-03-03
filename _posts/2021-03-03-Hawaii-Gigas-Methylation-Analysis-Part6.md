---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 6
tags: hawaii gigas-ploidy bismark mox
---

## Aligning to the new *C. gigas* genome

There's a new *C. gigas* genome! Steven mentioned Mac may have been using the new genome, so I posted [this issue](https://github.com/RobertsLab/resources/issues/1119) to get a link to it. Although Mac never used the [Roslin genome](https://www.ncbi.nlm.nih.gov/assembly/GCF_902806645.1), Steven thought it would be a good idea to align to it. It has linkage groups, not just scaffolds, which is better for mapping and understanding my output. It has 10 linkage groups, which is similar to the 10 *C. virginica* chromosomes! My goal is to align the samples to this version of the genome instead, and move forward with these alignments in the main workflow. I'm contemplating using the [previously-generated alignments](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part5/) as well as the new ones just to compare.

### Adding the mitochondrial genome

The first thing I needed to do was add the [mitochondrial genome sequence](https://www.ncbi.nlm.nih.gov/nuccore/7212445?report=fasta) to the Roslin genome. I couldn't figure out how to download the FASTA (NCBI why is your website so confusing). I opened up `nano` and copied and pasted the sequence into a new file, then saved it as a .fa and hoped for the best. Steven had the Roslin genome in one of his directories, so I copied it over to my `/gscratch/scrubbed/yaamini/Haws/data/Cg-roslin` folder. I appended the mitochondrial sequence to the Roslin genome using `cat`, and saved it as a new .fa file. I looked at the file with `less` and the format looked like a FASTA file, so I decided to proceed with [`bismark`](https://github.com/FelixKrueger/Bismark).

### `bismark` script

I created [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/03-bismark-roslin.sh) to prepare a bisulfite genome and align samples. I referred to [this old Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-04-27-Gonad-Methylation-Bismark.ipynb) for the genome preparation arguments. I also used `rsync` to move the FASTQ files from `gannet` to `mox` so I could run the script. I submitted the script to `mox` and it's in the queue to run!

### Going forward

1. Try [BS-SNPer](https://github.com/hellbelly/BS-Snper) and [EpiDiverse](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
2. Obtain preliminary methylation assessment from `methylKit`
4. Test-run [DSS](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#34_DMLDMR_detection_from_general_experimental_design) and [ramwas](https://bioconductor.org/packages/release/bioc/html/ramwas.html)
5. Investigate comparison mechanisms for samples with different ploidy in oysters and other taxa
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)
6. Update methods
7. Update results

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
