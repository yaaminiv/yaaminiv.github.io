---
layout: post
title: Reproducible SRM Analysis
---

## R&R: Review and Reproduce

Last week, I created a [workflow for reproducing my SRM data and analyses](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-09-28-SRM-Skyline-Data-Pipeline.ipynb). Steven went through the pipeline as much as he could and posted the following issues:

1. [Opening new Skyline file](https://github.com/RobertsLab/project-oyster-oa/issues/10)

I forgot to use the specifif verbage Skyline does! I asked the user to open a new Skyline document, which is the same as opening a blank document.

2. [Source of .blib?](https://github.com/RobertsLab/project-oyster-oa/issues/11)

Emma and her team made the .blib I used for my DIA and SRM analyses, but I did not explain how I got the file. I added the explanation Emma gave me, as well as a link to my original lab notebook entry with this information.

3. [Provide brief explanation of major steps](https://github.com/RobertsLab/project-oyster-oa/issues/9)

Because I guess it makes sense to let the user know what they're doing and why they're doing it...

4. [Fail at step 2d](https://github.com/RobertsLab/project-oyster-oa/issues/8)

Whoops. Skyline requires a FASTA file, but I linked a .txt file with the same information. On the Windows machine I found the corresponding FASTA file and uploaded it to owl. I also fixed the link in the protocol. When I tried following my instructions to populate the analyte tree (Step 2d), copying and pasting the sequence information did not work! Skyline daily has been kind of annoying with this in the past. To make things easier, I updated the instructions so the user would only have to import the file, not open and copy and paste sequences with varying levels of success.

Clearly no matter how explicit I think my instructions are, it's always different when other people look at it! Laura and Grace are going to continue going through my protocol. Hopefully they catch something I didn't and we figure out why my technical replication is funky.
