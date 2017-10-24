---
layout: post
title: SRM Analysis Part 8
---

## One script, infinite boxplots

Based on feedback from my practice talk, I've decided to focus on my shotgun proteomics data for PCSGA and hint at my SRM data. To do this, I needed to average my technical replicates and create plots to describe my area data. Because there's such great variation in my area data, Megan suggested that I make boxplots. Using this [R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-12-Protein-Area-Boxplots.R), I made boxplots. You can find them in my [SRM analysis folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902). I plotted normalized area as a proxy for protein abundance on the y-axis, with site (and sometimes eelgrass condition) on the x-axis. The plots were really good for demonstrating how much variation I had in my data! There was a lot of overlapping whiskers, which explains why my ANOSIM R values were close to zero.

To characterize my shotgun proteomics data, I used `grep` to sort proteins based on their gene ontology functions. I saved each search as a separate text file, which can be found [here](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_Preliminary_Analyses_20170321/all-proteins-go-terms). I did this in terminal, so here are screenshots with the code I used:

<img width="1361" alt="untitled" src="https://user-images.githubusercontent.com/22335838/30897333-857caabc-a30a-11e7-9abd-ff22de81ac9c.png">

<img width="1221" alt="untitled-2" src="https://user-images.githubusercontent.com/22335838/30897332-856a55a6-a30a-11e7-91e5-36c4429f2f14.png">

<img width="1221" alt="untitled-3" src="https://user-images.githubusercontent.com/22335838/30897331-85697fc8-a30a-11e7-8850-7a1be5be3175.png">

**Figures 1-3**. Code used to sort shotgun proteomics data.

I uploaded my talk to [my repository](https://github.com/RobertsLab/project-oyster-oa/blob/master/presentations/PCSGA2017_Venkataraman.pptx). PCSGA here I come!