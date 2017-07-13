---
layout: post
title: Selecting SRM Targets Part 6
---

## Those peaks are running interference

[Emma's feedback](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/2017-07-07-SRM-Target-Identification-in-Skyline.ipynb) on my [preliminary protein targets](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_TransitionSelection_20170707/2017-07-07-Preliminary-Transitions/2017-07-07-Preliminary-Target-Transitions-Evalues.csv) showed me that I did not peak the best peaks possible for some proteins.

Just as I thought, having one peptide for a protein will not be enough to confidently say that peptide is truly from that protein. Therefore, I will need to delete Extracellular superoxide dismutase from my protein list. After looking at my transitions, Emma noticed that some of my lower abundance transitions were not the greatest.

For example, the transition highlighed in red below has a much lower intensity than the others:

![screen shot 2017-07-08 at 2 31 17 pm](https://user-images.githubusercontent.com/22335838/28151840-06edb9d2-6752-11e7-854a-af711e61306d.png)

And the fifth transition in this peptide isn't even visible:

![screen shot 2017-07-08 at 2 30 09 pm](https://user-images.githubusercontent.com/22335838/28151805-d5313ce8-6751-11e7-8964-92137d5b2300.png)

However, Emma also said that SRM is good at detecting low abundance peptides. It is up to me to decide whether or not I want them. She also pointed out that I had a few peaks that had significance intereference and were pretty sloppy:

![screen shot 2017-07-08 at 2 31 44 pm](https://user-images.githubusercontent.com/22335838/28151874-3384909c-6752-11e7-8c5f-aedacc1669f8.png)

Peaks like the one above were deleted immediately. I documented my target refinement process in [this Excel document](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_TransitionSelection_20170707/2017-07-08-Final-Transitions/2017-07-08-Target-Selection-Process-Notes.csv). In the end, I had about 135 transitions! I sent my list over to Emma and Steven for feedback one last time:

<img width="581" alt="screen shot 2017-07-12 at 10 43 40 pm" src="https://user-images.githubusercontent.com/22335838/28152140-b88e73d8-6753-11e7-95a0-cf97eee756da.png">

[Revised Skyline document](http://owl.fish.washington.edu/spartina/DNR_Skyline_SRM_20170707/Gigas-7-8-Revised-Transition-List.sky.zip)
