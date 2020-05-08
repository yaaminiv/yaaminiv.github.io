---
layout: post
comments: true
title: MethCompare Part 13
tags: MethCompare prop.test bedtools
---

## Revamping distribution plots and creating genome feature plots

After [testing my characterization pipeline]() with union bedgraphs, I needed to [make stacked barplots](https://github.com/hputnam/Meth_Compare/issues/58) with that data. I also need to explore CpG locations in genome features between methods [using various CpG categories](https://github.com/hputnam/Meth_Compare/issues/60). It's time to make plots on plots on plots.

### CpG distribution stacked barplots

I created [this R Markdown document](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd) to run my code. I contemplated putting it in my [previous script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.Rmd), but I thought keeping the workflow separate like I did for the Jupyter notebooks would be better. I imported the tab-delimited files with the file counts for each species and created summary tables (*M. capitata* [here](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union_5x-CpG-Type.txt) and *P. acuta* [here](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Pact/Pact_union_5x-CpG-Type.txt)). I then isolated the percent information and used it for pairwise chi-squared tests and to create stacked barplots.

**Table 1**. Pairwise chi-squared results for *M. capitata*, with chi-squared statistics along the top and p-values at the bottom (df = 2). The hypothesis was that each method would have the same distribution of methylated, sparsely methylated, and unmethylated CpGs.

| **Method** | **WGBS** | **RRBS** | **MBD-BS** |
|:----------:|:--------:|:--------:|:----------:|
|  **WGBS**  |     -    |  2.2883  |   7.8469   |
|  **RRBS**  |  0.3185  |     -    |   2.3195   |
| **MBD-BS** |  0.01977 |  0.3136  |      -     |

**Table 2**. Pairwise chi-squared results for *P. acuta*, with chi-squared statistics along the top and p-values at the bottom (df = 2). The hypothesis was that each method would have the same distribution of methylated, sparsely methylated, and unmethylated CpGs.

| **Method** | **WGBS** | **RRBS** | **MBD-BS** |
|:----------:|:--------:|:--------:|:----------:|
|  **WGBS**  |     -    |  0.39734 |   8.2335   |
|  **RRBS**  |  0.8198  |     -    |   10.349   |
| **MBD-BS** |  0.0163  | 0.005659 |      -     |

It's interesting that using the union data changed results for *M. capitata* but not *P. acuta*. The fact that MBD-BS was significantly different from WGBS is probably due to the higher percentage of methylated loci detected by MBD-BS. What these results tell me is that MBD-BS does pick up significantly more methylated loci for each species.

[Previously](https://yaaminiv.github.io/MethCompare-Part10/), I used code to make each stacked bar a different color. In the middle of the night (I'm not kidding) I had an epiphany for a way to define `i` so I could combine three separate code chunks into a loop. In the light of day that's what I did:

```
for (i in 1:ncol(t(McapCpGTypePercents))){
    xx <- t(McapCpGTypePercents)
    xx[,-i] <- NA
    colnames(xx)[-i] <- NA
    barplot(xx, col = c(plotColors[(3*i)-2], plotColors[(3*i)-1], plotColors[(3*i)]), add = TRUE, axes = FALSE, names.arg = c("", "", "")) 
} #Make each bar in the stacked barplot a different color gradient. Create a new dataframe with data for the plot and ignore all additional columns and column names. Plot the single bar with the correct color from plotColors. Do not add axes and add blank names for each column.
```

![Screen Shot 2020-05-07 at 1 24 40 PM](https://user-images.githubusercontent.com/22335838/81341298-2466c400-9066-11ea-8e98-cdc0fddb4e4d.png)

**Figure 1**. CpG distribution for *M. capitata* (pdf [here](https://github.com/hputnam/Meth_Compare/blob/master/Output/Mcap-CpG-Type.pdf))

![Screen Shot 2020-05-07 at 1 24 21 PM](https://user-images.githubusercontent.com/22335838/81341301-26308780-9066-11ea-9fa4-aba1e369a10b.png)

**Figure 2**. CpG distribution for *P. acuta* (pdf [here](https://github.com/hputnam/Meth_Compare/blob/master/Output/Pact-CpG-Type.pdf))

After making the stacked barplots, I conducted pairwise chi-squared tests for each method between species.

**Table 3**. Pairwise chi-squared results examining method sensitivity between species. The hypothesis was that each method would have the same distribution of methylated, sparsely methylated, and unmethylated CpGs for each species.

| **Method** | **Chi-squared** | **df** | **p-value** |
|:----------:|:---------------:|:------:|:-----------:|
|  **WGBS**  |      4.2001     |    2   |    0.1225   |
|  **RRBS**  |      10.841     |    2   |   0.004424  |
| **MBD-BS** |      3.9764     |    2   |    0.1369   |

Again, my results were different. Instead of WGBS, RRBS performed differently between species. This lines up well with what we saw in Hollie's circos plots. I updated and closed [this issue](https://github.com/hputnam/Meth_Compare/issues/58).

### Planning plots

Before I leapt into the world of stacked barplots for genome locations, I sketched out what datasets I needed to include in each figure:

**Union data**:

- All CpGs in the genome (background)
- WGBS
- RRBS
- MBD-BS

***P. acuta* DMC**:

- MBD vs. WGBS
- MBD vs. RRBS

**UpSet plot data**

- All CpGs in the genome (background)
- WGBS, RRBS, and MBD-BS
- WGBS and RRBS
- WGBS and MBD-BS
- RRBS and MBD-BS
- WGBS only
- RRBS only
- MBD-BS only
- None

I knew I could at least finish the first plot before our meeting, and the others until next week because I was bound to encounter questions about the methods and visualizations. Even if I didn't make plots for the other two data categories, I could at least run them through the pipeline and visualize later.

### All CpG genomic locations

Looking at these requirements, I knew my R script needed to include my [CG motif intersection data](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x.ipynb). Actually, at first I had a bit of a panic thinking I needed locations for all my 5x CpG data and started messing with my script, but when I saw that Shelly used all CpGs in the genome for ther UpSet plot, I figured I could use the data I had already and add a bar for 5x CpGs if needed. I wanted to obtain this programmatically instead of copying and pasting this information into my script. I went back and saved line counts in [this file](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-CGMotif-Overlaps-counts.txt) for *M. capitata* and [this file](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-CGMotif-Overlaps-counts.txt) for *P. acuta*.

### Union data genomic locations

In the same script, I imported file counts, reformatted my data, and created summary tables.

***M. capitata***:

- [Feature overlaps (counts)](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union-Genomic-Location-Counts.txt)
- [Feature overlaps (percentages)](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union-Genomic-Location-Percents.txt)

***P. acuta***:

- [Feature overlaps (counts)](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Pact/Pact_union-Genomic-Location-Counts.txt)
- [Feature overlaps (percentages)](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Pact/Pact_union-Genomic-Location-Percents.txt)

I worked through my pairwise chi-squared tests and things looked a bit funky. I then created two sets of stacked barplots for each species: 1) all three methods versus the location of the CpGs in the genome, and 2) the locations of the CpGs in the genome based on method and methylation status.

![Screen Shot 2020-05-07 at 10 12 05 PM](https://user-images.githubusercontent.com/22335838/81374934-3a4fa580-90b5-11ea-9c44-6acdd6ea8ba4.png)

**Figure 3**. CpG locations in various genome features (from bottom to top of each bar: CDS, intron, flanks, intergenic regions) across the *M. capitata* genome and between three methods.

![Screen Shot 2020-05-07 at 10 48 38 PM](https://user-images.githubusercontent.com/22335838/81374936-3d4a9600-90b5-11ea-937f-5f0c7bdb6a04.png)

**Figure 4**. *M. capitata* CpG locations in various genome features (from bottom to top of each bar: CDS, intron, flanks, intergenic regions) split by CpG methylation status (methylated, sparsely methylated, and unmethylated)

![Screen Shot 2020-05-07 at 10 49 40 PM](https://user-images.githubusercontent.com/22335838/81374945-3facf000-90b5-11ea-917e-b29189e437e2.png)

**Figure 5**. CpG locations in various genome features (from bottom to top of each bar: CDS, intron, flanks, intergenic regions) across the *P. acuta* genome and between three methods.

![Screen Shot 2020-05-07 at 10 48 56 PM](https://user-images.githubusercontent.com/22335838/81374950-4176b380-90b5-11ea-9d1a-a1c6b5f0ad09.png)

**Figure 6**. *P. acuta* CpG locations in various genome features (from bottom to top of each bar: CDS, intron, flanks, intergenic regions) split by CpG methylation status (methylated, sparsely methylated, and unmethylated)

Even these figures looked weird! The trends in these stacked barplots didn't match the [trends I observed when characterizing CpGs by individual sample](https://yaaminiv.github.io/MethCompare-Part9/). When I thought about it some more, I was starting to doubt the CpG distribution results...

### Reminding myself about statistics

In this moment I was reminded of two very important statistics rules (please imagine the appropriate LOTR meme when reading):

1. One cannot simply use proportions in a chi-squared tests.

I, a rusty stats human, was using pairwise chi-squared tests with *proportion* data instead of count data! Oy vey. I went back and revised my CpG distribtuion and genomic location pairwise tests to use count data, and ended up with everything being extremely significant (p-value < 2.2e-16). Since I have a lot more comparisons than my *C. virginica* gonad methylation paper, I figured that chi-squared tests may not be the most powerful method to use. I asked Shelly how she dealt with her geoduck data, and she used an ANOVA. I think after our meeting I will revise my statistics with an ANOVA, Tukey post-hoc tests, and correction for multiple comparisons.

2. One cannot simply average percentages.

For these union bedgraphs, I averaged percent methylation data for three samples when I really should have done the following calculation: (count methylated sample 1 + count methylated sample 2 + count methylated sample 3)/(total reads sample 1 + total reads sample 2 + total reads sample 3). The union bedgraph information doesn't take this into account, so I'll bring this up at our next meeting and see if Steven can programmatically generate the files I want before I go about doing that myself. So really, the plots I generated are null and void. I posted [this issue]() to capture my action items.

...at least my code works and I can easily plug and chug revised data?

### Identifying genomic locations

Seeing how I don't trust any sort of statistics right now, I figured the best thing I could do was generate summary tables for [method-associated DMC Mac created](https://github.com/hputnam/Meth_Compare/issues/61) and the [upset plot data from Shelly](https://github.com/hputnam/Meth_Compare/issues/61). I created [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Identifying-Genomic-Locations.ipynb) to run through all the steps. I generated quick line count tables that can be found in [this folder for *M. capitata*](https://github.com/hputnam/Meth_Compare/tree/master/analyses/Identifying-genomic-locations/Mcap) and [this folder for *P. acuta*](https://github.com/hputnam/Meth_Compare/tree/master/analyses/Identifying-genomic-locations/Pact).

Once I had my line counts, I created [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Identifying-Genome-Features-Summary-Plots.Rmd) to piece the data tables together. Since I didn't have to worry about methylation status, I could easily use `cbind` to mash together tables before calculating percentages.

***M. capitata***:

- [Feature overlap counts](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Mcap/Mcap-upset-data-Feature-Overlap-Counts.txt)
- [Feature overlap percents](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Mcap/Mcap-upset-data-Feature-Overlap-Percents.txt)

***P. acuta***:

- [DMC overlap counts](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-DMC-Feature-Overlap-counts.txt)
- [DMC overlap percents](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-DMC-Feature-Overlap-percents.txt)
- [Feature overlap counts](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-upset-data-Feature-Overlap-Counts.txt)
- [Feature overlap percents](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-upset-data-Feature-Overlap-Percents.txt)

I also created rough stacked barplots for the upset plot data just so we had something to discuss. I will work on the statistical element later. It's interesting to see how the bars are relatively consistent for *M. capitata* but have some variation for *P. acuta*.

![Screen Shot 2020-05-08 at 1 18 15 AM](https://user-images.githubusercontent.com/22335838/81386414-ce2b6c80-90c9-11ea-8d9d-7dc576937bf5.png)

**Figure 7**. *M. capitata* upset plot data in various genome features (from bottom to top of each bar: CDS, introns, flanks, intergenic regions) (pdf [here](https://github.com/hputnam/Meth_Compare/blob/master/Output/Mcap-Upset-Feature-Overlaps.pdf)

![Screen Shot 2020-05-08 at 1 26 18 AM](https://user-images.githubusercontent.com/22335838/81387067-ec459c80-90ca-11ea-9839-e4b02ba66434.png)

**Figure 8**. *P. acuta* upset plot data in various genome features (from bottom to top of each bar: CDS, introns, flanks, intergenic regions) (pdf [here](https://github.com/hputnam/Meth_Compare/blob/master/Output/Pact-Upset-Feature-Overlaps.pdf)

Welp, I guess I have a lot to bring up during our weekly meeting!

### Going forward

5. [Locate TE tracks](https://github.com/hputnam/Meth_Compare/issues/56)
6. [Characterize intersections between data and TE, and create summary tables](https://github.com/hputnam/Meth_Compare/issues/59)
1. Calculate averages properly for union data and redo CpG characterization
2. Perform ANOVA with post-hoc tests and correction for multiple comparisons
3. Redo stacked barplot for CpG distributions for union data
4. Redo stacked barplot for CpG genomic locations for union data
5. Perform statistical tests for upset plot data
2. Update methods
3. Update results
7. Figure out methylation island analysis

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
