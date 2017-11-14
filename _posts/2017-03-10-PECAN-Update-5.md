---
layout: post
title: PECAN Update 5
---

## It works!

Kind of. [Using the digested proteome](https://yaaminiv.github.io/PECAN-Update-4/) solved my problems of not having [valid MS2 data](https://genefish.wordpress.com/2017/03/04/pecan-on-roadrunner-isnt-working-correctly/). However, Sean reported that we've hit a new snag. It seems like the memory request argument isn't a global memory request, but specifies memory for each instance of `pecanpie`. Basically, the first instance takes up all of the memory and the following instances time out. Sean's suggestion is to rerun `pecanpie` and tell it to only look at three isolation windows concurrently as it makes its way down the isolation scheme.

In the meantime, I'm going to draft poster layouts and modify the [DIA wiki](https://github.com/RobertsLab/resources/blob/master/protocols/DIA-data-Analyses.md) and our [DNR Paper](https://docs.google.com/document/d/1giP16iXWPE7oDSNI7fyLV3p_1jqsXuuxlH7cJQAwhLM/edit#heading=h.7vvlns7jaib). Seriously hoping things work before I go out of town Wednesday!

![screen shot 2017-03-10 at 1 32 19 pm](https://cloud.githubusercontent.com/assets/22335838/23813952/29ebc15e-0596-11e7-9f2d-8d40d9849dd5.png)

**Figure 1**. #MOOD
