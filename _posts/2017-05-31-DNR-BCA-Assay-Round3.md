---
layout: post
title: DNR BCA Assay Round 3
---

## All this protein but I'm still not swole

Once again, Jose and I used the protocol [generated in December](https://yaaminiv.github.io/BCA-Assay-Trial-1/) to quantify the protein concentrations in my samples, Laura's samples, and Rhonda's larval geoduck samples. It's a lot to keep track of! The end goal was to obtain the µL of protein required for 100 µg of protein in our samples, and the corresponding amount of 50 mM NH4HCO3 in 6M urea.

We finished running our plates in a timely manner when we noticed the protein concentrations we got were pretty low. Combined with the fact that our wells weren't very green, we deduced that we added too little of Reagent B to make the BCA working reagent. 

![badplate](https://cloud.githubusercontent.com/assets/22335838/26664970/f8df4a90-464a-11e7-9ae8-f8916a539c85.jpg)

**Figure 1**. Incorrectly prepared microplate. The wells that aren't pink look clear when they should be a bright green.

We repeated the process and got much better results. What's a day of labwork without a little snafu?

Here's the protocol we followed:

### Step 1: Prepare reagents for 100 samples

Combined, Jose and I needed to work through 88 samples. I calculated the amount of reagent we would need for 100 samples to give us some wiggle room.

- 1 set of 8 standard vials (recipe in [lab protocol](https://github.com/sr320/LabDocs/blob/master/protocols/ProteinprepforMSMS.md))
- 10 mL 50 mM NH4HCO3 made (recipe in [Laura's notebook](https://laurahspencer.github.io/LabNotebook/Proteo-Lab-Day-3/))
  - Need 22 µL 50 mM NH4HCO3 per sample, so 2200 µL or 2.2 mL total
- 6 mL Lysis Buffer made (recipe in [Laura's notebook](https://laurahspencer.github.io/LabNotebook/Proteo-Lab-Day-3/))
  - The lysis buffer is used to make the standards. Since we only need one set of eight standards for all of our samples, we did not need to replicate this recipe.

### Step 2: Add 50 mM NH4HCO3 to each sample tube

The first time we made the plates, we added 22 µL of 50 mM NH4HCO3 to 11 µL of sonicated sample. However, this only gave us 3 µL of wiggle room when pipetting the samples into the plates. It was difficult to work with and inefficient, so Jose and I modified the protocol the second time around. Below is the modified protocol, with changes in **bold**:

- Obtain tubes with **15 µL of sonicated sample**
- Add **30 µL 50 mM NH4HCO3** to each sample tube
- Vortex sample

The modified protocol gave us 15 µL extra sample in case we messed up. In the end, Laura and I will have (100-11-15)µL, or 74 µL, sonicated sample to work with for trypsin digestions.

### Step 3: Plan microplate arrangement

![microplate-arrangement](https://cloud.githubusercontent.com/assets/22335838/26664972/fcd6074c-464a-11e7-8064-9af2a152ec16.JPG)

**Figure 2**. Microplate arrangement. A total of four plates were used for our samples.

### Step 4: Pipet 10 µL of either standard or sample into the corresponding microplate well

- Pipet all standards into designated wells first
- Add samples according to microplate arrangement (above)
- When finished pipetting a microplate, cover with parafilm and place in refrigerator

### Step 5: Prepare BCA working reagent

The BCA working reagent should only be prepared right before plate incubation and reading.

- ((8 standards x 5 plates) + 100 samples) x (3 replicates each) x (200 µl of working reagent per well) = 104,000 µL working reagent = 104 mL working reagent for 100 samples
- Used 100 mL Reagent A and 2000 µL (2 mL) Reagent B to make a 50:1 Reagent A to B ratio.

### Step 6: Added BCA working reagent to each well

- Obtain one microplate from the fridge
- Using a multichannel pipet, add 200 µL BCA working reagent to each well in the microplate
- When finished, cover with parafilm to prevent evaporation
- Repeat process for remaining plates

### Step 7: Incubate plates

Jose set up an incubator in FTR, so we used that instead of going to Genome Sciences

- Cover plates with aluminum foil since BCA working reagent is light-sensitive
- Incubate at 37 ºC for 30 minutes

At this point, I needed to run to a homework review session, so Jose finished out the remaining steps.

### Step 8: Used Genome Sciences plate reader

- Set up Varioskan to shake and read plates
  - Protocol: Open Session >> Brook >> 2017-05-21-Plates12-DNR (or something like that) >> Place plate in machine >> Press play button
- Export results
  - Data Processing >> Quick Export >> Matrix >> Save file on Desktop and email to yourself

Plates with my samples:

[Plate 2 data](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/BCA_analysis/Plate2-20170531.txt)

[Plate 3 data](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/BCA_analysis/Plate3-20170531.txt)

Plates with Jose and Laura's samples:

[Plate 1 data](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/BCA_analysis/Jose310517_plate1.txt)

[Plate 4 data](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/BCA_analysis/Jose310517_plate4.txt)

### Step 7: Calculated volume of protein needed for 100 µg per sample

[Plate 2 calculations](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/BCA_analysis/2017-5-31_Plate_2_BCA_analyses.xlsx)

![plate2-graph](https://cloud.githubusercontent.com/assets/22335838/26664934/8fca9258-464a-11e7-85b9-52c2c4700329.png)

[Plate 3 calculations](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/BCA_analysis/2017-5-31_Plate_3_BCA_analyses.xlsx)

![plate3-graph](https://cloud.githubusercontent.com/assets/22335838/26665327/afff0f10-464d-11e7-8f9d-b913a1520504.png)

These calculations are used in the first step of tomorrow's trypsin digestion!
