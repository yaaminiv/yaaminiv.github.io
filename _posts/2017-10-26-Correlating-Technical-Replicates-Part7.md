---
layout: post
title: Correlating Technical Replicates Part 7
---

## I finally have technical replication I kinda trust?!

What a relevation to have right as the sun made an appearance on a cloudy day! As I mentioned in my [previous post](), I decided to filter down my transitions using different coefficient of variation levels in an effort to improve my technical replication. [In this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-10/2017-10-26-NMDS-for-Technical-Replication-after-CV-10-Filtering.R), I only included transitions with a coefficient of variation less than or equal to 10. Look at the technical replication I got:

![NMDS-tech-rep1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-10/2017-10-26-NMDS-TechnicalReplication-Normalized-after-CV10-Filtering.jpeg)

**Figure 1**. Technical replication after using a coefficient of variation maximum of 10.

![NMDS-distance](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-10/2017-10-26-NMDS-TechnicalReplication-Ordination-Distances-after-CV10-Filtering.jpeg)

**Figure 2**. Ordination distances between technical replicates.

It's probably the best technical replication I've gotten with my data so far, and I kind of trust it! Obviously not thinking too hard about why certain transitions vary more than others and I didn't ensure that each protein included in this had at least two peptides (and each peptide had at least two transitions), but this is some exciting stuff after a long slug of data yuckiness.

Using this dataset, I averaged my technical replicates and remade an NMDS plot to look at clustering by site and eelgrass [in this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-10/2017-10-26-NMDS-ANOSIM-for-Cluster-Analysis-after-CV-10-Filtering.R). Once again, there were no obvious patterns. My ANOSIM was also not significant (but hey, maybe it's a statistically sound nonsignificant result and I am on board with that).

![NMDS-cluster](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-10/2017-10-26-NMDS-Norm-Analysis-Averaged-after-CV10-Filtering.jpeg)

**Figure 3**. NMDS for assessing similarities between sites and habitats.

I also weeded out technical replicates with the highest ordination distances from Figure 1 and remade my NMDS for technical replication.

![NMDS-tech-rep2](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-10/2017-10-26-NMDS-TechnicalReplication-Normalized-after-CV10-and-Distance-Filtering.jpeg)

**Figure 4**. Technical replication after using a coefficient of variation maximum of 10 and removing any technical replicates with ordination distances greater than 0.2.

I also reran my cluster analysis and ANOSIM and found no differences in significance.

![NMDS-cluster2](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-10/2017-10-26-NMDS-Norm-Analysis-Averaged-Adjusted-after-CV10-and-Distance-Filtering.jpeg)

**Figure 5**. NMDS for assessing similarities between sites and habitats after eliminating technical replicates with large ordination distances.

I used the same process but with a coefficient of variation maximum of 15 in [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-15/2017-10-26-NMDS-ANOSIM-for-Cluster-Analysis-after-CV-15-Filtering.R) and [this one](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-15/2017-10-26-NMDS-for-Technical-Replication-after-CV-15-Filtering.R), but having a threshold of 10 was much better.

My next step is to make boxplots for all of my transitions and to look at the transition list I used for the above plots to make sure that I'm not including any proteins without the appropriate number of peptides (and peptides without the appropriate number of transitions).
