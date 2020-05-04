---
layout: post
comments: true
title: MethCompare Part 10
tags: MethCompare prop.test
---

## Revisiting CpG distributions

Based on feedback from Friday's meeting, I concatenated my CpG distribution data and created stacked barplots to investigate trends by methods in [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.Rmd).

### Chi-squared tests

The first thing I needed to do was combine data from the three replicates for each method. To do this, I created a new empty data frame. For each column (total lines, methylated CpGs, sparsely methylated CpGs, umethylated CpGs), I took the sum of the three samples for each method. Then I obtained percentages for each CpG type. This is the data table I used with `prop.test`.

Since my datatable had columns for percent CpG and rows for each method, I transposed the table to use in `prop.test`. I conducted pairwise tests within each species that compared the distribution of CpG type in one method with another.

**Table 1**. Chi-squared test results for *M. capitata*

|   **Comparison**   | **Chi-squared** | **df** | **p-value** |
|:------------------:|:---------------:|:------:|:-----------:|
|    WGBS vs. RRBS   |      3.7661     |    2   |    0.1521   |
| WGBS vs. MBD-BSSeq |      2.2279     |    2   |    0.3283   |
| RRBS vs. MBD-BSSeq |      9.7074     |    2   |    0.0078   |

**Table 2**. Chi-squared test results for *P. acuta*

|   **Comparison**   | **Chi-squared** | **df** | **p-value** |
|:------------------:|:---------------:|:------:|:-----------:|
|    WGBS vs. RRBS   |     0.012088    |    2   |    0.994    |
| WGBS vs. MBD-BSSeq |      10.811     |    2   |   0.004493  |
| RRBS vs. MBD-BSSeq |      11.352     |    2   |   0.003427  |

As I expected, the distribution of CpG type is significantly different between RRBS and MBD-BSSeq in both species. It's interesting to see that the WGBS vs. MBD-BSSeq comparison is ony different for *P. acuta*. This could be because *M. capitata* is a more heavily-methylated species, so by default WGBS does a good job picking up heavily methylated regions.

I also conducted similar pairwise tests to look at method sensitivity between species.

**Table 3**. Chi-squared test results for methods compared between *M. capitata* and *P. acuta*

| **Method Compared** | **Chi-squared** | **df** | **p-value** |
|:-------------------:|:---------------:|:------:|:-----------:|
|         WGBS        |      9.2868     |    2   |   0.009625  |
|         RRBS        |      3.6296     |    2   |    0.1629   |
|      MBD-BSSeq      |      1.5601     |    2   |    0.4584   |

It looks like WGBS is different between the two species, which would feed into the hypothesis that *M. capitata* is a more heavily methylated species to begin with.

### Creating stacked barplots

The next thing I wanted to do with this data is create stacked barplots similar to what I made for the *C. virginica* gonad methylation paper. I wanted each bar to be a different method, and then I wanted to "stack" the percent of CpGs that were methylated, sparsely methylated, or unmethylated. This was simple enough, but the tricky part was figuring out how to make each method a different color with a separate gradient to signify the different CpG types. I thought I could just create a color vector with nine entries instead of three, but `barplot` reused the first three entries for all the bars! After doing some digging, I found [this link](https://stackoverflow.com/questions/22781685/different-colors-for-each-bar-in-stacked-bar-graph-base-graphics). To do this, I needed to create a new matrix from the original data I'm plotting, then finding a way to ignore the other columns and column names.

```
xx <- t(McapCpGTypeTotalPercents) #New matrix for piecewise color addtions
xx[,-1] <- NA #Ignore all other columns
colnames(xx)[-1] <- NA #Ignore all other column names
barplot(xx, col = c(plotColors[1], plotColors[2], plotColors[3]), add=T, axes=F) #Add WGBS greens

xx <- t(McapCpGTypeTotalPercents) #New matrix for piecewise color addtions
xx[,-2] <- NA #Ignore all other columns
colnames(xx)[-2] <- NA #Ignore all other column names
barplot(xx, col = c(plotColors[4], plotColors[5], plotColors[6]), add=T, axes=F) #Add RRBS blues

xx <- t(McapCpGTypeTotalPercents) #New matrix for piecewise color addtions
xx[,-3] <- NA #Ignore all other columns
colnames(xx)[-3] <- NA #Ignore all other column names
barplot(xx, col = c(plotColors[7], plotColors[8], plotColors[9]), add=T, axes=F) #Add MBD-BSSeq reds
```

After I figured that out, I found a way to add line segments and text to indicate which pairwise comparisons were significant.

```
segments(x0 = 2, y0 = 106, x1 = 3, y1 = 106, lwd = 1.5) #Add line connecting RRBS and MBD-BSSeq
segments(x0 = 2, y0 = 106, x1 = 2, y1 = 103, lwd = 1.5) #Add left segment from connecting line to RRBS
segments(x0 = 3, y0 = 106, x1 = 3, y1 = 103, lwd = 1.5) #Add right segment from connecting line to MBD
text("p = 0.008", x = 2.5, y = 108, cex = 1.2) #Add p-value
```

I messed around with adding a greyscale legend, but I didn't like the way it looked. I'll deal with that later.

<img width="742" alt="Screen Shot 2020-05-04 at 4 46 33 PM" src="https://user-images.githubusercontent.com/22335838/81023985-ddd75680-8e26-11ea-8ce5-7717439c7533.png">

**Figure 1**. CpG distribution for *M. capitata* (pdf [here](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Mcap/Mcap-CpG-Type-Combined.pdf))

<img width="752" alt="Screen Shot 2020-05-04 at 4 46 42 PM" src="https://user-images.githubusercontent.com/22335838/81023984-dca62980-8e26-11ea-8540-a013ec11f081.png">

**Figure 2**. CpG distribution for *P. acuta* (pdf [here](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x/Pact/Pact-CpG-Type-Combined.pdf))

### Second thoughts

After all of this work, I started to have some doubts about how I concatenated the data. I summed all the categories between the three samples then calculated percentages. However, Hollie used averages to create Circos plots. I wasn't sure which method was better (and I went back-and-forth when I was doing it in the first place), so I commented in [this issue](https://github.com/hputnam/Meth_Compare/issues/58) only an hour after I closed it. Whoops.

### Going forward

1. Figure out if my concatenation method is valid
1. [Create tracks for 1 kb upstream and downstream flanks](https://github.com/hputnam/Meth_Compare/issues/57)
2. [Locate TE tracks](https://github.com/hputnam/Meth_Compare/issues/56)
2. [Characterize intersections between data, flanking regions, and TE and create summary tables](https://github.com/hputnam/Meth_Compare/issues/59)
3. [Create figures for CpG characterization in various genome features](https://github.com/hputnam/Meth_Compare/issues/60)
4. Update methods
5. Update results
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
