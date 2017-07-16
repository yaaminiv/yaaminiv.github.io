---
layout: post
title: SRM Assay Day 6
---

## Murphy's Law is always relevant

Before I went to Manchester today, I checked the mass spectrometer progress. For bivalve 74 (O22) and bivalve 75 (O12), the second injection didn't have any data. I noted the sample numbers down so I could rerun them after the rest of my samples were injected a second time.

While I was at Manchester in the morning, the analytical column blew! 

![capture](https://user-images.githubusercontent.com/22335838/28251317-a4bcc3da-6a2f-11e7-8e48-04cf95dffb4b.PNG)

**Figure 1**. Pressure on the mass spectrometer. From the abnormal pattern, you can see that there was something wrong with the column.

Emma went down to UWPR after noon to investigate. From her notebook:

- Files 82, 83, 84 and QC18 are bad (*Yaamini's note: 82 and 83 are actually QCs, and QC18 is actually O01*)
- Cut a couple of mm off bottom of column, reattached, and injected a QC (19)
- Column blew at beginning of gradient. I will need to attach a new column
- New 30 cm analytical column
- Analytical flow, 5% solvent B (ACN), flow at 0.2 (~2430 psi)
- Analytical flow (50% ACN, 0.3 flow) and let run 10 minutes (~3430 psi)
- Cut column to 30 cm
- Switch back to 5% ACN for 10 minutes, 0.3 flow (3860 psi)

Emma left UWPR around 3 p.m., and I made it to the facility around 5:30 p.m. after coming back from the hatchery. Before she left, she injected QC20. When I got there, I injected QC21 and QC22. Both had good looking mass spectrometer signals and the files looked good when I opened them in Skyline. 

![capture-9](https://user-images.githubusercontent.com/22335838/28251349-1dbfde8e-6a30-11e7-94cb-d1161ff99746.PNG)

**Figure 2**. Comparison of QC5 and QC22. Even though the retention time shifted between the samples, the peaks have the same pattern. After seeing this, I began running oyster samples.

Because we only had enough QC in there for about 25 injections, and we'd run 24 QCs, I only had enough solution to run more QC before I needed to add more. I was in a rush to make it to UWPR, so I forgot to bring the QC from the lab. Emma said I could stop running QCs between every set of five samples, and I could just run one QC in the night. When I worked out the math, I only skipped one QC injection. Just to be safe, I added 100 µL more to the blank vial.

The file names in the sample queue were off when I came to UWPR because of all of the adjusting done to recalibrate the machine. I fixed the sequence file and made sure all file names were unique, all samples were queued to be injected, and all file names were associated with the right sample. 

![capture-10](https://user-images.githubusercontent.com/22335838/28251362-5889f0e0-6a30-11e7-972c-b1ad19fcc231.PNG)

**Figure 3**. Modified sequence file and sample queue.

As per Emma's instructions, I restarted the queue with file 87 (O14). Once the sample finished, I checked it to make sure the RAW file and Skyline peaks looked normal. 

![capture-12](https://user-images.githubusercontent.com/22335838/28251377-93f5d5ea-6a30-11e7-8cb5-b184ab7b3c67.PNG)

**Figure 4**. RAW file for bivalve 87.

![capture-13](https://user-images.githubusercontent.com/22335838/28251376-93f0e756-6a30-11e7-82ff-9da3e00eb2c6.PNG)

**Figure 5**. Skyline peaks for bivalve 87 looked similar to previous files, so I deemed them acceptable.

After this I added enough samples to the queue to last the night and left UWPR. 

![capture-11](https://user-images.githubusercontent.com/22335838/28251369-800d4630-6a30-11e7-943b-ef156c5a90a6.PNG)

**Figure 6**. Status of injection patterns when I left UWPR.

Tomorrow morning I will come in and add more QC, as well as prepare samples that need to be rerun.
