---
layout: post
title: Selecting SRM Targets Part 3
---

## MSstats works!

After a few weeks, I finally figured out how to use MSstats to identify differentially expressed proteins and peptides. This [Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/2017-05-12-Selecting-SRM-Targets-with-MSstats.ipynb) has my workflow, but here are the highlights:

### Step 1: Export data from Skyline

At first, I was exporting data from Skyline to meet the specifications for `dataProcess` in the [MSstats manual](https://bioconductor.org/packages/release/bioc/vignettes/MSstats/inst/doc/MSstats-manual.pdf). By following that route, I ran into an error saying that I had duplicate proteins and needed to specify a "Fraction" value. Better yet, there was no information about peptide fractions anywhere relating to MSstats! After a bunch of Googling, I found this handy function [`SkylinetoMSstatsFormat`](https://rdrr.io/bioc/MSstats/src/R/SkylinetoMSstatsFormat.R). I exported three separate Skyline reports to use this function, but with different BioReplicate and Condition information: 1) Bare vs. Eelgrass, 2) Sites only and 3) Sites and Eelgrass condition.

![unnamed-2](https://user-images.githubusercontent.com/22335838/27842639-90c1709c-60c0-11e7-8062-167e796ed7c7.png)

**Figure 1**. Columns needed for `SkylinetoMSstatsFormat`.

![unnamed-3](https://user-images.githubusercontent.com/22335838/27842651-aa492a46-60c0-11e7-9f16-c7e0098fd022.png)

**Figure 2**. I saved the columns needed for `SkylinetoMSstatsFormat` under Export >> Report >> "SkylinetoMSstats".

![unnamed-4](https://user-images.githubusercontent.com/22335838/27842702-2bf7f37e-60c1-11e7-8585-77d55538a0cc.png)

**Figure 3**. Example of Condition and BioReplicate settings, this one for both Site and Eelgrass condition pairwise comparisons.

### Step 2: Use MSstats

The commands I used to conduct pairwise comparisons: `SkylinetoMSstatsFormat` >> `dataProcess` >> `groupComparison`

I then used `groupComparisonPlots` to graphically represent my data.

I found no differentially expressed proteins when doing pairwise comparisons between Bare and Eelgrass sites, but I did find differentially expressed proteins when looking at just sites, and when accounting for both site and eelgrass conditions!

Next step: examine annotations to see what kinds of proteins are differentially expressed.

