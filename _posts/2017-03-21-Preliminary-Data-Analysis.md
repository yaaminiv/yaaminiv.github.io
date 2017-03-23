---
layout: post
title: Preliminary Data Analysis
---

## On that number-crunching grind

Using the [Skyline output](http://owl.fish.washington.edu/spartina/DNR_Skyline_Test_20170314/2017-03-16_Skyline_report_yaamini.csv) I generated from an [oyster seed .blib](https://github.com/sr320/course-fish546-2016/blob/master/data/oysterseed2.blib) and [my raw data](http://owl.fish.washington.edu/spartina/January_2017_DNR_Raw_Data/Oyster_raw_files/), it's time for me to understand what everything means. My goal is to get figures I can use for my NSA poster.

My methods can be found in [this notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/2017-03-21-Preliminary-Proteomic-Data-Analyses.ipynb), but here are the highlights.

#### Data exploration and Ratio Analysis

I used Average Peak Area statistics for my analysis. The data for approximately 6900 proteins looked like this:

![average peak area](https://camo.githubusercontent.com/bd5a3bb82cdfa58ba19f0deed13a62025fba7af3/68747470733a2f2f636c6f75642e67697468756275736572636f6e74656e742e636f6d2f6173736574732f32323333353833382f32343137383334382f63653438363531382d306536352d313165372d393261352d3939396635366138373237342e706e67)

I calculated ratios of eelgrass:bare areas across the five sites with the intention of using this information to pare down my dataset. However, I chose a different method of finding proteins of interest that suited our data more.

#### Preliminary Plots

Using the entire dataset, I created an NMDS plot and heatmap. Unsurprisingly, it wasn't very informative.

![prelimNMDS](https://camo.githubusercontent.com/1f0fd313b693971d14762093961e06594e9181b3/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f6d61737465722f616e616c797365732f444e525f5072656c696d696e6172795f416e616c797365735f32303137303332312f66696e616c4e4d44532e706e67)
![prelimheatmap](https://camo.githubusercontent.com/6c3b6692058655bb163542456307fec13e664a03/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f6d61737465722f616e616c797365732f444e525f5072656c696d696e6172795f416e616c797365735f32303137303332312f7072656c696d696e617279486561746d61702e706e67)

#### Preliminary Enrichment and REVIGO analysis

I used DAVID to find proteins overrepresented in all of my samples. One of the pathways that was overrepresented involved carbohydrate metabolism.

![carbs](https://camo.githubusercontent.com/13a25d06a006f369ff67d4cd0b0ff13e4f8f6c6d/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f6d61737465722f616e616c797365732f444e525f5072656c696d696e6172795f416e616c797365735f32303137303332312f44415649442d66696c65732f444e522d636172626f6e2d6d657461626f6c69736d2e706e67)

I produced a plot with 44 of my most significant biological processes GO terms in REVIGO.

![prelimREVIGO](https://camo.githubusercontent.com/1976c9aaefbc290a4e0bef7721c70524e6ee8582/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f6d61737465722f616e616c797365732f444e525f5072656c696d696e6172795f416e616c797365735f32303137303332312f44415649442d66696c65732f6f766572726570726573656e7465642d62696f6c6f676963616c2d70726f6365737365732e6a7067)

#### Data Subsetting

Steven helped me create a subset of my data with *C. gigas* [proteins involved in stress response](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_Preliminary_Analyses_20170321/short-list-analysis/stress-short-list.tab.txt). I merged [one of Rhonda's tables](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_Preliminary_Analyses_20170321/all-proteins-go-terms/Proteins-GO-terms.tabular) with [my Skyline output](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_Preliminary_Analyses_20170321/all-proteins-go-terms/Oyster-AverageAdjustedMergedArea.tabular) in Galaxy, and Steven selected proteins of interest.

Using this subset, I calculated the [coefficient of variation](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_Preliminary_Analyses_20170321/short-list-analysis/coefficient-of-variance-stress-short-list.txt) across all sites and eelgrass condition for each protein listed.

#### Subset Enrichment and REVIGO analysis

I returned to DAVID and REVIGO to create a plot for biological processes overrepresented in my data subset.

![subsetREVIGO](https://camo.githubusercontent.com/dd55e3d81d79cc81211a38caa77c82d80f77acfd/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f6d61737465722f616e616c797365732f444e525f5072656c696d696e6172795f416e616c797365735f32303137303332312f73686f72742d6c6973742d616e616c797369732f52455649474f2f702d76616c75652d62696f6c6f676963616c2d70726f6365737365732e706e67)

I also used my coefficient of variation data and GO IDs to create a similar REVIGO plot.

![cvREVIGO](https://camo.githubusercontent.com/387ae5251db3a295c9782a715bfe82d6bee13a30/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f6d61737465722f616e616c797365732f444e525f5072656c696d696e6172795f416e616c797365735f32303137303332312f73686f72742d6c6973742d616e616c797369732f52455649474f2f636f656666696369656e742d6f662d766172696174696f6e2d62696f6c6f676963616c2d70726f6365737365732e706e67)

#### Subset Plots

Finally, I remade NMDS and heatmap plots for the data subset. The NMDS was more useful with the data subset, showing evidence of clustering between sites.

![subsetNMDS](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_Preliminary_Analyses_20170321/short-list-analysis/R-analyses/subsetNMDS-revised.png)

The heatmap didn't really change.

![subsetheatmap](https://camo.githubusercontent.com/8bbf8ab276b09fef5e908de63f5aa18ad0f770c4/68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f6d61737465722f616e616c797365732f444e525f5072656c696d696e6172795f416e616c797365735f32303137303332312f73686f72742d6c6973742d616e616c797369732f522d616e616c797365732f737562736574486561746d61702e706e67)

Now I'll figure out which figures will tell a cohesive story for my poster!
