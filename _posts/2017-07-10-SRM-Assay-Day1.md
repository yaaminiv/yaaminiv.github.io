---
layout: post
title: SRM Assay Day 1
---

## It's SRM assay time!

### Mass Spectrometer Set-Up

For the SRM assay, we're using a Thermo TSQ Vantage machine since it is better at SRM detection. To use the machine, we followed a protocol similar to our [DIA set-up](https://yaaminiv.github.io/Mass-Spec-Setup/). Emma attached a previously-prepared 3 cm trap to the mass spectrometer. I set Solvent A to 5% and the flow to 0.2 µL/min using the "trapping" method. I increased flow to 0.5 µL/min after ensuring that the pressure was holding and there were no leaks.

Emma added the first column, but the pressure was low (around 94 psi). She checked the column and found it empty! She then added a new column. I set Solvent B to 5% and changed flow to 0.2 µL/min under the "analytical" method. After a few minutes, the system pressure crashed due to a leak in the trap. This began a series of trial-and-error solutions to ensure the system was running smoothly:

- Reattached the trap after seeing the leak
- Pressure crashed again
  - The column itself was long, so it's possible the pressure inside the column was too high
  - Emma cut the column to 30 cm and tried again
- Presure crashed: leak at the base of the column
  - Trimmed column once more, replaced t-junction and tried again
- Pressure reached ~4800 psi, which was too high. The system crashed again
  - Detattched analytical column
  - Added a new trap
    - Broke new trap, added another trap which was 5 cm instead
    - Set method back to "trapping" with Solvent A at 5% and flow at 0.2 µL/min
    - Increased flow to 0.5 µL/min (about 242 psi max)
  - Attached analytical column
    - Flow at 0.2 µL/min, Solvent B 5%, analytical method
- Pressure crashed due to a leak in the trap
  - Removed column and added a new one
    - Kept old column just in case
  - New column produced a solvent droplet, so I set a ten minute timer
    - Pressure reached ~2780 psi max
    - Switched flow to 0.3 µL/min and 50% Solvent B
- Trap leaked
  - Trimmed off trap where leak was
  - Reattached trap
  - Tried again at 0.3 µL/min flow
  
At this point, the column and trap combination finally worked! Once I saw a solvent droplet form at the tip of the column, I set a ten minute timer. Emma took care of resetting the flow and solvent ratios while I moved onto the next step of preparation.

### PRTC Addition

I completely forgot about the [PRTC addition](https://yaaminiv.github.io/PRTC-preparation/) step before running samples until Emma mentioned it on Friday. We didn't have any PRTC in the lab, so I borrowed 4-20 µL aliquots from Emma's lab. I labelled new centrifuge tubes for each of my samples and added 9.4 µL of the final solvent (3% Acetonitrile + 0.1% Formic Acid) to each tube. I started to add 1.9 µL of PRTC to the sample tubes when I realized I probably wouldn't have enough PRTC for all of my samples. Therefore, I just added it to the samples I wanted to run on the first plate (see plate arrangement below). I then added 7.5 µL of sample to each tube. I vortexed the centrifuge tubes down and then added 15 µL of the solution to each sample glass mass spectrometer vial. For some reason I wasn't successful at generating 15 µL of mass spectrometery-ready solution for samples 21, 49, 51 and 71, so I had to use another 7.5 µL of raw peptide sample to create a new solution. I now have 25 samples raedy for analysis!

Emma also provided me with 10 µL of a quality control standard. I added 30 µL of Final Solution to this vial and pipetted it into a QC mass spectrometry vial. Emma took 100 µL of the Final Solution and added it into a different vial to use as a blank.

### Transition reduction

Emma realized that the 150 transition maximum she provided needs to also include PRTC transitions! We edited down the transitions together. My final transition list can be found [here](). I also edited my [Skyline document]() and [transition selection Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/2017-07-07-SRM-Target-Identification-in-Skyline.ipynb) to reflect these changes.

### Loading Samples

Because I only had enough PRTC to prepare half of my samples, I split my 50 samples between the two plates in the mass spectrometer. Ideally, I would run the first injection of all 50 samples first, then run the second injection.

**Table 1**. Plate 1 layout for mass spectrometery. These samples had PRTC added to them and were ready for SRM analysis.

| **Plate 1** | **1** | **2** | **3** | **4** | **5** |
|:-----------:|:-----:|:-----:|:-----:|:-----:|:-----:|
|    **B**    |  O49  |  O52  |  O102 |  O01  |  O122 |
|    **C**    |  O17  |  O14  |  O71  |  O145 |  O128 |
|    **D**    |  O39  |  O113 |  O12  |  O99  |  O103 |
|    **E**    |  O56  |  O10  |  O22  |  O118 |  O06  |
|    **F**    |  O08  |  O04  |  O106 |  O21  |  O51  |

**Table 2**. Plate 2 layout for mass spectrometry. These samples will be prepared at a later date.

| **Plate 2** | **1** | **2** | **3** | **4** |  **5** |
|:-----------:|:-----:|:-----:|:-----:|:-----:|:------:|
|    **B**    |  O32  |  O60  |  O101 |  O91  |  O100  |
|    **C**    |  O137 |  O96  |  O46  |  O90  |  O147  |
|    **D**    |  O30  |  O31  |  O131 |  O35  |   O24  |
|    **E**    |  O43  |  O40  |  O26  |  O78  |  O124  |
|    **F**    |  O64  |  O140 |  O66  |  O121 | OBLNK2 |

I had to run to Montlake for some security paperwork, so Emma prepared the queue and started my sample analysis. I should be done with all of my injections around July 18!
