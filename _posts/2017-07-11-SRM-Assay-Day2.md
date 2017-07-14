---
layout: post
title: SRM Day 2
---

## Our assay works!

How do we know this? We checked, obviously. When Emma opened some of the newly generated data in Skyline yesterday, she noticed that Skyline wasn't picking the best peaks. To ensure that 1) the SRM assay picked up the peaks in our transition list and 2) Skyline could quantify those peaks, I calculated predicted SRM retention times for my targets.

### SRM target confirmation

In [this Excel file](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_TransitionSelection_20170707/2017-07-08-Final-Transitions/2017-07-11-Predicted-SRM-Retention-Times.xlsx), I first regressed SRM retention times against DIA regression times for these PRTC peptides:

- LTILEELR
- ELGQSGVDTYLQTK
- SAAGAFGPELSR
- DIPVPKPK
- HVLTSIGEK
- SSAAPPPPPR

![picture1](https://user-images.githubusercontent.com/22335838/28190198-95cf2f46-67de-11e7-97c6-de7c88a4d6a7.jpg)

Using this equation and DIA retention times for my transition targets, I calculated predicted SRM retention times. My data can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_TransitionSelection_20170707/2017-07-08-Final-Transitions/2017-07-11-Predicted-SRM-Retention-Times.xlsx). 

In Skyline, I went through each peptide in two of my newly-collected oyster samples to ensure that Skyline picked the right peak. While the peaks were present around their calculation retention time, Skyline did not pick that peak for all but a few peptides.

For example, Skyline chose a peak at 34.4 min for this peptide, when the best peak is clearly around 28 minutes. The best peak matches the best retention time I calculated as well.

![capture-1](https://user-images.githubusercontent.com/22335838/28190411-a4f2b0be-67df-11e7-9b5d-5e78b4083d5a.PNG)

To fix this, I clicked on the peak I wanted Skyline to measure. Once I clicked on it, a black arrow appeared next to it.

![capture-2](https://user-images.githubusercontent.com/22335838/28190460-da3e7406-67df-11e7-9dc7-1003caccb435.PNG)

I then right-clicked and selected "Apply Peak to All", which had Skyline select the best peak for all samples in the document. Below, you can see that both samples have peaks made of three coeluting fragments around 28 minutes.

![capture-4](https://user-images.githubusercontent.com/22335838/28190462-da56eab8-67df-11e7-85c8-ff50c7e636a8.PNG)

I went through this process for all of my peaks. While Skyline may not be fully capable of picking the best peaks, the peaks were present! This is confirmation that the SRM assay works.

### New sample preparation

Using the PRTC I had leftover from [yesterday](https://yaaminiv.github.io/SRM-Assay-Day1/), I prepared more samples to run on the mass spectrometer. I added PRTC to the first 11 samples on Plate 2. After the PRTC addition, Emma realized that the PRTC I had been using was the stock solution, and not a dilution! So the concentration of PRTC I've been adding to my sample is much higher than I wanted.

### Mass spectrometer update

The SRM has been going quite well! I've been checking the machine every few hours. When I look at the progress, I go through the following checklist:

1. Check injection pattern

**Figure 1**. Normal injection pattern on the mass spectrometer. The drop in pressure is related to an injection.

2. Look at RAW files for newly finished samples

Almost all of my RAW files look good! The one file that didn't have any data in it was O01. If its technical replicate also doesn't have any data, then I will need to remake the sample.

**Figure 2**. RAW file for a good sample.

**Figure 3**. RAW file for sample O01.

**Figure 4**. Raw file for a blank.

**Figure 5**. Raw file for a PRTC QC sample.

3. Add any QC RAW files to the PRTC QC Skyline file
  - Emma set up a Skyline document so we can look at the retention time and peak quality of all quality control files. By looking at them right away, we're watching the retention times shift and ensuring that the assay does not decrease detection quality.
  
**Figure 6**. PRTC peaks in Skyline. Notice how the retention time shifts each QC file.

4. Add files to the queue

Things are going smoothly, but tomorrow I'll need to either 1) add PRTC to my remaining samples and run them or 2) proceed with the second injection for the samples I've done so far, depending on whether or not the PRTC I ordered arrives at the lab.
