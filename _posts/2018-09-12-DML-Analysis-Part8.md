---
layout: post
comments: true
title: DML Analysis Part 8
tags: virginica MBDSeq DML gene-enrichment ReVIGO
---

## DML-mRNA Gene Enrichment Results

Yesterday, I went through DML-mRNA overlap gene enrichment in DAVID (see my [lab notebook post](https://yaaminiv.github.io/DML-Analysis-Part7/)), but I was unable to generate plots in ReVIGO. Turns out my [issue with ReVIGO](https://github.com/RobertsLab/resources/issues/372) was related to Adobe Flash. Once I updated my flash player, I was able to create the interactive graphics. I added the graphics and explanations below to my [R Markdown file](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-06-14-Gene-Enrichment-Analysis.Rmd) as well. I probably need to learn how to use the R script to generate plots for reproducibility purposes, but that can wait.

I pasted significant GOterms and p-values into [ReViGO](http://revigo.irb.hr) to generate the following figures. For each category, I created a plot with medium term similarity, and either sizes based on logSize or log10p-value.

*Biological processes*:

Processes include cytoskeleton and microtubule movement, cell cycle, and protein activity. The most interesting results were those related to DNA and reproduction. DNA replication, mRNA processing, positive regulation of transcription from RNA polymerase II promoters, and response to DNA damage were all enriched. Additionally, enrichment positive regulation of histone H3-K9 acetylation, methylation-dependent chromatin silencing, DNA methylation involved in gamete generation, and fertilization point to mechanisms for epigenetic inheritance! This is going to be the central finding of my PCSGA talk.

![BP-size](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-12-ReVIGO-Plots/2018-09-12-DML-mRNA-BP-LogSize-Medium.png)

![BP-pvalue](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-12-ReVIGO-Plots/2018-09-12-DML-mRNA-BP-LogPValue-Medium.png)

*Cellular components*:

Cellular components involved in enriched processes included protein complexes, transcription factor complexes, chromosomes, centromeres, microtubules, and cilia.

![CC-size](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-12-ReVIGO-Plots/2018-09-12-DML-mRNA-CC-LogSize-Medium.png)

![CC-pvalue](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-12-ReVIGO-Plots/2018-09-12-DML-mRNA-CC-LogPValue-Medium.png)

*Molecular function*:

Molecular functions of enriched processes include GTPase and ATPase activity, helicase activity, microtuble motor activity, protein kinase binding and activity, transcription regulatory region DNA binding, general DNA binding, and ion binding.

![MF-size](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-12-ReVIGO-Plots/2018-09-12-DML-mRNA-MF-LogSize-Medium.png)

![MF-pvalue](https://raw.githubusercontent.com/RobertsLab/project-virginica-oa/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-09-12-ReVIGO-Plots/2018-09-12-DML-mRNA-MF-LogPValue-Medium.png)

It'll be interesting to see how the DAVID gene enrichment compares to the Mike Riffle's [gene enrichment tool](https://meta.yeastrc.org/compgo_yaamini_oyster/pages/goAnalysisForm.jsp), or how gene enrichment in the other components will complicate the story. For now, I have enough to work with for PCSGA.

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
