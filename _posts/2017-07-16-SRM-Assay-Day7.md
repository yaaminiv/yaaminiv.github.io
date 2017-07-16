---
layout: post
title: SRM Assay Day 7
---

## Status quo?

I checked the mass spectrometer this morning and saw that the injection pattern looked normal.

![capture-1](https://user-images.githubusercontent.com/22335838/28251522-f9f57244-6a33-11e7-9f29-d486136ae465.PNG)

**Figure 1**. This morning's mass spectrometer injection pattern.

I figured this meant that my samples were good to go! I checked the files that ran overnight, and found that files 86 (O01), 87 (O118) and 93 (O113) had no data. I'll need to prepare these samples again. I stopped by the lab to pick up the QC and PRTC vials, then made my way down to UWPR. I added 30 µL of the final acetonitrile solution to the 10 µL of PRTC+BSA and vortexed. I then added the 40 µL of solution to the QC vial. We are now able to run 12 more QC injections, so we'll need to add a new QC around QC36.

I checked the QC23 raw file, which was the one I ran while I slept. It looked good, so I opened the file in Skyline to compare it with the other QCs.

![capture-2](https://user-images.githubusercontent.com/22335838/28251521-f9f57582-6a33-11e7-88a2-999a5ff99338.PNG)

**Figure 2**. RAW file for QC23.

When I opened the file in Skyline however, I saw that some peaks were not detected in that QC sample. Additionally, some of the peaks that were detected did not look like the other files.

![capture-3](https://user-images.githubusercontent.com/22335838/28251519-f9f49ea0-6a33-11e7-9b52-6451882b1a39.PNG)

**Figure 3**. Peptide undetected in QC23 but present in QC5.

![capture-4](https://user-images.githubusercontent.com/22335838/28251520-f9f5599e-6a33-11e7-9c37-e8153b48f33f.PNG)

**Figure 4**. Peptide detected in QC23 that looks abnormal.

When I looked at the sample queue, I saw that I had queued QC23 incorrectly by assigning it the oyster sample method instead of the QC method!

![capture-5](https://user-images.githubusercontent.com/22335838/28251524-f9f8423a-6a33-11e7-8e26-28157dfd5560.PNG)

**Figure 5**. Incorrectly queued QC23 (bottom) versus correctly queued QC22 (top). While I specified the sample placement and injection volume correctly, I ran it using the oyster method instead of the QC method.

After seeing this, I checked to make sure none of my other files were added to the sequence file with the wrong method. The only file that was queued incorrectly was QC23. However, I realized that I skipped straight from bivalve 88 to bivalve 93. This means that there are no files for bivalve 89, 90, 91, and 92 because I skipped them, not because I am missing data.

Based on my notes, I prepared new samples for those that did not collect any data during an injection. I remade samples O01, O12, O22, O113 and O118. I place the mass spectrometer vials in Plate 2.

**Table 1**. Sample arrangement for make-up injections on Plate 2.

| **Plate 2** | **1** | **2** | **3** | **4** | **5** |
|:-----------:|:-----:|:-----:|:-----:|:-----:|:-----:|
|    **B**    |   O1  |  O12  |  O22  |  O113 |  O118 |

I added the samples to the sequence file, adding one injections for all samples except O01. Because I did not collect any data for O01 on either the first or second injection, I added it twice.

![capture-6](https://user-images.githubusercontent.com/22335838/28251523-f9f61744-6a33-11e7-9202-0f37e75e893d.PNG)

**Figure 6**. Sequence file with make-up samples at the bottom.

The last thing I wanted to do before I left was check that the next QC I run, QC24, showed peak patterns similar to the previously run QCs. When the machine finished collecting data, I imported the file into Skyline.

![capture-7](https://user-images.githubusercontent.com/22335838/28251581-4dae7bfa-6a35-11e7-89ed-2221046a71bf.PNG)

**Figure 7**. QC24 in the Skyline PRTC+BSA document.

I saw that I had the same problem I did with QC23, so I called Emma. While talking to her, she had me check the PRTC peaks in the oyster samples. Since the PRTC and oyster peaks looked good, she figures that the QC sample may have degraded. 

![capture-8](https://user-images.githubusercontent.com/22335838/28251733-7d0c3326-6a38-11e7-812e-015f2189cb5b.PNG)

**Figure 8**. QC24 vs. bivalve 97's PRTC peptide peaks.

![capture-9](https://user-images.githubusercontent.com/22335838/28251731-7d085e54-6a38-11e7-83c5-cc2af8ff90de.PNG)

**Figure 9**. QC24 vs. bivalve 97's oyster peptide peaks.

She had 5 µL of QC left in the freezer from a previous experiment, so I added 15 µL the final acetonitrile solution to that and put 20 µL of the final solution in a new mass spectrometry vial. I placed it in Plate 1-A3, so I adjusted the sequence file accordingly. This will give me five additional QC injections, from QC25 to QC30. This should be enough to finish my samples. I proceeded with my injections and decided to check on the mass spectrometer after it ran the next QC.

![capture-10](https://user-images.githubusercontent.com/22335838/28251732-7d087ca4-6a38-11e7-959b-a536497a6663.PNG)

**Figure 10**. Adjusted sequence file.
