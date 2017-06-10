---
layout: post
title: Skyline Attempt 3
---

## Remember that Skyline fail? 

We think it's fixed! Based on my [previous error rates](https://yaaminiv.github.io/Another-Skyline-Fail/), Emma and her team were able to figure out that brecan (a version of `pecanpie` produced by Brian Searle in Genome Sciences) "produces blib files that have a certain number of incompatibilities with Skyline (one of which is redundant peptide identifications). Jarrett Egertson wrote a script to get rid of these incompatibilities so that the brecan blib files work," (Timmins-Schiffman, pers. comm.). I saved the .blib file [here in OWL](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170524/2017-05-23-oyster-desearleinated.blib).

Using Skyline document settings from my [test document](https://yaaminiv.github.io/Skyline-Test-2/) and [first full Skyline attempt](https://yaaminiv.github.io/Full-Skyline-Analysis/), and results specifications from my [first try at SRM target selection](https://yaaminiv.github.io/Selecting-SRM-Targets/), I created a new Skyline document.

### Step 1: Peptide settings

I used the proteome file from my [Gigas-4-27 document](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170505/Gigas-4-27-oyster1-test.sky.zip):

![image-1](https://user-images.githubusercontent.com/22335838/27004381-482f26dc-4dbd-11e7-9c36-6db75023d19f.png)

![image-2](https://user-images.githubusercontent.com/22335838/27004382-48340986-4dbd-11e7-989d-911df53055bd.png)

![image-3](https://user-images.githubusercontent.com/22335838/27004380-482c53f8-4dbd-11e7-86ba-1fada1a47626.png)

![image-4](https://user-images.githubusercontent.com/22335838/27004378-482bbeac-4dbd-11e7-9375-7d6890054dc5.png)

Added the new "desearlinated" .blib:

![image-5](https://user-images.githubusercontent.com/22335838/27004379-482bd630-4dbd-11e7-8e1c-8396145c14c4.png)

![image-6](https://user-images.githubusercontent.com/22335838/27004383-4840a736-4dbd-11e7-9070-de3053540497.png)

![image-7](https://user-images.githubusercontent.com/22335838/27004384-48427ac0-4dbd-11e7-92bf-8d4d5595d157.png)

### Step 2: Import FASTA file

File >> Import >> FASTA >> Combined-gigas-QC.txt

### Step 3: Transition settings

![image-8](https://user-images.githubusercontent.com/22335838/27004485-bb7d5946-4dbe-11e7-99c9-f10772f9c2dc.png)

![image-9](https://user-images.githubusercontent.com/22335838/27004481-bb69e28a-4dbe-11e7-9d90-34a83d6731ea.png)

![image-10](https://user-images.githubusercontent.com/22335838/27004484-bb6c570e-4dbe-11e7-8320-7d35434887fa.png)

![image-11](https://user-images.githubusercontent.com/22335838/27004483-bb6c3486-4dbe-11e7-9c68-0bd3dd7469c1.png)

![image-12](https://user-images.githubusercontent.com/22335838/27004482-bb6b5b56-4dbe-11e7-8c80-fd8927009460.png)

### Step 4: Import demultiplexed .mzmL files

![image-13](https://user-images.githubusercontent.com/22335838/27004495-e73e1462-4dbe-11e7-9650-c0a3ef39fa84.png)

![image-14](https://user-images.githubusercontent.com/22335838/27004498-e742c70a-4dbe-11e7-9dd8-3fdb06ac13e3.png)

![image-16](https://user-images.githubusercontent.com/22335838/27004497-e7427fa2-4dbe-11e7-9061-643f0d53c7cd.png)

I didn't upload files 17, 21, 22 and 23 because oyster 21 was my blank (so there should be no data in it!) and oyster 17, 22 and 23 corresponded with my original 0107 sample had a bubble and did not read properly in the mass spec.

![image-17](https://user-images.githubusercontent.com/22335838/27004500-e7430350-4dbe-11e7-94a3-7fd5c23ff2a9.png)

![image-18](https://user-images.githubusercontent.com/22335838/27004499-e7431fac-4dbe-11e7-898f-7d4909e54729.png)

![image-19](https://user-images.githubusercontent.com/22335838/27004496-e741535c-4dbe-11e7-9f8e-8bc2559d9c8e.png)

Once my data files finish loading, I'm going to start error checking again to see how much of a difference the new .blib made.
