---
layout: post
title: SRM Analysis Part 3
---

## All aboard the NMDS Struggle Bus

The first step in my analysis is to see if my technical replicates cluster together without any normalization. If they do cluster, that means both mass spectrometer injections were similar and I can keep them for further analysis. If they do not cluster together, I must remove them from my dataset.

All of my work can be found in this [R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-06-NMDS-for-Technical-Replication.R). Here's what I did:

- Imported [my non-pivoted dataset](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-06-Gigas-SRM-ReplicatesOnly-PostDilutionCurve-NoPivot-Report.csv)
- Merged Skyline SRM data with the [sequence file](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-07-28-SRM-Samples-Sequence-File.csv) and [biological replicate information](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-06-Biological-Replicate-Information.csv)
  - When I merged my data with the biological replicate information, somehow my OBLNK data was not conserved in the merge. This is okay since I wasn't planning on analyzing my procedural blanks in this step anyways, but it's still weird.
  - Wrote out the [master dataframe as a .csv](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-07-Master-SRM-Data-BiologicalReplicates-NoBlanks-NoPivot.csv)
- Subsetted Area, Protein, Peptide, Transition and sample identifiers for NMDS analysis
  - Merged Protein, Peptide and Transition information into one column to use as row names
  - Cast my dataframe using `reshape2` so it could be used with metaMDS
  - Saved the [cast dataframe as a .csv](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-07-SRM-Data-NMDS-Pivoted.csv)
- Used metaMDS to make an NMDS plot
  - Was unsuccessful using the Bray-Curtis distance, even when log(x+1) transforming the data

<img width="1196" alt="screen shot 2017-09-08 at 9 22 16 am" src="https://user-images.githubusercontent.com/22335838/30221302-9c769724-9477-11e7-929e-52941a98cc6a.png">

<img width="1189" alt="screen shot 2017-09-08 at 9 25 51 am" src="https://user-images.githubusercontent.com/22335838/30221348-c4b8a0e2-9477-11e7-8425-d9a6ad4cfaa0.png">

**Figures 1-2**. Unsuccessful NMDS plot and corresponding Shepard plot using Bray-Curtis distance.

<img width="1183" alt="screen shot 2017-09-08 at 9 38 18 am" src="https://user-images.githubusercontent.com/22335838/30221791-89aa947c-9479-11e7-9030-330f95b9e126.png">

<img width="1175" alt="screen shot 2017-09-08 at 9 27 12 am" src="https://user-images.githubusercontent.com/22335838/30221402-f0f9f160-9477-11e7-8060-0a2b663179d4.png">

**Figures 3-4**. Unsuccessful NMDS plot and corresponding Shepard plot using log(x+1) transformed data and Bray-Curtis distance.

  - Was successful when using Euclidean distances

<img width="1195" alt="screen shot 2017-09-08 at 9 23 08 am" src="https://user-images.githubusercontent.com/22335838/30221314-a88e5830-9477-11e7-8ca0-4f590d42bf96.png">

<img width="1193" alt="screen shot 2017-09-08 at 9 26 04 am" src="https://user-images.githubusercontent.com/22335838/30221349-c4f70f1c-9477-11e7-8d97-783580732826.png">

**Figures 5-6**. Preliminary NMDS plot using Euclidean distances and corresponding Shepard plot.

Now I'll work on color-coding my plot so I can see if the technical replicates are clustering togther. I'm meeting with Julian tomorrow, so I'll ask him for his advice on using raw Euclidean distances instead of Bray-Curtis distances. After I see how my replicates cluster, I can average them and look primarily at site and eelgrass differences!
