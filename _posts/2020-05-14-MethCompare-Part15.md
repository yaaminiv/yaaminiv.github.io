---
layout: post
comments: true
title: MethCompare Part 15
tags: MethCompare bedtools chisq.test
---

## Statistical comparisons for methods

Last time I [attempted statistical comparisons](https://yaaminiv.github.io/MethCompare-Part13/) I realized I needed to use chi-squared on count data, not proportions. I wanted a better way to make pairwise comparisons, so I posted [this issue](https://github.com/hputnam/Meth_Compare/issues/68). Shelly suggested a method she used in [this script](https://github.com/hputnam/Geoduck_Meth/blob/master/RAnalysis/Scripts/DMR_stats_and_anno.Rmd#L597), so I attempted to replicate that with my data over the past few days. I also clarified that I could use percent methylation data and that I averaged the union data correctly.

### Methylation status

I piloted everything with the methylation status information from union data in [this R Markdown document](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd). For each pairwise method comparison (WGBS vs. RRBS, WGBS vs. MBD-BS, RRBS vs. MBD-BS), I created contingency tables for each methylation status (strong methylation, moderate methylation, weak methylation). Each contingency table had one column for CpGs in that type, and another column for all other CpGs. The rows were the different sequencing methods. Once I created the tables, I ran a chi-squared test with `chisq.test`. I then extracted data from the test model using `broom::tidy` (aka MAGIC) and saved it as a dataframe! Finally, I added CpG type information to the column with test results and used `rbind` to merge results from one test with other test results:

```{r}
McapCpGTypeWGRR <- data.frame() #Create empty dataframe to store chi-squared results
for(i in 1:ncol(McapCpGTypeStatTest)) { #For each CpG type
  Method1CpG <- McapCpGTypeStatTest[1,i] #Variable for # CpG type for method 1
  Method2CpG <- McapCpGTypeStatTest[2,i] #Variable for # CpG type for method 2
  Method1NotCpG <- sum(McapCpGTypeStatTest[1,-i]) #Variable for # other CpG types for method 1
  Method2NotCpG <- sum(McapCpGTypeStatTest[2,-i]) #Variable for # other CpG types for method 2
  ct <- matrix(c(Method1CpG,Method2CpG,Method1NotCpG,Method2NotCpG), ncol = 2) #Create contingency table
  colnames(ct) <- c(as.character(colnames(McapCpGTypeStatTest[i])), paste0("Not", colnames(McapCpGTypeStatTest[i]))) #Assign column names: type, not type
  rownames(ct) <- c(as.character(row.names(McapCpGTypeStatTest)[1:2])) #Assign row names: method 1, method 2
  print(ct) #Confirm table is correct
  ctResults <- data.frame(broom::tidy(chisq.test(ct))) #Create dataframe storing chi-sq stats results. Use broom::tidy to extract results from test output
  ctResults$CpGType <- as.character(colnames(McapCpGTypeStatTest)[i]) #Add CpG type to results
  McapCpGTypeWGRR <- rbind(McapCpGTypeWGRR, ctResults) #Add test statistics to master table
}
```

I also used an FDR correction to correct for multiple comparisons and also included metadata on the pairwise comparison I did:

```{r}
McapCpGTypeWGRR$p.adj <- p.adjust(McapCpGTypeWGRR$p.value, method = "fdr") #Correct p-value using FDR
McapCpGTypeWGRR$comparison <- rep("WGBS vs. RRBS", times = 3) #Add methods compared
head(McapCpGTypeWGRR) #Confirm changes
```

I repeated this process for *P. acuta*, and used a similar loop to compare methods between species! I saved the output for the statistical tests for each species and for the interspecies comparisons:

- [*M. capitata*](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union-CpG-Type-StatResults.txt)
- [*P. acuta*](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Pact/Pact_union-CpG-Type-StatResults.txt)
- [Interspecies](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Interspecies_union-CpG-Type-StatResults.txt)

Looking at the p-values, everything was still significant. I'll keep this for now, but perhaps I need to use a stricter correction method.

The next thing I did is [revise my stacked barplots](https://github.com/hputnam/Meth_Compare/issues/66)! My initial goal was to add statistical difference information in the figures. Since every pairwise comparison was significant, I opted against this. I used "strong," "moderate," and "weak" language to describe methylation status instead of "methylated," "sparsely methylated," and "unmethylated." I also added legends to each plot and modified axis labels! Modifying the legend took a bit of finagling, since I needed to match x and y coordinates for `text` with `legend`. I also created a multipanel plot to use in the paper.

![Screen Shot 2020-05-13 at 5 02 09 PM](https://user-images.githubusercontent.com/22335838/81878028-a316a200-953b-11ea-948e-1f9f1d8ea2f8.png)

![Screen Shot 2020-05-13 at 5 02 25 PM](https://user-images.githubusercontent.com/22335838/81878037-a9a51980-953b-11ea-920c-c17f0e63eb4e.png)

![Screen Shot 2020-05-13 at 5 02 43 PM](https://user-images.githubusercontent.com/22335838/81878138-f688f000-953b-11ea-9572-003523fab883.png)

**Figures 1-3**. Methylation status stacked barplots for [*M. capitata](https://github.com/hputnam/Meth_Compare/blob/master/Output/Mcap_union-CpG-Type.pdf), [*P. acuta*](https://github.com/hputnam/Meth_Compare/blob/master/Output/Pact_union-CpG-Type.pdf), and a [multipanel plot](https://github.com/hputnam/Meth_Compare/blob/master/Output/Union-CpG-Type-Multipanel.pdf).

### Genomic location

Since everything worked, the next step was to repeat everything with CpG feature overlap data!

#### Union data

Before I could do pairwise comparisons, I needed to add counts for CG motif overlaps with upstream and downstream flanking regions. In [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x.ipynb), I used `intersectBed` to get overlaps, updated the notebook's summary tables, and saved the overlap counts:

- [*M. capitata*](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-CGMotif-Overlaps-counts.txt)
- [*P. acuta](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-CGMotif-Overlaps-counts.txt)

I then imported the upstream and downstream flank overlap information into [my R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd). I modified the genome feature overlap counts for both species so it included this information:

- [*M. capitata](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union-Genomic-Location-Counts.txt)
- [*P. acuta](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Pact/Pact_union-Genomic-Location-Counts.txt)

The pairwise comparisons are basically the same for feature overlaps, but I also added pairwise comparisons between CpG locations in the genome and sequencing methods. Again, everything was significant:

- [*M. capitata*](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union-Genomic-Location-StatResults.txt)
- [*P. acuta*](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Pact/Pact_union-Genomic-Location-StatResults.txt)
- [Interspecies](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Interspecies_union-Feature-StatResults.txt)

I updated the percentages to include upstream and downstream flank information, and removed the total flank information since it was redundant:

- [*M. capitata*](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union-Genomic-Location-Percents.txt)
- [*P. acuta*](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Pact/Pact_union-Genomic-Location-Percents.txt)

Finally, I revised my the stacked barplots! Similar to the CpG methylation status plots, I updated the axes, added in legends, and created a multipanel plot. I didn't conduct stastical comparisons for the plots that incorporated methylation status and genomic location, but I did add a legend.

![Screen Shot 2020-05-13 at 5 39 52 PM](https://user-images.githubusercontent.com/22335838/81879942-efb0ac00-9540-11ea-811f-07d341a48267.png)

![Screen Shot 2020-05-13 at 5 40 37 PM](https://user-images.githubusercontent.com/22335838/81879949-f17a6f80-9540-11ea-9fae-7421ebf10e6c.png)

![Screen Shot 2020-05-13 at 5 40 56 PM](https://user-images.githubusercontent.com/22335838/81879952-f3dcc980-9540-11ea-93cd-c1a571d2836d.png)

**Figures 4-6**. Genomic location stacked barplots for [*M. capitata](https://github.com/hputnam/Meth_Compare/blob/master/Output/Mcap_union-CpG-Features.pdf), [*P. acuta*](https://github.com/hputnam/Meth_Compare/blob/master/Output/Pact_union-CpG-Features.pdf), and a [multipanel plot](https://github.com/hputnam/Meth_Compare/blob/master/Output/Union-CpG-Features-Multipanel.pdf).

![Screen Shot 2020-05-13 at 5 43 32 PM](https://user-images.githubusercontent.com/22335838/81880082-533ad980-9541-11ea-8857-1058ba1bbbba.png)

![Screen Shot 2020-05-13 at 5 43 48 PM](https://user-images.githubusercontent.com/22335838/81880083-53d37000-9541-11ea-8737-e9af69619fbc.png)

**Figures 7-8**. Genomic location by methylation status for [*M. capitata*](https://github.com/hputnam/Meth_Compare/blob/master/Output/Mcap_union-CpG-Features-MethStatus.pdf) and [*P. acuta](https://github.com/hputnam/Meth_Compare/blob/master/Output/Pact_union-CpG-Features-MethStatus.pdf).

I added all of the statistical output and bargraph links to [this issue](https://github.com/hputnam/Meth_Compare/issues/60) so I could keep track of my progress.

#### DMC

I didn't need to revise barplots for the method-associated DMC, but I did want to run statistical comparisons. Before running any statistical tests, I updated the [overlap counts](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-DMC-Feature-Overlap-counts.txt) and [percents](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-DMC-Feature-Overlap-percents.txt) with up- and downstream flank information, and added information for the background — locations of all CpGs in the *P. acuta* genome. Next, I did three pairwise tests: all CpGs vs. RM DMC, all CpGs vs. WM DMC, and RM DMC vs. WM DMC. I performed the tests in [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Identifying-Genome-Features-Summary-Plots.Rmd) and saved the test ouptut [here](https://github.com/hputnam/Meth_Compare/blob/be25687cdb0d1e47472680cd3da27430b5a589cd/analyses/Identifying-genomic-locations/Pact/Pact-DMC-Genomic-Location-StatResults.txt). Finally I [updated this issue](https://github.com/hputnam/Meth_Compare/issues/63) so Mac would know I ran the tests!

#### Upset plot data

In [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Identifying-Genome-Features-Summary-Plots.Rmd) I also reformatted upset plot data and stacked barplots. For each species, I added information on all genomic CpGs and up- and downstream flank overlap information to the count data. I removed the gene and flank data from the percent overlap information since it would be redudant with other features.

***M. capitata***:

- [Overlap count information](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Mcap/Mcap-upset-data-Feature-Overlap-Counts.txt)
- [Overlap percent information](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Mcap/Mcap-upset-data-Feature-Overlap-Percents.txt)

***P. acuta***:

- [Overlap count information](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-upset-data-Feature-Overlap-Counts.txt)
- [Overlap percent information](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-upset-data-Feature-Overlap-Percents.txt)

![Screen Shot 2020-05-14 at 12 04 14 PM](https://user-images.githubusercontent.com/22335838/81975087-60a4a200-95db-11ea-9c56-251e83611f4c.png)

![Screen Shot 2020-05-14 at 12 04 41 PM](https://user-images.githubusercontent.com/22335838/81975103-64d0bf80-95db-11ea-90b1-c2a1accb5a6f.png)

**Figures 9-10**. Genomic location of upset plot data for [*M. capitata*](https://github.com/hputnam/Meth_Compare/blob/master/Output/Mcap-Upset-Feature-Overlaps.pdf) and [*P. acuta*](https://github.com/hputnam/Meth_Compare/blob/master/Output/Pact-Upset-Feature-Overlaps.pdf).

When it came to the statistical comparisons, I wasn't sure what pairwise tests I needed to do. Including genome information, there were nine categories of CpGs. If I did pairwise tests between all of them, that would be a lot. I commented in [this issue](https://github.com/hputnam/Meth_Compare/issues/60) and asked Shelly which comparisons she was most interested in.

### Replicating Liew et al. Figure 1B

Since this is a coral project, Hollie suggested we [replicate Liew et al. (2018) Fig. 1B](https://github.com/hputnam/Meth_Compare/issues/67) so we can easily compare results between our species and *Stylaphora pistillata*. The figure is a barplot that has genomic feature on the y-axis and a percent of methylated CpGs vs. all CpGs on the x-axis.

In [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd) I created divided the number of strongly methylated CpGs in a feature by all of the genomic CpGs in that feature. I created two versions of the horizontal grouped barplot: one with flanking information, and one without. Liew et al. (2018) did not include flanking information, but I did since the data was easily accessible. I figured we could decide what kind of question we wanted to answer with the figure and pick a version accordingly.

![Screen Shot 2020-05-14 at 2 07 27 PM](https://user-images.githubusercontent.com/22335838/81986738-41af0b80-95ed-11ea-94cd-42ceb85fc149.png)

![Screen Shot 2020-05-14 at 2 08 30 PM](https://user-images.githubusercontent.com/22335838/81986741-44116580-95ed-11ea-9b7d-d5f022d81b35.png)

**Figures 10-11**. Ratio of strongly methylated to all CpGs in a genomic feature for [*M. capitata*](https://github.com/hputnam/Meth_Compare/blob/master/Output/Mcap_union-StrongMeth-CpG-Features.pdf) and [*P. acuta](https://github.com/hputnam/Meth_Compare/blob/master/Output/Pact_union-StrongMeth-CpG-Features.pdf).

![Screen Shot 2020-05-14 at 2 07 47 PM](https://user-images.githubusercontent.com/22335838/81986792-58556280-95ed-11ea-8257-b22d99ef6d6d.png)

![Screen Shot 2020-05-14 at 2 08 45 PM](https://user-images.githubusercontent.com/22335838/81986800-5a1f2600-95ed-11ea-909c-833014088ffe.png)

**Figures 10-11**. Ratio of strongly methylated to all CpGs in genic regions for [*M. capitata*](https://github.com/hputnam/Meth_Compare/blob/master/Output/Mcap_union-StrongMeth-CpG-Features-NoFlanks.pdf) and [*P. acuta](https://github.com/hputnam/Meth_Compare/blob/master/Output/Pact_union-StrongMeth-CpG-Features-NoFlanks.pdf).

Looking at these figures, it's clear that RRBS detects the most strongly methylated CpGs in *M. capitata* and WGBS detects the most in *P. acuta*. Low coverage of MBD-BS could explain why it does poorly in this figure but looks like it is enriching for strongly methylated CpGs in the stacked barplots.

### Going forward

1. Update methods
2. Update results
3. [Locate TE tracks](https://github.com/hputnam/Meth_Compare/issues/56)
4. [Characterize intersections between data and TE, and create summary tables]
5. [Perform statistical comparisons for upset plot data](https://github.com/hputnam/Meth_Compare/issues/60)
(https://github.com/hputnam/Meth_Compare/issues/59)
6. Look into program for mCpG identification

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
