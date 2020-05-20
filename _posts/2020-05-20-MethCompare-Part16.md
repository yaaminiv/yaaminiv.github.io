---
layout: post
comments: true
title: MethCompare Part 16
tags: MethCompare bedtools chisq.test
---

## Genome features and running the CpG characterization pipeline (again)

I was minding my business yesterday, updating our draft paper, when I noticed something fishy. That fishy thing lead me to another fishy thing, so I guess my Github-free week was a fantasy after all.

### Intergenic-CG motif overlaps

A few weeks ago, I copied and pasted the CG motif overlap summary tables from [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x.ipynb) into the paper. I decided to update this table with upstream and downstream flank information, as well as the revised intergenic overlap counts since I [modified this track recently](). When looking back at my Jupyter notebook, I noticed I never characterized CG motif overlaps with the revised intergenic track! In the same Jupyter notebook, I used `intersectBed` to get these counts and added them to the table. I then reran my [R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd). Since I revised the number of CpGs in intergenic regions, I needed these updated counts incorporated when I did my chi-squared tests. While going through this process, I realized that this information was already captured in the genomic location stacked barplot figure. I made sure all of my counts, figures, and tables were up-to-date with this new information, then I deleted the original CG motif overlap summary table that started me down this rabbit hole. 

### CpG characterizations

Alright, back to updating the results section. When looking for the number of lines for each method-specific union bedgraph, I noticed the second fishy thing. According to my union bedgraph averaging output, the number of CpG loci with WGBS data was less than the number of loci with MBD-BS data in *M. capitata*! I quickly checked [my 5x union data characterization pipeline](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union.ipynb). When selecting columns from my `pandas` output, I noticed that row numbers were saved as the first column. Past Yaamini took this into acount when putting in column numbers to subset for the chromosome, start, and end positions, but she didn't use the correct column numbers to also include the column with average percent methylation data for each species!! I ended up using sample 18 data as the WGBS average, the WGBS average as the RRBS average, and the RRBS average as the MBD-BS average. Yikes. 

I fixed this error for *M. capitata* and *P. acuta*, then reran everything in [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union.ipynb). I then took the output (line count information) and reran my [R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd) The output for both scripts can be found [here](https://github.com/hputnam/Meth_Compare/tree/master/analyses/Characterizing-CpG-Methylation-5x-Union). Immediately, I noticed changes to the plots I made ([found here](https://github.com/hputnam/Meth_Compare/tree/master/Output)).

<img width="776" alt="Screen Shot 2020-05-20 at 10 14 55 AM" src="https://user-images.githubusercontent.com/22335838/82496553-ce504280-9aa1-11ea-90b8-f71f1ab0d437.png">

**Figure 1**. CpG methylation status by sequencing method.

For both species, WGBS detects a higher percentage of strongly methylated CpGs. MBD-BS detects more strongly methylated Cpgs than RRBS, but in *P. acuta* neither enrichment method gets close to WGBS.

<img width="777" alt="Screen Shot 2020-05-20 at 10 15 09 AM" src="https://user-images.githubusercontent.com/22335838/82496571-d60fe700-9aa1-11ea-9753-507b0e24e869.png">

**Figure 2**. CpG genomic location by sequencing method.

Looking at genomic locations for *M. capitata*, MBD-BS and WGBS appear to perform similarly, while RRBS and the genome information are more consistent. MBD-BS may have slightly more CpGs in CDS. In *P. acuta* it's clear that all methods do some sort of enrichment or gene bias, but it's the strongest in MBD-BS.

<img width="980" alt="Screen Shot 2020-05-20 at 10 27 45 AM" src="https://user-images.githubusercontent.com/22335838/82496877-4454a980-9aa2-11ea-863f-3503d8712ea6.png">

<img width="978" alt="Screen Shot 2020-05-20 at 10 30 28 AM" src="https://user-images.githubusercontent.com/22335838/82496881-44ed4000-9aa2-11ea-932a-4282462afb38.png">

**Figures 3-4**. Proportion strongly methylated CpGs in genomic features for *M. capitata* and *P. acuta*. Scale for *P. acuta* is double that of *M. capitata*.

When considering the number of strongly methylated CpGs divided by the total number of CpGs in a given genomic feature, WGBS performs the best in *M. capitata*. In contraast, MBD-BS performs better than WGBS and RRBS in *P. acuta*. These differences may be due to poor MBD-BS coverage in *M. capitata* and the higher baseline methylation in *M. capitata*.

Nothing really jumped out at me when I skimmed the pairwise contingency test results, but the visualizations are different. I think they may be more consistent with our overall hypotheses, which is good. I updated the tables, figures, and text with this information.

### Going forward

1. [Locate TE tracks](https://github.com/hputnam/Meth_Compare/issues/56)
2. [Characterize intersections between data and TE, and create summary tables]
3. [Perform statistical comparisons for upset plot data](https://github.com/hputnam/Meth_Compare/issues/60)
(https://github.com/hputnam/Meth_Compare/issues/59)
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
