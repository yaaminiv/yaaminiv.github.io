---
layout: plot
title: Remaining Analyses Part 6
---

## Looked into ROC curves

[Receiver operating characteristic curves](https://en.wikipedia.org/wiki/Receiver_operating_characteristic), or ROC curves, can be used with proteomic data to visualize how sensitive and specific a potential biomarker is to the environment. I wanted to make some ROC curves for [peptides that were differentially expressed between Puget Sound sites and Willapa Bay](https://yaaminiv.github.io/Remaining-Analyses-Part3/). In addition to this paper on [ROC analysis](https://ccrma.stanford.edu/workshops/mir2009/references/ROCintro.pdf), I found some resources on how to make ROC curves in R. It seems like I have a few  options:

1. [`pROC`](https://www.rdocumentation.org/packages/pROC/versions/1.10.0)
2. [`simple.ROC`](http://blog.revolutionanalytics.com/2016/08/roc-curves-in-two-lines-of-code.html)
3. [`roc.plot`](https://www.rdocumentation.org/packages/verification/versions/1.42/topics/roc.plot)
4. [Base R](http://rstudio-pubs-static.s3.amazonaws.com/220197_7131fb0b2455404cb95ea8f788d45828.html)

I haven't really looked into the differences between these methods, but what I can tell is that I'll need to first build a model with my data. The simplest model I can build is something like the following:

> protein abundance ~ site

However, I think my model will be more powerful (and the ROC curves more useful) if I include environmental variables and Alex's data as covariates:

> protein abundance ~ site + habitat + pH + temperature + DO + conductivity + chlorophyll + tidal depth + %c + %n + tissue mass + shell strength + shell density

Obviously, I can use covariate deletion to identify the most significant model before I proceed, or I can only include covariates that contribute to the differential protein expression. I think I'll also need to use a binomial model with a logit link, since all of the resources mention that the ROC analysis is evaluating successes and failures.

Bottom line: I need to wait until I get the environmental data before I can proceed.
