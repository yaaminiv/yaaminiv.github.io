---
layout: post
title: PECAN Update 3
---

## New day, same errors

[Jupyter notebook found here](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-03-07-Reconvert-mzML-Files.ipynb)

`pecanpie` ran for my one reconverted sample last night. When I checked the log files, I saw some sort of Percolator error this time. Not too sure what it is, so I'm having Sean look into it.

Either way, I tried opening the file in Skyline and ran into the same [error I got yesterday](https://yaaminiv.github.io/PECAN-Update-2/). 

![error-message-1](https://cloud.githubusercontent.com/assets/22335838/23712498/05e1653a-03d8-11e7-9df5-77f475cb2c63.png)

![error-message-2](https://cloud.githubusercontent.com/assets/22335838/23712502/08367384-03d8-11e7-8c70-2d1db20a234c.png)

**Update March 8 8:50 a.m.**

Using the code below, Sean was able to run one instance of `pecanpie` to try and figure out where the error was:

![screenshot from 2017-03-08 08-35-03](https://cloud.githubusercontent.com/assets/22335838/23713864/340504cc-03dc-11e7-8e7b-722a13b6621c.png)

![screenshot from 2017-03-08 08-29-32](https://cloud.githubusercontent.com/assets/22335838/23713865/3409b800-03dc-11e7-8053-e2241be9a8cc.png)

Alas, it's the same problem I had before: [no usable MS2 data](https://genefish.wordpress.com/2017/03/04/pecan-on-roadrunner-isnt-working-correctly/).

Sean and I opened the mzML file to figure out where the error was, but so far nothing looks suspicious to our untrained eyes. Until we sort this error out, I can't continue with my analysis.
