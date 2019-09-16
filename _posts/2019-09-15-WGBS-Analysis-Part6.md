---
layout: post
comments: true
title: WGBS Analysis Part 6
tags: manchester gigas-broodstock WGBS methylKit IGV bedtools
---

## Preliminary data for PCSGA

My PCSGA practice talk is tomorrow, and my real talk is on Tuesday! Today I'll finish processing my data and get tables and figures prepared for my talk.

### Revised `bismark` output in `methylKit`

EVERYTHING FINALLY PROCESSED CORRECTLY (as far as I can tell)! I moved all my output off Mox and onto `gannet` with this code:

```
rsync --archive --progress --verbose /gscratch/scrubbed/yaamini/analyses/Gigas-WGBS/2019-09-11-Gigas-Bismark/* yaamini@172.25.149.226:/Volumes/web/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-12-Bismark
```

I used the revised deduplicated and sorted files in `methylKit`. Something to remember: `methylKit` uses a Fisher's exact test instead of a logistic regression since there are no replicates.


**Table 1**. Number of DML identified by `methylKit` that are a 100% different between samples with 10x coverage.

| **Coverage** | **Date Created** | **Number of DML** |
|:------------:|:----------------:|:-----------------:|
|       5      |    2019-09-12    |        729        |
|       5      |    2019-09-15    |        732        |
|      10      |    2019-09-12    |        625        |
|      10      |    2019-09-15    |        628        |

Looks like the new deduplicated and sorted files gave me a few more DML. Although I looked at different coverage metrics, I'm still going to use the 10x coverage data with a 100% methylation difference. These settings are stringent to account for the fact that I only have two samples.

I tried the `diffMethPerChr` to obtain a breakdown of DML by chromosomes. The output doesn't look that great, but at least it's a visual that shows how sparse the DML are in the genome.

```{r}
jpeg(filename = "2019-09-15-Loci-Analysis/2019-09-15-DML-Distribution-by-Chromosome.jpeg", height = 1000, width = 1000) #Save file with designated name
diffMethPerChr(differentialMethylationStatsFilteredCov10Destrand, plot = TRUE, qvalue.cutoff = 0.001, meth.cutoff = 99) #Look at distribution of hyper- and hypomethylated DML per chromosome. Create an accompanying plot.
dev.off()
```

![percmethchr](https://raw.githubusercontent.com/RobertsLab/project-gigas-oa-meth/master/analyses/2019-09-12-MethylKit/2019-09-15-Loci-Analysis/2019-09-15-DML-Distribution-by-Chromosome.jpeg)

**Figure 1**. Percentage of DML by chromosome.

There are also still weird things going on with the DML background when using `destrand = TRUE`...I'll worry about this after PCSGA.

<img width="637" alt="Screen Shot 2019-09-15 at 9 34 10 PM" src="https://user-images.githubusercontent.com/22335838/64934573-8c7b4800-d800-11e9-9a1c-70484d9231de.png">

**Figure 2**. `destrand = TRUE` output.

### Verifying DML in IGV

I returned to [this notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-13-Generating-Coverage-Tracks.ipynb) and used my new `bismark` output to revise 5x and 10x coverage tracks. I moved the finished tracks to `gannet`, then imported these tracks into IGV along with the revised BEDfiles from `methylKit`.

For the most part, things lined up and looked good! Weird inconsistencies do exist...again, something to think about after PCSGA.

<img width="973" alt="Screen Shot 2019-09-15 at 9 33 33 PM" src="https://user-images.githubusercontent.com/22335838/64934574-8c7b4800-d800-11e9-9859-a66c901c23c4.png">

**Figure 3**. Tracks and CG motifs not lining up in IGV.

### Characterizing DML locations

In [this issue](https://github.com/RobertsLab/resources/issues/746), Steven provided [this link](http://onsnetwork.org/halfshell/2015/06/22/first-steps-at-an-aggregated-view-of-all-dna-methylation-data/) and [this link](https://eagle.fish.washington.edu/trilobite/index.php?dir=Crassostrea_gigas_v9_tracks%2F) to *C. gigas* feature tracks. I created [this notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb) to download feature tracks from `eagle` and characterize DML locations (P.S. by "created," I mean I copied my *C. virginica* notebook and deleted all the cells I no longer wanted. I feel so accomplished).

**Table 2**. Counts for each genome feature.

| **Genome Feature Track** | **Length** |
|:------------------------:|:----------:|
|         CG motifs        | 10,035,701 |
|           Exons          |   196,691  |
|          Introns         |   176,049  |
|           Genes          |   28,027   |
|         Promoters        |   28,023   |
|   Transposable Elements  |   119,786  |

What's interesting at first glance is that 1) there are not an equal number of genes and promoters and 2) there are 4 million less CG motifs in the *C. gigas* genome than the *C. virginica* genome.

**Table 3**. Number of overlaps with various genome feature tracks. For example, the 1.7% overlap between exons and CG motifs means that exons contained 1.7% of all CG motifs.

| **Genome Feature Track** | **Overlaps with CG motifs** |         **Overlaps with DML**          | **Overlaps with TE** |
|:------------------------:|:---------------------------:|:--------------------------------------:|:--------------------:|
|         CG motifs        |             N/A             |               624 (99.4%)              |     82,554 (0.8%)    |
|           Exons          |        172,056 (1.7%)       |                157 (25%)               |     2,597 (2.2%)     |
|          Introns         |        150,884 (1.5%)       |               285 (45.4%)              |    18,989 (15.9%)    |
|           Genes          |        28,015 (0.3%)        | 442 (70.4%); 398 unique genes (61.9%%) |     11,748 (9.8%)    |
|         Promoters        |        5,696 (0.06%)        |                24 (3.8%)               |     3,966 (3.3%)     |
|   Transposable Elements  |        82,554 (0.8%)        |                8 (1.3%)                |          N/A         |
|        No overlaps       |       5,118,363 (51%)       |               165 (26.3%)              |          N/A         |

It's interesting to see that most of the DML are actually in introns. This makes me thing that DML could be involved with alternative splicing. There are also slightly more DML in unannotated intergenic regions than exons. I put the overlap proportions in [this file](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/2019-09-15-Overlap-Proportions.csv) and compared overlap proportions in [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/2019-09-15-Proportion-Test.Rmd). I didn't know how to effectively put files together to generally characterize methylation trends, so I compared total CpGs with DML instead of methylated CpGs with DML. The distributions are different, and I generated a figure which can be found [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/2019-09-15-Total-CpGs-Versus-DML.pdf) in pdf form.

### Annotating output

Bottom line, this was unsuccessful. I posted [this issue](https://github.com/RobertsLab/resources/issues/751) to see if we had any annotated gene information since the GFF I'm using has CGI IDs and does not match up with what I see on GigaTON. Steven ran a `blast`, and I used that output to try and annotate my DML-gene overlaps in [this notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb). Something's fishy though because I can't do the annotation! I'm pretty sure I modified and joined all my files correctly until then, so there must be some reason why the IDs aren't matching up. At this point, I think it's time to make a talk instead of trying to work out analyses.

### Going forward

6. PUT TOGETHER A TALK
7. Present preliminary results at PCSGA
8. Figure out how to clean up results for WSN and ASLO
9. Potentially sequence more samples...?
10. Return to *C. virginica* data and finish. that. paper.

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
