---
layout: post
title: Aggregating Metaanalysis Data
---

## I became well-aquainted [NCBI SRA database](https://www.ncbi.nlm.nih.gov/sra/) today

It's not the most user-friendly database, but it's still pretty great! Based off of [Sean and Grace's previous searches](http://rpubs.com/seanb80/267942) and some of my own searches, I found *C. gigas* methylation data. You can read up on my methods [here](https://github.com/RobertsLab/paper-gigas-metaanalysis/blob/master/notebooks/2017-04-18-Data-collection.ipynb).

I recorded the following metadata for each run I found:

- Project code
- SRA study code
- Experiment code
- Experimental Design
- Sample type
- Library information (Name, Platform, Strategy, Source, Selection, Layout)
- Run code
- Run information (Spots, Bases, Size, GC Content)
- Submitted by
- Date published

All of the metadata is in [this spreadsheet](https://github.com/RobertsLab/paper-gigas-metaanalysis/blob/master/data/metaanalysis-data-sources.xlsx). Tomorrow, I'll download the data (apparently it's not a straightforward process).
