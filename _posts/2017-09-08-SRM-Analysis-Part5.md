---
layout: post
title: SRM Analysis Part 5
---

## Everything sucks.

[R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-06-NMDS-for-Technical-Replication.R)

I normalized my data using [TIC values](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-07-28-SRM-Samples-Sequence-File.csv), saved in my sequence file. After dividing all of my area values by TIC content, I remade my NMDS plot. Things did not look much better :disappointed:

![tech-rep-cluster](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-09-08-NMDS-TechnicalReplication-Normalized.jpeg)

During my meeting with Julian, he suggested I calculate the distances between ordinations for each technical replicate so I have a quantitative way of pulling out bad technical replicate clustering. For my bad NMDS, the distances didn't look great :cold_sweat:

![tech-rep-distances](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-09-08-NMDS-TechnicalReplication-Ordination-Distances.jpeg)

Emma said she's never seen such bad technical replication. Will I have yet another failed experiment? Will I be able to present anything at PCSGA? Who could never be sure :sob:
