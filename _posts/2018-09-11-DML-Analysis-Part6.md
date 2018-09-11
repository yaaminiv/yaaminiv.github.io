---
layout: post
comments: true
title: DML Analysis Part 6
tags: virginica MBDSeq DML gene-enrichment DAVID
---

## DML-mRNA Gene Enrichment

My [revised `blastx`](https://yaaminiv.github.io/DML-Analysis-Part6/) finished running over the weekend! The output can be found [here](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-06-Transcript-Uniprot-blastx.txt). I emailed the revised blast output and original transcript file to Mike Riffle in Genome Sciences so he could modify the [gene enrichment tool](https://meta.yeastrc.org/compgo_yaamini_oyster/pages/goAnalysisForm.jsp).

### DML-mRNA Gene Enrichment

In my [R Markdown file](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-06-14-Gene-Enrichment-Analysis.Rmd), I merged the transcript-uniprot blast results with the DML-mRNA overlap file. I exported the results in [this file](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-11-mRNA-DML-Uniprot.csv).

I isolated the Uniprot accession codes so I could do a standard [DAVID](https://david.ncifcrf.gov/summary.jsp) gene enrichment. I unfolded columns in both the [transcript `blastx` results](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-11-Transcript-Uniprot-blastx-codeIsolated.txt) and [DML-mRNA overlaps](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-11-mRNA-DML-Uniprot-codeIsolated.csv) and saved them as new files. I also saved the Uniprot codes for the background (transcripts) and gene list (overlaps) in [this document](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-11-DML-mRNA-Background-GeneList.csv).

<img width="339" alt="1" src="https://user-images.githubusercontent.com/22335838/45392033-5ea26880-b5da-11e8-9965-8b9a4704ca87.png">

![2](https://user-images.githubusercontent.com/22335838/45392034-5ea26880-b5da-11e8-9c34-6a3eec037fe1.png)

<img width="389" alt="3" src="https://user-images.githubusercontent.com/22335838/45392035-5ea26880-b5da-11e8-8f61-79075a807ee7.png">

![4](https://user-images.githubusercontent.com/22335838/45392036-5ea26880-b5da-11e8-978a-a0ebf7a4b156.png)

![5](https://user-images.githubusercontent.com/22335838/45392037-5ea26880-b5da-11e8-93cb-36477a4a54aa.png)

![6](https://user-images.githubusercontent.com/22335838/45392038-5ea26880-b5da-11e8-866b-097b262205c0.png)

![7](https://user-images.githubusercontent.com/22335838/45392039-5ea26880-b5da-11e8-811b-a3c7f7527f5d.png)

I saved the DAVID output in [this folder](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-11-DAVID-Output). I downloaded information for [all functional annotations](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-11-DAVID-Output/2018-09-11-DML-mRNA-All-Functional-Annotations.txt), as well as individual lists for [biological processes](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-11-DAVID-Output/2018-09-11-DML-mRNA-BPGO.txt), [cellular components](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-11-DAVID-Output/2018-09-11-DML-mRNA-CCGO.txt), and [molecular functions](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-11-DAVID-Output/2018-09-11-DML-mRNA-MFGO.txt).

I wanted to use [ReVIGO]() to visualize significant GOterms, but ReVIGO's interactive graphic wasn't loading after I pasted a list of GOterms and p-values! I posted [this issue](https://github.com/RobertsLab/resources/issues/372). I have the option to download an R script to create the graphic, which would be useful, but also extremely tedious for image formatting. If I don't hear anything by tomorrow morning, I'll need to download the R scripts. My goal is to present the DML-mRNA enrichment results at PCSGA.

### DML-Exon Gene Enrichment

[Steven confirmed](https://github.com/RobertsLab/resources/issues/370) I need to do a gene enrichment for DML-exon, DML-intron, flanking regions, and all regions combined. I thought I could get started on these enrichments, even though I'll probably concentrate on the DML-mRNA enrichment for my PCSGA talk. However, the DML-exon file doesn't have the same transcript IDs the DML-mRNA and `blastx` results do. The DML-exon and DML-intron files only have chromosome IDs, positions, and strand information. I posted [another issue](https://github.com/RobertsLab/resources/issues/371) asking if it made sense to merge the DML-exon or DML-intron files with the original mRNA gff to obtain transcript IDs. If it doesn't, I'll need to figure out another way to do that gene enrichment.

### Going forward

While I wait for the verdict on my two open issues, I'm going to start drafting my PCSGA talk, focusing on the DML-mRNA enrichment. I saw lots of significant GOterms related to reproduction and growth, so that will be interesting to explain!

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
