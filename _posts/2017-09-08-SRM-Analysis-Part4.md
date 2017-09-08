---
layout: post
title: SRM Analysis Part 4
---

## R Help is Helpful

Using the help pages for [`ordiplot`](https://www.rdocumentation.org/packages/vegan/versions/2.4-2/topics/ordiplot), I was able to modify my code and make a better NMDS plot:

```
ordiplot(proc.nmds.euclidean, choices = c(1,2), type = "text", display = "sites") #Plot refined NMDS displaying only samples with their names
```
- choices: Specify which axes you want to display
- type: What you want displayed. Instead of "points," using "text" allows me to display the row and column names
- display: There are two options: sites and species. In the [previous NMDS plot](https://yaaminiv.github.io/SRM-Analysis-Part3/), sites are the white dots and species are the red crosshairs. Sites relate to my columns, which are my samples. Therefore, I only want to display sites.

I was unsure about whether or not to include PRTC peptides in my technical replicate cluster analysis. Yesterday I removed the PRTC peptides, which lead me to a plot that looks like this:

<img width="1194" alt="screen shot 2017-09-08 at 9 53 14 am" src="https://user-images.githubusercontent.com/22335838/30224277-f776515e-9482-11e7-98aa-a7ddb1a06189.png">

**Figure 1**. NMDS using euclidean distances and untransformed data without PRTC peptides.

Adding the peptides back in, I get a different cluster pattern:

<img width="1188" alt="screen shot 2017-09-08 at 9 52 56 am" src="https://user-images.githubusercontent.com/22335838/30224278-f77b6590-9482-11e7-8e4a-01e21a07cdd4.png">

**Figure 2**. NMDS using euclidean distances and untransformed data with PRTC peptides.

For both plots, it looks like there is variation that needs to be normalized before I reexamine my data. I emailed Emma, who said I should not use PRTC peptides. I saved the NMDS plot [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-08-NMDS-TechnicalReplication-NotNormalized.jpeg). She also said I should normalize my data with TIC content. I need to go through my [RAW files](http://owl.fish.washington.edu/spartina/DNR_SRM_20170728/Raw-Files/) and get the TIC content so I can continue analyzing data.  However, we need the program [`xcalibur`](https://www.thermofisher.com/order/catalog/product/OPTON-30487) to open up RAW files. It's not installed on Woodpecker, but Emma has access to it. I will see if I can access a Genome Sciences computer and look at my RAW files.
