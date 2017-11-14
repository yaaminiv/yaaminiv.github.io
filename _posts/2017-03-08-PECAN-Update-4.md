---
layout: post
title: PECAN Update 4
---

## Reformatting PECAN inputs

[Jupyter notebook found here](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-03-08-Formatting-PECAN-Inputs.ipynb)

After comapring my PECAN inputs with Laura's we realized that I had been using the undigested proteome as a database this whole time! I had the digested file, but just didn't use it. To remedy this, I did the folliwng (see Jupyter notebook for explicit details):

- Converted the digested proteome to a .tabular file format
- Removed extraneous columns from my .tabular digested proteome (needed only the "Protein Name" and "Sequence" columns
- Updated the file path in my background proteome path list
- Selected 5 samples to analyze
- Updated mzML file path list
- Uploaded all PECAN inputs to OWL
- Downloaded folder from OWL into Documents directory on Roadrunner

Everything is now set up for Sean to run the code!
