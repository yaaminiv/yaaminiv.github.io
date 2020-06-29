---
layout: post
comments: true
title: MethCompare Part 22
tags: MethCompare glm
---

## Checking stacked barplots and post-hoc tests

Previously I [remade *M. capitata*](https://yaaminiv.github.io/MethCompare-Part21/) genome tracks and my genomic location stacked barplot. Something looked funky, but I tabled my investigation until later. Later is now, and my goal is to resolve that issue and finalize post-hoc tests to understand GLM output for methylation status and genomic location.

### *M. capitata* genomic location

The first thing I did was check my `bedtools` code in [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union.ipynb). That code looked good, so I figured there must have been an issue when I processed line count output in [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd). Before I began, I cleared by environment. I then reran the script to produce the [overlap counts](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap_union-Genomic-Location-Counts.txt) and [overlap percents](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap_union-Genomic-Location-Percents.txt) tables. Looking at the tables, WGBS detected 10% of CpGs that overlapped with CDS, while MBD-BS detected 14% of CpGs overlapping with CDS. This was not reflected in the stacked barplot I made! I ran the code fo my barplot and saw that it now reflected those percentages. My hunch is that clearing my environment did the trick. Since my union data and individual data scripts use the same dataframe names, I think the barplot I made last week called the correct dataframe name, but the content was for individual samples. I posted the revised barplot in [this issue](https://github.com/hputnam/Meth_Compare/issues/84) and moved on.

<img width="766" alt="Screen Shot 2020-06-29 at 1 50 21 PM" src="https://user-images.githubusercontent.com/22335838/86054709-7c3efd00-ba0f-11ea-89a7-3cf564c5aa1a.png">

**Figure 1**. *M. capitata* and *P. acuta* overlaps with various genome features ([*pdf here*](https://github.com/hputnam/Meth_Compare/blob/master/Output/Union-CpG-Features-Multipanel.pdf)).

### Post-hoc tests

I felt good about my PCoA, perMANOVAs, and models, but I still didn't have all the information I needed. For each CpG methylation status or genomic feature overlap, the models test WGBS vs. RRBS or WGBS vs. MBD-BS, since WGBS is the base condition. However, the models don't compare RRBS with MBD-BS. I wasn't sure what post-hoc test to run, so I asked Evan. He suggested I use `emmeans` to run post-hoc tests; I did so in [this script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.Rmd). The package runs estimated marginal means tests on each model and corrects p-values. I can then obtain log odds ratio test output and corrected p-values with a FDR. The command itself is very simple:

```
emmeans(McapCpGHighModel, pairwise ~ seqMethod, adjust = "FDR")
```

The output provides confidence intervals for proportion overlap for each method on the logit scale, as well as log odds ratio results. I only wanted the test results since we're not concerned with absolute proportions. To do this, I specified `$contrasts`. I also took the output with adjusted p-values and saved it as a dataframe:

```
McapCpGHighPostHoc <- data.frame(emmeans(McapCpGHighModel, pairwise ~ seqMethod, adjust = "FDR")$contrasts) #Run pairwise comparisons (estimated marginal means). Obtain log odd ratio results and not confidence intervals for individual methods in a dataframe format. Specify FDR instead of Tukey post-hoc test (default)
head(McapCpGHighPostHoc) #Look at log odd ratio results
```

I used `rbind` to combine statistical output for each model:

***M. capitata***:

- [Methylation status](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-CpG-Type-StatResults.txt)
- [Genomic location](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-CpG-Overlap-StatResults.txt)

***P. acuta***:

- [Methylation status](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-CpG-Type-StatResults.txt)
- [Genomic location](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-CpG-Overlap-StatResults.txt)

After I [knit my script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.md), I shared the output in [this issue](https://github.com/hputnam/Meth_Compare/issues/68) and on Slack. I still think Meth 8 may be an outlier, and I don't know if the PCoAs should be included with the stacked barplot as a multipanel plot, or if they should just go in the supplement.

I think that's it for my statistical analyses! Based on what we want for figures I can go back and make the PCoAs look pretty. Now I need to understand these results and update the paper.

### Going forward

3. Update methods
4. Update results
5. Update figures
4. Look into program for mCpG identification

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
  
