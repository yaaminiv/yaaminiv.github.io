---
layout: post
comments: true
title: MethCompare Part 9
tags: MethCompare
---

## CpG distributions

I made my grand return to R today to apply handy dandy file manipulations and generate tables and figures in [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.Rmd).

### Programmatic tables

After [characterizing CpGs with 5x coverage](https://yaaminiv.github.io/MethCompare-Part8/), I quickly realized I needed an efficient and programmatic way to create summary tables with CpG counts and percentages. Manually pulling all of the data from `wc -l` output and calculating percents by hand wasn't cutting it anymore. The first thing I did was save `wc -l` output from [this Jupyter noteook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x.ipynb) as text files:

*M. capitata*:

- [Total 5x CpGs with data](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-5x-bedgraph-counts.txt)
- [Methylated CpGs](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-5x-Meth-counts.txt)
- [Sparsely methylated CpGs](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-5x-sparseMeth-counts.txt)
- [Unmethylated CpGs](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-5x-unMeth-counts.txt)
- [Gene overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-5x-mcGenes-counts.txt)
- [CDS overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-5x-mcCDS-counts.txt)
- [Intron overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-5x-mcIntrons-counts.txt)
- [Intergenic overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-5x-mcIntergenic-counts.txt)

*P. acuta*:

- [Total 5x CpGs with data](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-5x-bedgraph-counts.txt)
- [Methylated CpGs](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-5x-Meth-counts.txt)
- [Sparsely methylated CpGs](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-5x-sparseMeth-counts.txt)
- [Unmethylated CpGs](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-5x-unMeth-counts.txt)
- [Gene overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-5x-paGenes-counts.txt)
- [CDS overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-5x-paCDS-counts.txt)
- [Intron overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-5x-paIntron-counts.txt)
- [Intergenic overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-5x-paIntergenic-counts.txt)

I started by generating a summary table with total CpG with data and the number and percentage of methylated, sparsely methylated, and unmethylated CpGs. To do this, I imported each .txt file into R using spaces as delimiters, used `cbind` to mash all the columns together, removed the columns with file names, and calculated percentages. I saved the final tables after reorganizing the columns.

Creating the summary tables for the genome feature overlaps were a little more difficult. The `wc -l` output had information for a feature's overlap with total, methylated, sparsely methylated, and unmethylated loci in each sample. Using a loop, I extracted the information from the correct row and saved it in a new table. Similar to the CpG type tables, I calculated percentages and reorganized the columns before saving the final output.

I pasted the links to the summary tables in [this issue](https://github.com/hputnam/Meth_Compare/issues/38), but here they are again:

*M. capitata*:

- [CpG type](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-CpG-Type.txt)
- [Gene overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-Gene-Overlaps.txt)
- [CDS overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-CDS-Overlaps.txt)
- [Intron overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-Introns-Overlaps.txt)
- [Intergenic overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-Intergenic-Overlaps.txt)

*P. acuta*:

- [CpG type](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-CpG-Type.txt)
- [Gene overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-Gene-Overlaps.txt)
- [CDS overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-CDS-Overlaps.txt)
- [Intron overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-Introns-Overlaps.txt)
- [Intergenic overlaps](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-Intergenic-Overlaps.txt)

### Methylation distribution figure

The next thing I wanted to do was [revise the methylation frequency distribution figure](https://github.com/hputnam/Meth_Compare/issues/39) I created earlier. Hollie, Shelly, and Mac suggested that I rework the figure such that the x-axis bins CpG type (methylated, sparsely methylated, or unmethylated), and the y-axis is percent of total loci with data at 5x coverage. Using the summary tables I created in R, I removed all non-percent information columns. Then I used `barplot` to plot the CpG type for each sample. I created a multipanel plot that allows metehods comparisons vertically and sample comparisons horizontally. I added axis labels and sequencing information, then saved each figure as a pdf:

- [*M. capitata* CpG frequency distribution](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-CpG-Type.pdf)
- [*P. acuta* CpG frequency distribution](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-CpG-Type.pdf)

### Going forward

1. Create tracks for 2 kb upstream and downstream flanks
2. Characterize intersections between data and flanking regions and create summary tables
3. Create figures for CpG characterization in various genome features
5. Generate transposable element tracks for each species
4. Figure out how to meaningfully concatenate data for each method
3. Figure out methylation island analysis

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
