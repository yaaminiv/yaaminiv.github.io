---
layout: post
comments: true
title: DML Analysis Part 46
tags: virginica MBDSeq DML methylation-islands
---

## Epigenetics reading group feedback!

Although the whole gang wasn't present Mac and Steven gave me some great feedback on the [*C. virginica* edits I started on](https://yaaminiv.github.io/DML-Analysis-Part45/)! Here are some of the highlights:

### Minor improvements

- Clearly defining what I consider a DML, since a locus doesn't have to be one base pair
- Rephrasing GO-MWU input creation methods
- Not using signed p-values for GO-MWU. Mac made a good point that understanding how up- or downregulated genes influence enrichment patterns makes more sense for transcription than seeing how hyper- and hypomethylated DML influence enrichment. We don't know what methylation is doing in a gene, and using signed p-values doesn't shed any light on that function. Additionally, hypermethylation could lead to increased or decreased gene activity. If that gene is responsible for repressing another gene function, assigning a positive or negative value based solely on methylation status doesn't take into account gene function.
- Removing DMG analysis since the actual function of methylation in these genes is unknown (no paired transcription data) and it didn't add anything to the paper.
- I moved the percent methylation across genome feature figure to Figure 1 instead of keeping it with Figure 2.
- Changed Figure 8 so it was colored by gene group, not biological process
- Mac suggested I convert the multiple barplots in Figure 5 (panels c-f) to stacked barplots. I created a new versions of this plot after doing some standard dataframe manipulation.

### Calculating "genome methylation"

In the paper, I state that 22% of the *C. virginica* genome was methylated. Mac and Steven were hesitant to make this claim since it wasn't clear how I got this number, and I couldn't find it in the methods anywhere. When I looked at the 3,181,904  methylated CpGs (> 50% methylated) versus the 14,458,703 total CpGs in the *C. virginca* gneome, I got 22%. This doesn't saying that 22% of the genome is methylated. Instead, 22% of CpGs were found to be methylated. I clarified this statement in the abstract, results, and discussion.

### Methylation islands

I removed the methylation island bar in Figure 2, since the methylated CpGs and methylation islands are correlated. Mac described methylation islands as a smoothing function, so it's not really interesting to see where they are located with respect to the other genome features beyond just knowing what's genic and intergenic. I also removed Figure 2b (individual feature locations in methylation islands). After calculating median and ranges for methylation island length and number of mCpGs, I tried creating a histogram of island lengths in [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2020-02-11-Characterizing-Methylation-Islands.Rmd). When I created a preliminary plot, it was clear that I would need a gapped axis! I [previously created a barplot with a gapped axis](https://genefish.wordpress.com/2019/07/16/creating-a-plot-with-a-gapped-axis/), so I thought I could recycle code. I kept getting the following error when I tried plotting my data:

```
Error in rect(xtics[littleones] - halfwidth, botgap, xtics[littleones] + : cannot mix zero-length and non-zero-length coordinates
```

In the meantime, I looked at the histogram bins and how many islands were in each bin. I wrote up a quick description for the results section.

### Things to improve in the discussion

- Look at list of genes with DML and see if any of them have been reported in ocean acidification and gene expression studies
- Add ocean acidification context to final two discussion paragraphs, along with caveat about experimental design confounding responses to ocean acidification with potential differences in gamete maturation

### Going forward

1. Address remaining comments about discussion text
2. Update manuscript text
2. Update response to reviewers
1. Consolidate any co-author feedback
3. Submit comment responses and reviesed manuscript
9. Post revised paper on bioRXiv
10. Update [paper repository](https://github.com/epigeneticstoocean/paper-gonad-meth) with new code and figures

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
