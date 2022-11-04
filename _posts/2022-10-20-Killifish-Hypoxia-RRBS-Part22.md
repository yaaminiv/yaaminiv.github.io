---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 22
tags: killifish RRBS poseidon BAT
---

## Global methylation differences by population

Previously, I showed that [methylation levels are lower in Scorton Creek killifish](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part19/). I wanted to revisit this analysis to make sure that 1) I used appropriate methods and 2) discern why there was less methylation in the Scorton Creek fish.

### Revising methods

I returned to [this Jupyter notebook](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-methylation-landscape-analysis.ipynb) for the analysis. One thing I noticed was that the number of CpGs with 10x coverage (5,413,381) was much greater than the number of CpGs in the [output file for all populations (14,896)](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/05-analysis/summarize/all_pop/all_pop_metilene_N_S.txt). I think this is because `BAT_summarize` only uses data for CpGs with coverage across all samples. I want to see if global methylation information differs when I allow for missing data.

To do this, I first created a union bedGraph:

```
#Create a union bedGraph
#Use N/A when there is no data for a CpG in a sample
#Define sample IDs
#Use sorted bedgraphs
#Cound the number of lines (CpGs) with data
!{bedtoolsDirectory}unionBedGraphs \
-header \
-filler N/A \
-names N_20-N4 N_5-N1 N_5-N2 N_20-N2 N_5-N3 N_20-N1 N_OC-N5 N_OC-N1 N_OC-N2 N_OC-N4 S_20-S1 S_20-S3 S_20-S4 S_5-S3 S_5-S4 S_5-S2 S_20-S2 S_5-S1 S_OC-S1 S_OC-S2 S_OC-S3 S_OC-S5 \
-i ../../04-calling/filtered/*sort.bedgraph \
> union_10x.bedgraph
```

Of 5,413,382 CpGs, 4,339,834 had non-zero methylation. I used `pandas` to create a column of average methylation values, then calculated the average percent methylation for every locus and overall average methylation. When I averaged methylation across all loci, I calculated 58.9% of CpGs were methylated. This is much higher than the 20.7% genome methylation when considering CpGs with data in all samples! I don't know how valid the 58.9% is, since many loci only had coverage in one sample. I'll stick with the 20.7% figure.

I then revised the percent methylation calculations for each population. Instead of using the [common dataset for all samples](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/05-analysis/summarize/all_pop/all_pop_metilene_N_S.txt), I used the dataset with common CpGs for either [New Bedford](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/05-analysis/summarize/20_5_N/N_metilene_NO_HY.txt) or [Scorton Creek](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/05-analysis/summarize/20_5_S/S_metilene_NO_HY.txt).

*Table 1. Revised methylation landscape information calculated using all common CpGs for each specific contrast.

| **Contrast** | **Methylated CpGs (%)** | **Unmethylated CpGs (%)** | **Average Methylation** |
|:------------:|:-----------------------:|:-------------------------:|:-----------------------:|
|  All samples |       7275 (48.8%)      |        7620 (51.2%)       |          20.7%          |
|    All NBH   |     116821 (66.3%)      |       59465 (33.7%)       |          21.2%          |
|    All SC    |      57338 (69.7%)      |       24967 (30.3%)       |          16.3%          |
|  Hypoxic NBH |      96857 (54.9%)      |       79429 (45.1%)       |          22.1%          |
| Normoxic NBH |      93895 (53.3%)      |       82391 (46.7%)       |          20.4%          |
|  Hypoxic SC  |      22694 (27.6%)      |       59611 (72.4%)       |          16.4%          |
|  Normoxic SC |      45661 (55.5%)      |       36644 (44.5%)       |          16.2%          |

### Methylation differences

Based on this table, I think it's interesting that there are less methylated CpGs in hypoxic Scorton Creek samples, but the average methylation remains constant. This makes me think that there are more highly methylated CpGs in hypoxic conditions. I wonder if intermediately methylated CpGs in normoxia are the ones that are more dynamic in hypoxic conditions.

I decided to plot methylation differences by population and treatment in [this R Markdown script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-methylation-landscape-analysis.Rmd). I made density plots showing methylation and methylation difference for each population. I used `scale_color_manual` to add a legend based on advice from [this post](https://www.statology.org/ggplot-manual-legend/), and I used `cowplot` to create an inset based on [this post](https://stackoverflow.com/questions/5219671/it-is-possible-to-create-inset-graphs).

<img width="1094" alt="Screen Shot 2022-10-21 at 3 49 58 PM" src="https://user-images.githubusercontent.com/22335838/197418224-cea146d3-c3ed-43ae-a8cb-fc8bfaf6b268.png">

**Figure 1**. Density plot of percent methylation for each population x treatment combination

<img width="1094" alt="Screen Shot 2022-10-21 at 4 23 29 PM" src="https://user-images.githubusercontent.com/22335838/197418226-4f095337-4203-41da-b6e9-79bf8bb02cc6.png">

**Figure 2**. Density plot of methylation difference between treatments for each population

I then thought it would be good to have a circos plot showing methylation across some chromosomes for each population and treatment. I picked two chromosomes with the most methylated positions for each population. I used [this link](https://www.royfrancis.com/beautiful-circos-plots-in-r/) to learn how to use `circlize` and reformat the plot. However, I ran into an issue where some points were being plotted outside of the sector for each chromosome. I looked at the [`circlize` manual](https://jokergoo.github.io/circlize_book/book/introduction.html), [old Github issue](https://github.com/jokergoo/circlize/issues/325), and [another issue](https://github.com/jokergoo/circlize/issues/101) but couldn't find a solution that worked for my data. So, I [posted my own](https://github.com/jokergoo/circlize/issues/345). I at least was able to start the process of creating a circos plot, and hopefully my issue gets addressed soon!

<img width="810" alt="Screen Shot 2022-10-23 at 3 55 17 PM" src="https://user-images.githubusercontent.com/22335838/197422787-4a772b6f-6bf2-42b2-8928-a5d75c9c015e.png">

**Figure 3**. Circos plot of DNA methylation at four different chromosomes.

### Going forward

1. Conduct pathway analysis for RNA-Seq data by population
5. Identify known SNP/DMR overlaps
1. Update methods and results
1. Figure out why there is such low methylation with the new genome
2. Continue BAT workflow with new genome
6. Create OSF repository for all intermediate files

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
