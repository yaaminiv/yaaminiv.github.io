---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 5
tags: hawaii gigas-ploidy multiqc IGV
---

## Evaluating `bismark` output

I've been sitting on the Hawaii [`bismark`](https://github.com/FelixKrueger/Bismark) data for a while, so it's time I actually look at it! I referred to [the MethCompare repository](https://github.com/hputnam/Meth_Compare/) to remind myself what Shelly did to evaluate `bismark` output quality. I found [this script](https://github.com/hputnam/Meth_Compare/blob/master/code/00.05-FormatMultiQC.Rmd) and realized all I needed to do was 1) get [`multiqc`](https://multiqc.info/) information for `bismark` output, and 2) look at the data in IGV!

### `multiqc`

When I looked at [my script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/02-bismark.sh), I realized I never ran `multiqc` on the output! I navigated to where I stored the output on `gannet` and ran `multiqc *` to get the reports. From this command, I got a [HTML report](https://htmlpreview.github.io/?https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/Haws_02-bismark/multiqc_report.html) with relevant summary statistics, as well as individual text files with the raw data (found in [this folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_02-bismark/multiqc_data)). Looking at the general statistics, sample alignment averaged at 60%, which feels pretty good! About 12% of reads per sample aligned ambiguously, and 26% did not align at all. I was more interested in duplicated read alignment. Since I'm comparing diploid and triploid oysters, sequence duplication seems like it would be a bigger issue if there were duplicates removed from triploid oysters. Percent duplicated reads removed ranged from 10-12% for each treatment combination (ploidy x pH), so there doesn't seem to be a treatment effect (at least at first glance). I did notice that sample 7 had more reads than any other sample by a long shot.

![Screen Shot 2021-02-21 at 3 38 45 PM](https://user-images.githubusercontent.com/22335838/108642576-e868e800-745a-11eb-96ee-3f8325cfb4d2.png)

**Figure 1**. Reads aligned for reach sample

### IGV

My next step was looking at the coverage files in IGV! I created coverage files that merged CpG locations (`*merged_CpG_evidence.cov`), filtered the data to 5x coverage, then created bedgraphs (`*5x.bedgraph`). I wanted to look at these bedgraphs in IGV with respect to genome feature files.

The first thing I did was download the lateset version of IGV (v 2.9.2) from [this link](http://software.broadinstitute.org/software/igv/download). I found the links to the *C. gigas* genome and genome feature files in the [Genomic Resources wiki](https://robertslab.github.io/resources/Genomic-Resources/#crassostrea-gigas). I created [this IGV file](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Haws/sample-5x-bedgraphs.xml) using the `eagle` and `gannet` links, and colored samples based on treatment (light blue (1-6): 2N high pH; light red (7-12), 2N low pH; dark blue (13-18): 3N high pH; dark red (19-24): 3N low pH). From my quick browse, most methylation seems to be in the gene regions (makes sense because invertebrates), but I did find some instances of transposable element methylation.

![Screen Shot 2021-02-21 at 4 16 59 PM](https://user-images.githubusercontent.com/22335838/108643691-7c897e00-7460-11eb-862d-ed4974eb54ea.png)

![Screen Shot 2021-02-21 at 4 17 39 PM](https://user-images.githubusercontent.com/22335838/108643689-7abfba80-7460-11eb-8b3b-953b87bb152f.png)

**Figure 2-3**. IGV tracks

Now that I've looked at the data, my next steps are to load data into R to explore with [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html). While I won't be able to conduct a full analysis, I can obtain sample methylataion histograms and PCAs. To identify DML, I'm going to look into `DSS` and `ramwas`. Addtionally, Steven brought up that there's a revised *C. gigas* genome! While I work through DML identification with this dataset, I should also repeat `bismark` with the new genome.

### Going forward

1. Align samples to the revised *C. gigas* genome
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
