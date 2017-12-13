---
layout: post
title: Remaining Analyses Part 13
---

## Triple-nested for loops are not fun

As I mentioned in a [previous post](https://yaaminiv.github.io/Remaining-Analyses-Part12/), Steven suggested I create a table with information about the [peptide vs. biomarker regressions](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Biomarker-Scatterplots) I did. I was trying to do that using a three for loops nested within eachother.

<img width="772" alt="screen shot 2017-12-13 at 2 16 29 pm" src="https://user-images.githubusercontent.com/22335838/33966364-30d658a4-e014-11e7-8640-070c1f29af8e.png">

It did not go well.

When I was talking with Sam today, he suggested I break the nested for loops. Instead, he thought I should save all of the regression information into a new table, and then reformat that information into the table I wanted to save. That was the right idea! The code can be found in [this R Script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Biomarker-Scatterplots/2017-11-06-Peptide-Abundance-versus-Biomarker-Scatterplots.R), or below.

<img width="926" alt="screen shot 2017-12-13 at 2 46 36 pm" src="https://user-images.githubusercontent.com/22335838/33966450-8081cd20-e014-11e7-8d60-df867d597323.png">

<img width="924" alt="screen shot 2017-12-13 at 2 46 51 pm" src="https://user-images.githubusercontent.com/22335838/33966451-809c36a6-e014-11e7-85c3-01063f87a53c.png">

<img width="916" alt="screen shot 2017-12-13 at 2 46 59 pm" src="https://user-images.githubusercontent.com/22335838/33966452-80c630d2-e014-11e7-91e3-4f15047c2b34.png">

The table I produced can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-11-29-Biomarker-Scatterplots/2017-12-13-Peptide-Biomarker-Regression-Statistics.csv). 

My next steps are to (finally) quality control my data, remake pH, DO and salinity plots, make a table of important environmental variables, and do something with the growth data. Oh, and write I guess.

...can you tell I'm a bit tired of this yet?
