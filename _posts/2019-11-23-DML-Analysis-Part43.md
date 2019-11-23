---
layout: post
comments: true
title: DML Analysis Part 43
tags: virginica MBDSeq
---

## Revising manuscript figures

The new submission deadline is Dec. 31! I'd like to get a full draft in for comments before the end of the month, so I've been regaining motivation and slowly chipping away at manuscript edits. The task I focused on this week was revising figures and making some new ones. I chose this task because I wanted to finalize my methods and results sections before tackling my introduction and discussion, and because making figures is fun and it seemed like the best way to trick myself into being productive :satisfied:

### Recoloring plots!

First task: creating color schemes for my plots! I picked the Green-Blue color scheme from RColorBrewer, and checked that it was colorblind-friendly with `dichromat`.

```
plotColors <- rev(RColorBrewer::brewer.pal(5, "GnBu")) #Create a color palette for the barplots. Use 5 green-blue shades from RColorBrewer. Reverse the order so the darkest shade is used first.
barplot(t(t(proportionData)),
        col = dichromat(plotColors)) #Check that the plot colors will be easy to interpret for those with color blindess
```

<img width="467" alt="Screen Shot 2019-11-23 at 2 00 26 PM" src="https://user-images.githubusercontent.com/22335838/69485818-a08f8700-0df9-11ea-9a0d-add7ac06fe92.png">

**Figure 1**. Plot color scheme with `dichromat` adjustment.

I started by recoloring [previously-created figures](https://yaaminiv.github.io/DML-Analysis-Part42/): my methylation distribution (code [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2019-03-19-Characterizing-CpG-Methylation.Rmd)), stacked barplots (code [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Proportion-Test.Rmd)), and DML location figures (code [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-Gene-Enrichment-Analysis.Rmd)).

<img width="816" alt="Screen Shot 2019-11-15 at 4 57 01 PM" src="https://user-images.githubusercontent.com/22335838/69485758-d6803b80-0df8-11ea-8c6c-3eaceff6b444.png">

<img width="822" alt="Screen Shot 2019-11-15 at 4 56 12 PM" src="https://user-images.githubusercontent.com/22335838/69485761-dda74980-0df8-11ea-84b2-a2f15d5578d1.png">

<img width="824" alt="Screen Shot 2019-11-15 at 4 55 56 PM" src="https://user-images.githubusercontent.com/22335838/69485765-e13ad080-0df8-11ea-93e2-58e7d0c4a9ad.png">

<img width="816" alt="Screen Shot 2019-11-15 at 4 57 11 PM" src="https://user-images.githubusercontent.com/22335838/69485768-e5ff8480-0df8-11ea-8ce9-724234496793.png">

<img width="800" alt="Screen Shot 2019-11-15 at 4 57 20 PM" src="https://user-images.githubusercontent.com/22335838/69485770-ea2ba200-0df8-11ea-91b8-e427bcb6c2c0.png">

**Figures 2-6**. Recolored versions of CpG locus distribution, CpG location stacked barplot, DML location stacked barplot, multipanel with DML locations in chromosomes and genes and hyper-hypomethylation breakdown, and scaled DML distribution.

### GOslim reanalysis

The next task I tackled was reanalyzing my GOslim information by following methods from [my *C. gigas* GOslim analysis](https://yaaminiv.github.io/WGBS-Analysis-Part8/). In [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-08-28-blastx-to-GOslim.ipynb), I modified my GOslim annotations. I used GOslim annotations that included repeat GOslim terms for each gene, and included "uncharacterized biological processes" as a term in the analysis. Based on feedback from Steven and the rest of my committee, I first removed duplicate entries:

```
#Remove duplicate entries
#Count the number of unique IDs with GOSlim terms
!uniq Blastquery-GOslim-BP.sorted > Blastquery-GOslim-BP.sorted.unique
!uniq -f1 Blastquery-GOslim-BP.sorted.unique | wc -l
```

There were 126,112 unique *C. virginica* transcripts matched with GOslim terms. Once I removed duplicate entries, I removed any entry with "uncharacterized biological processes" using `grep --invert-match`:

```
#Remove all "other biological processes"
#Count the number of unique CGI IDs with defined GOSlim terms
!grep --invert-match "other biological processes" Blastquery-GOslim-BP.sorted.unique \
> Blastquery-GOslim-BP.sorted.unique.noOther
!uniq -f1 Blastquery-GOslim-BP.sorted.unique.noOther | wc -l
```

There were 105,959 unique *C. virginica* transcripts matched with defined GOslim terms. I moved onto [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-Gene-Enrichment-with-GO-MWU.Rmd) to annotate all tested genes and genes with DML with GOslim information. For all genes tested with GO-MWU, 27/2281 GO-MWU entries did not have accompanying GOslim entries, and 425/5283 GO-MWU entries did not have matching GOslim entries for genes with DML. After calculating the percentage of genes involved in each biological process, I created bar plots.

![Screen Shot 2019-11-19 at 11 04 50 AM](https://user-images.githubusercontent.com/22335838/69486392-1860b000-0e00-11ea-81a3-c972c99840f4.png)

![Screen Shot 2019-11-19 at 10 55 28 AM](https://user-images.githubusercontent.com/22335838/69486394-1ac30a00-0e00-11ea-8f8d-5fcf95720bab.png)

**Figure 7**. Percent of all genes tested involved in various biological processes.

**Figure 8**. Percent of genes with DML involved in various biological processes.

I figured it made sense to have a plot with biological process information for all tested genes and genes with DML side-by-side, so I created a multipanel plot. Instead of [using mirror plot code](https://genefish.wordpress.com/2019/10/09/creating-plots-mirrored-along-the-y-axis/) [like I did with the scaled DML distribution](https://yaaminiv.github.io/DML-Analysis-Part42/), I just created a multipanel with no margin between the two plots. I extended the leftside outer margin to make space for biological process labels. The code can be found [in this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-Gene-Enrichment-with-GO-MWU.Rmd).

<img width="688" alt="Screen Shot 2019-11-19 at 2 36 59 PM" src="https://user-images.githubusercontent.com/22335838/69486398-1e569100-0e00-11ea-9f63-c668f5949401.png">

**Figure 9**. Side-by-side comparison of biological processes represented by all genes tested using GO-MWU enrichment and genes with DML.

I think this plot clearly shows why we didn't see any significantly enriched gene ontology categories: the distributions are so similar! Having the background to compare to the GOslim categories of genes with DML drives the point home.

Julian suggested I give more weight to genes with multiple DML, or less weight to genes with DML that are part of multiple biological processes. The first suggestion is good, but becuase we don't know the functional role of genes with multiple DML for gene expression in this species, I don't know of giving more weight to genes with multiple DML makes sense. I think my new processes takes into account genes with multiple processes, since I'm looking at how many times that process is represented in my data.

### Principal component analyses with `methylKit`

A while back, Katie suggested I compare PCAs for all my percent methylation data and DML. Instead of writing code from scratch using my multivariate class notes, I decided to use functions built into `methylKit`. Turns out that this should have been easy but in reality was way more complicated than it should have been. Understanding the matrix the data was in and what the functions were doing took me at least a day and a half. The documentation in the `methylKit` manual is somewhat helpful, but I found I had more questions than it could answer! I dug through specific function pages on the [R Documentation page](rdocumentation.org) and even the `methylKit` publication. But I digress...

I returned to [my `methylKit` R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd) and examined the structure of the methylation information matrix. It's a `methylBase` object, which (spoiler alert) means it's a pain in the ass. The function `PCASamples` calculates a percent methylation matrix by dividing the number of cytosines by the sum of cytosines and thymines at that locus (C/C+T). Then, it uses `prcomp` to create the PCA. I struggled for a while to figure out how to customize the PCA that `methylKit` creates! FINALLY after hours of searching, I learned I could use the argument `obj.return = TRUE` to save the PCA object! I could then use `summary` to look at how much variance each PC explained.

```{r}
allDataPCA <- PCASamples(methylationInformationFilteredCov5Destrand, obj.return = TRUE) #Run a PCA analysis on percent methylation for all samples. methylKit uses prcomp to create the PCA matrix
summary(allDataPCA) #Look at summary statistics. The first PC explains 18.1% of variation, the second PC explains 11.7% of variation
```

According to Alan, my PCA explains more variance than his does. That's interesting, wbut I don't know what it means. I then used `ordiplot` to create a better looking PCA. I modeled my code after [code I used for the DNR paper](https://github.com/RobertsLab/paper-gigas-DNR-proteomics/blob/master/analyses/7-SRM/2018-12-01-Final-Ordination-Plot.Rmd).

```
fig.allDataPCA <- ordiplot(allDataPCA, choices = c(1, 2), type = "none", display = "sites", cex = 0.5, xlim = c(-400, 200), xlab = "", ylab = "", xaxt = "n", yaxt = "n") #Use ordiplot to create base biplot. Do not add any points
points(fig.allDataPCA, "sites", col = c(rep(plotColors[2], times = 5), rep(plotColors[4], times = 5)), pch = c(rep(16, times = 5), rep(17, times = 5)), cex = 3) #Add each sample. Darker samples are ambient, lighter samples are elevated pCO2

#Add multiple white boxes on top of the default black box to manually change the color
box(col = "white")
box(col = "white")
box(col = "white")
box(col = "white")
box(col = "white")
box(col = "white")
box(col = "white")
box(col = "white")
box(col = "white")
box(col = "white")

ordiellipse(allDataPCA, plotCustomization$treatment, show.groups = "1", col = plotColors[4]) #Add confidence ellipse around the samples in elevated pCO2
ordiellipse(allDataPCA, plotCustomization$treatment, show.groups = "0", col = plotColors[2]) #Add confidence ellipse around the samples in ambient pCO2

axis(side =  1, at = seq(-400, 200, 200), col = "grey80", cex.axis = 1.7) #Add x-axis
mtext(side = 1, text = "PC 1 (18.1%)", line = 3, cex = 1.5) #Add x-axis label

axis(side =  2, labels = TRUE, col = "grey80", cex.axis = 1.7) #Add y-axis
mtext(side = 2, text = "PC 2 (11.7%)", line = 3, cex = 1.5) #Add y-axis label

mtext(side = 3, line = -5, adj = c(-100,0), text = "    a. All CpG Loci", cex = 1.5)
```

<img width="776" alt="Screen Shot 2019-11-23 at 1 23 05 PM" src="https://user-images.githubusercontent.com/22335838/69486419-7097b200-0e00-11ea-8d17-58b99cd1ca6e.png">

**Figure 10**. PCA with percent methylation data for all CpG loci with 5x data across all samples.

I then decided to do the same thing with just DML information. This is when it got more complicated than it needed to be. The original methylation data is in a `methylBase` object, and `PCASamples` only accepts input in that format. I needed a way to subset DML without compromising the format. According to the `methylKit` manual, you can only subset rows using either `select` or `[]`. After two hours of banging my head against a wall trying to `join` or `merge` my list of DML with all methylation data, I left the office and immediately had an epiphany. I realized I could get the row numbers of DML in the original methylation data, save the row numbers as a vector, then use that vector to select the rows I want, and save it as a `methylBase` object.

```{r}
DMLPositions <- rep(0, times = length(diffMethStats50FilteredCov5Destrand$chr)) #Create an empty vector with 598 places to store row numbers
for (i in 1:length(DMLPositions)) {
  DMLPositions[i] <- which(getData(diffMethStats50FilteredCov5Destrand)$start[i] == getData(methylationInformationFilteredCov5Destrand)$start)
} #For each DML, save the row number where that DML is found in methylationInformationFilteredCov5Destrand
tail(DMLPositions) #Confirm vector was created
```

```{r}
DMLMatrix <- methylationInformationFilteredCov5Destrand[DMLPositions,] #Subset methylationInformationFilteredCov5Destrand to only include DML and save as a new methylBase object
sum((DMLMatrix$start) == (diffMethStats50FilteredCov5Destrand$start)) == length(diffMethStats50FilteredCov5Destrand$start) #Confirm that start columns are identical. If they are identical, the sum of all TRUE statements should equal the length of the original methylBase object
tail(DMLMatrix) #Confirm methylBase object creation
```

I then used similar code wtih `PCASamples` and `ordiplot` to create a refined figure.

<img width="812" alt="Screen Shot 2019-11-23 at 1 23 24 PM" src="https://user-images.githubusercontent.com/22335838/69486421-7392a280-0e00-11ea-851d-f9d41f20c00e.png">

It's interesting that the first PC explains almost half of the variance in the data! PC 1 is definitely related to experimental conditions.

**Figure 11**. PCA with percent methylation data for DML.

And you betcha I made a multipanel plot.

<img width="765" alt="Screen Shot 2019-11-23 at 1 24 03 PM" src="https://user-images.githubusercontent.com/22335838/69486424-768d9300-0e00-11ea-88fc-887eba818f69.png">

**Figure 12**. Multipanel plot with PCAs for all CpG loci with 5x data across all samples and DML only.

### Heatmaps for DML

The last thing I wanted to do was make a heatmap of DML! I used `percMethylation` within `methylKit` to obtain the percent methylation matrix `PCASamples` uses.

```{r}
percMethDML <- percMethylation(DMLMatrix, rowids = TRUE) #Get percent methylation for all samples at DML. Include row IDs (chr, start, end) information
```

I used `pheatmap` and `heatmap.2` (from `gplots`) to create heatmaps:

```{r}
pheatmap(percMethDML, color = rev(plotColors),
         cluster_rows = TRUE, clustering_distance_rows = "euclidean", treeheight_row = 70, show_rownames = FALSE,
         cluster_cols = TRUE, clustering_distance_cols = "euclidean", treeheight_col = 40, show_colnames = FALSE,
         annotation_col = data.frame(pCO2 = factor(rep(c("Ambient","Treatment"), each = 5))),
         annotation_colors = list(pCO2 = c(Ambient = "grey90", Treatment = "grey10")), 
         annotation_legend = FALSE, annotation_names_col = FALSE,
         legend = TRUE) #Create heatmap using pheatmap using percMethDML and plotColors color scheme. Cluster rows and columns using euclidean distances. Adjust the dendogram tree heights and do not show any row or column names. Create a dataframe with treatment information using annotation_col. Use annotation_colors to indicate colors for treatment ("grey90") and ambient ("grey10") samples. Do not include an annotation_legend or name for annotations (annotatino_names_col). Include a legend.
```

```{r}
par(oma = c(0, 1, 0, 0)) #Adjust outer margins
heatmap.2(percMethDML, col = plotColors, scale = "none", margins = c(1,1),
          trace = "none", tracecol = "black",
          labRow = FALSE, labCol = FALSE, 
          ColSideColors = c(rep("grey90", times = 5), rep("grey10", times = 5)),
          key = TRUE, keysize = 1.8, density.info = "density", key.title = "", key.xlab = "% Methylation", key.ylab = "",
          key.par = list(cex.lab = 2.0, cex.axis = 1.5)) #Create heatmap using heatmap.2 from gplots package using percMethDML data. Use plotColors but do not scale data, label rows, or label columns. Use ColSideColors to indicate colors for treatment and ambient samples. Add a legend using key, and adjust keysize. Have key display density data with density.info. Do not add a key title or y-axis label, and label x axis with key.xlab.
mtext("Density", cex = 1.6, las = 3, adj = 0.8, padj = -29) #Manually add y-axis label for key since heatmap.2 doesn't let you change font size
```

I've used `pheatmap` before, but I like how `heatmap.2` also generates a density plot as part of the legend. I decided to save the `heatmap.2` version for the paper.

![Screen Shot 2019-11-23 at 1 22 15 PM](https://user-images.githubusercontent.com/22335838/69487026-1ac70800-0e08-11ea-8049-62a5fe8921b8.png)

**Figure 13**. Heatmap of DML with percent methylation density plot in legend.

I tried making a multipanel plot with the heatmap and PCAs, but `heatmap.2` calls `plot.new` within its code so I couldn't create a multipanel plot! I could use `gridGraphics`, but I didn't want to figure out an entirely new package. If I wanted to create a multipanel in the future, I might just use InDesign or PowerPoint.

### Going forward

2. Update methods and results
4. Revise the discussion
5. Revise the introduction
7. Revise my abstract
8. Send to collaborators and committee members
8. Address any new edits and clean up the text
9. Update paper repository
10. Submit to the Special Issue
9. Post the paper on bioRXiv

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
