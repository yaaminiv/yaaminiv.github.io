---
layout: post
title: SRM Analysis Part 8
---

## One script, infinite boxplots

Based on feedback from yesterday's practice talk, I've decided to focus on my shotgun proteomics data for PCSGA and hint at my SRM data. To do this, I needed to average my technical replicates and create plots to describe my area data. Because there's such great variation in my area data, Megan suggested that I make boxplots. Using this [R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-12-Protein-Area-Boxplots.R), I made boxplots. You can find them in my [SRM analysis folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902). I plotted normalized area as a proxy for protein abundance on the y-axis, with site (and sometimes eelgrass condition) on the x-axis. The plots were really good for demonstrating how much variation I had in my data! There was a lot of overlapping whiskers, which explains why my ANOSIM R values were close to zero.

I uploaded my talk to [my repository](https://github.com/RobertsLab/project-oyster-oa/blob/master/presentations/PCSGA2017_Venkataraman.pptx). PCSGA here I come!
