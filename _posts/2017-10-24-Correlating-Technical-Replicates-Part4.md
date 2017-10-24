---
layout: post
title: Correlating Technical Replicates Part 4
---

## Continuing the PRTC confusion

Emma suggested a while back that I use my PRTC data to make NMDS plots for my technical replicates. The goal was to see if my PRTC data was messier or better than my sample data. If anything, it's just confusing.

In [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-PRTC-NMDS/2017-10-24-PRTC-NMDS.R), I made NMDS plots with my PRTC data four different ways.

1. **Nonnormalized data, log transformation**: I couldn't make a plot without normalizing or transforming! I got an error that indicated too many weights were negative or zero. There is very clear separation of samples into four batches. I believe that samples that were run back to back are clustered together. I need to go back through my notes, but this may also be related to batches of PRTC I used.

![nonnorm log](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-PRTC-NMDS/2017-10-24-NMDS-TechnicalReplication-NonNormalized-LogTransformation.jpeg)

2. **Nonnormalized data, autotransformation**: Autotransformation produced a graph, but I'm not sure what it means. All of my samples are just clustered together in one large circle.

![nonnorm autotransform](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-PRTC-NMDS/2017-10-24-NMDS-TechnicalReplication-NonNormalized-Autotransformation.jpeg)

3. **Normalized data, log transformation**: This looked similar to the NMDS from my nonnormalized data, but the batch separation wasn't as clear. For this graph, I also looked at ordination distances between technical replicates. It did not look good at all.

![norm log](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-PRTC-NMDS/2017-10-24-NMDS-TechnicalReplication-Normalized-LogTransformation.jpeg)

![ordination distances](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-PRTC-NMDS/2017-10-24-NMDS-TechnicalReplication-Ordination-Distances-Normalized-LogTransformed.jpeg)

4. **Normalized data, autotransformation**: Again, not sure what it means.

![norm autotransform](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-PRTC-NMDS/2017-10-24-NMDS-TechnicalReplication-Normalized-Autotransformation.jpeg)

Once again I'm left with more questions than answers. I'll just wait to see what Emma thinks while investigating when I added PRTC.
