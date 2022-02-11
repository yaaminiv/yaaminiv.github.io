---
layout: post
comments: true
title: WGBS Analysis Part 37
tags: manchester BS-Snper
---

## Thinking about SNPs and DML

Steven and I have gone through the [BS-Snper code and results](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/07-BS-SNPer.ipynb) multiple times this week to determine how to deal with SNPs that overlapped with DML. We dove into the BS-Snper program information to understand how it could identify C->T SNPs over bisulfite-converted unmethylated CpGs. During bisulfite conversion, unmethylated cytosines are replaced with uracils, then converted to adenines during PCR amplification. The complementary strand for this locus would have a guanine, since the original base pair was a cytosine. If there's a C->T SNP, then the complementary strand would have an adenine.

Once we clarified the methods, we realized that the SNPs needed to be excluded from any DML characterization: they are false DML! I removed the SNPs from DML using `subtractBed` in [this script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.ipynb). I used [my revised DML track with no SNPs](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/10_DML-characterization/DML-pH-50-Cov5-All-NO-SNPs.bed) to characterize genomic locations. Once I updated the counts, I returned to [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.Rmd) to revise my heatmap and chromosome distribution figures. Then, I used the updated counts in [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.Rmd) to update my stacked barplot and gene count table.

![Screen Shot 2022-02-10 at 4 08 34 PM](https://user-images.githubusercontent.com/22335838/153632447-f00b106a-781e-4e89-acee-46b81103bce2.png)

**Figure 1**. Heatmap of unique DML

![Screen Shot 2022-02-11 at 9 56 04 AM](https://user-images.githubusercontent.com/22335838/153632448-5a43668a-4685-4288-b9fb-6d0a288f9736.png)

**Figure 2**. DML distribution over chromosomes

![Screen Shot 2022-02-11 at 10 27 22 AM](https://user-images.githubusercontent.com/22335838/153632450-ea49e6d5-a4e9-47fc-b8a0-4c6b21f61ea1.png)

**Figure 3**. Genomic location of DML

Nothing left to do now but put the finishing touches on!

### Going forward

1. Revise title
6. Format for submission
7. Submit preprint to bioRXiv
8. Submit paper for publication
9. Report `mc.cores` issue to [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html)
10. Perform randomization test
12. Determine if larval DNA/RNA should be extracted

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
