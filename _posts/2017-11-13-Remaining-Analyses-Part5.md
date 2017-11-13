---
layout: post
title: Remaining Analyses Part 5
---

## I've got the power

(Maybe).

I added a section to my [boxplot R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-06-Boxplots/2017-11-06-Protein-Area-Boxplots-after-Integration.R) for power analyses. I did two things: 1) calculate the  power of my experimental design based on theoretical effect sizes and 2) calculated the detectable effect sized based on theoretical power values. To do this, I used the [`pwr` R package](https://www.statmethods.net/stats/power.html).

**Calculating power**:

<img width="778" alt="screen shot 2017-11-13 at 12 20 53 pm" src="https://user-images.githubusercontent.com/22335838/32747287-24db6030-c86d-11e7-8cc9-bd0a2aabf132.png">

*Conclusion: I may not have enough power*

**Calculating effect size**:

<img width="797" alt="screen shot 2017-11-13 at 12 20 58 pm" src="https://user-images.githubusercontent.com/22335838/32747289-24fc0574-c86d-11e7-89ed-9c8fe15ab6ef.png">

*Conclusion: If I have power, I can detect quite a bit*
