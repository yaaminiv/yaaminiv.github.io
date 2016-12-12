---
layout: post
title: BCA Assay Trial 2
---

## If at first you don't succeed, trial trial again.

Happy #microplatemonday! With a working microplate reader available to us, we proceeded (again) with the BCA Assay. We followed the [protocol](https://yaaminiv.github.io/BCA-Assay-Trial-1/) generated during our last attempt. The only difference between what is listed in that entry and what happened during our attempt is that I broke the bottom of microplate well E9. Sample O07 only had two replicates (E7 and E8).

#### **Some photos from our lab work**:

Here I am pipetting 10 µL of my sample for the microplate
![pipetting sample](https://raw.githubusercontent.com/yaaminiv/yaaminiv.github.io/master/images/pipettingsample.jpg)

Laura using a multipipetter to add 200 µL of our BCA working reagent to each well
![multipipetter](https://github.com/yaaminiv/yaaminiv.github.io/blob/master/images/multipipetting.jpg)

Our completed microplate! Well A1 is in the top left corner.
![mircoplate](https://raw.githubusercontent.com/yaaminiv/yaaminiv.github.io/master/images/microplate.JPG)

#### **Once in the Genome Sciences Building, we did the following**:
- Using an Eppendorf ThermoMixer C, we incubated our plate for 30 minutes at 37ºC
- Vortexed plate to mix solutions thoroughly
- Read plate

#### **Calculations**
In order to start the Mini-Trypsin digestion, we need to know the volume of our sample that contains 100 µg of protein. To calculate this volume, we used the following steps.

- Calculate average wavelength for each standard and unknown samples using wavelength for each replicate
- Substract average absorbance for the blank standards from the average absorbance of the remaining standards and unknown samles
 - This is the **blank-corrected absorbance**
- Create a scatterplot for the standards
 - y-axis: BCA concentration (µg/µL)
 - x-axis: blank-corrected absorbances
 - Add polynomial trendline
 - Display equation and R-squared value
- Using the trendline equation in the scatterplot, calculate protein concentration from absorbance
- Multiply each concentration by three
 - We do this because we diluted our sample 1:2 with 50 mM NH4HCO3 in 6M Urea
- For each sample, calculate volume (µL) needed for 100 µg protein
 - Need 100 µL to start Mini-Trypsin digestion
 - Calculate how much 50 mM NH4HCO3 in 6M Urea needed for each sample to reach 100 µL volume
  - 100 µL total volume - volume for 100 µg protein

The scatterplot I generated and a table of calculations are below:

Check back Tuesday for our Mini-Trypsin digestion #trypsintuesday
