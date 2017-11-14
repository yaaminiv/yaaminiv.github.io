---
layout: post
title: SRM Analysis
---

## Clean up, clean up

I spent the last few days cleaning my Skyline data. Here's what I did:

- Create a [new Skyline document](http://owl.fish.washington.edu/spartina/DNR_SRM_20170728/Analyses/2017-08-31-Gigas-SRM-ReplicatesOnly.sky.zip) with just my sample files
- Used my [sequence file](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-07-28-SRM-Sequence-File.xlsx) to make sure I had all of my data
- Remove sample files that had no data
  - Files 3, 16, 22, 34 and 88 had no data in them, even though I did not encounter these problems when I was looking at their RAW files during my mass spectrometer run. All files recorded PRTC peptides but not my sample peptides

![nodata-1](https://user-images.githubusercontent.com/22335838/30078327-b91d6760-9231-11e7-8c18-bf195c46054a.png)

![nodata-2](https://user-images.githubusercontent.com/22335838/30078325-b9112b26-9231-11e7-95a6-c228865a1eb6.png)

![nodata-3](https://user-images.githubusercontent.com/22335838/30078323-b9092a0c-9231-11e7-980b-51a126aa80e9.png)

![nodata-4](https://user-images.githubusercontent.com/22335838/30078324-b90aee82-9231-11e7-8234-601dd7d156e7.png)

![nodata-5](https://user-images.githubusercontent.com/22335838/30078328-b91de820-9231-11e7-9bb5-996d50f5a99b.png)

![nodata-6](https://user-images.githubusercontent.com/22335838/30078330-b9498502-9231-11e7-8288-e3fbe3d8cc10.png)

**Figures 1-6**. Files with no data present

- Ensure Skyline selected the correct peak
  - I used the [retention times I calculated earlier](https://yaaminiv.github.io/SRM-Assay-Day2/) to find the correct peak in file 2. I then used "Apply All" to have Skyline use the correct peak for all data files
- Quality checked selected peaks
  - I found that even after using "Apply All", Skyline made plenty of mistakes! For each peptide, I went through all of the sample files to make sure Skyline actually did identify the right peak. Some peptides had more incorrect peaks than others. I think my transitions that were cleaner in the DIA document had much higher success rate for correct peak selection. This is something that would be interesting to follow up on! For each incorrect peak, I took a screenshot and saved them [here](https://github.com/RobertsLab/project-oyster-oa/tree/master/images/DNR/2017-08-31-Skyline-Cleaning-Screenshots). I can use this information to produce an [error rate like I did for my DIA document](https://yaaminiv.github.io/Skyline-Error-Checking-Round2/).
  - Skyline has a problem selecting the correct peak in the following situations
    - When the correct peak is much less intense than other peak
    
![wrongpeak-1](https://user-images.githubusercontent.com/22335838/30078607-a047962e-9232-11e7-9865-c5a8a049daff.png)

![wrongpeak-4](https://user-images.githubusercontent.com/22335838/30078618-a98bdeb6-9232-11e7-8fe5-dbfa772a2071.png) 

**Figures 7-8**. Incorrect peaks selected by Skyline (top panel) compared with the correct peak in a different sample (bottom panel). I believe this happened because the incorrect peak has a higher intensity than the correct peak
   
    - When the correct peak doesn't exist. In this case, I right-clicked and selected "Remove Peak." This happened most often with files 14, 27, 60 and 127.
    
![removepeak-1](https://user-images.githubusercontent.com/22335838/30078879-33b99542-9233-11e7-84cf-3f52f0e2ec79.png)

![removepeak-2](https://user-images.githubusercontent.com/22335838/30078878-33a725ec-9233-11e7-9890-4daebac7e124.png)

![removepeak-3](https://user-images.githubusercontent.com/22335838/30078877-33a59254-9233-11e7-9002-960cd2aa25e4.png)

**Figures 9-11**. Examples where a sample did not have the desired peak.
    
    - When it's just being dumb
    
![wrongpeak-2](https://user-images.githubusercontent.com/22335838/30078906-50538d84-9233-11e7-819b-a486507a4e1d.png)

![wrongpeak-3](https://user-images.githubusercontent.com/22335838/30078907-5053d0fa-9233-11e7-8ab5-4e26cd912c92.png)

![wrongpeak-5](https://user-images.githubusercontent.com/22335838/30078909-505c8c7c-9233-11e7-8aeb-4199ebe00a06.png)

![wrongpeak-6](https://user-images.githubusercontent.com/22335838/30078908-5057ac84-9233-11e7-9fb0-530f6339dc5c.png)
    
 **Figures 12-15**. #SkylineFails
 
 - Fixed peak boundaries
 
While going through my data for the PRTC peptides, I noticed that there seemed to be a batch effect. I would expect all peptide abundances to be uniform between my samples since I added the same about of PRTC to each sample. However, some samples had higher abundances of PRTC peptides than others. The batch effects aren't as pronounced as [Laura's samples](https://github.com/laurahspencer/Geoduck-DNR/issues/1) and seem to vary in intensity between each peptide. I will need to talk to Emma to figure out how to analyze my data. I'm wondering if I have to use the same normalization method Laura did even though my abundance differences aren't as stark.

![unnamed-1](https://user-images.githubusercontent.com/22335838/30079204-4a929cfe-9234-11e7-83f5-ad7329963ac2.png)

![unnamed-2](https://user-images.githubusercontent.com/22335838/30079200-4a8c46f6-9234-11e7-9d5b-5b902456ee4c.png)

![unnamed-3](https://user-images.githubusercontent.com/22335838/30079202-4a8c9570-9234-11e7-9c46-d8124a1eeec3.png)

![unnamed-4](https://user-images.githubusercontent.com/22335838/30079203-4a8fbf5c-9234-11e7-9497-a011b09dbb87.png)

![unnamed-5](https://user-images.githubusercontent.com/22335838/30079201-4a8c2cd4-9234-11e7-9c47-dea0c6e50b7a.png)

![unnamed-6](https://user-images.githubusercontent.com/22335838/30079205-4aa58ab2-9234-11e7-9b02-4b252fdc31f6.png)

![unnamed-7](https://user-images.githubusercontent.com/22335838/30079209-4abeb44c-9234-11e7-9ea4-ad620d945ca0.png)

![unnamed-8](https://user-images.githubusercontent.com/22335838/30079206-4aa858c8-9234-11e7-9706-03ed3e7705eb.png)

![unnamed-9](https://user-images.githubusercontent.com/22335838/30079207-4aada4f4-9234-11e7-9a14-00251b60d662.png)

![unnamed-10](https://user-images.githubusercontent.com/22335838/30079208-4ab51932-9234-11e7-81c7-88cfcc08cc01.png)

**Figures 16-25**. PRTC abudances for each peptide.

Another thing I noticed with the PRTC peptides is that some peptides have much sloppier peaks than others. Not sure if this is something I should also refine, or if I should trust Skyline to pick the right PRTC peaks. Again, I'll ask Emma!

![unnamed-11](https://user-images.githubusercontent.com/22335838/30079404-f0560a7c-9234-11e7-9e01-cbcc436d4d60.png)

![unnamed-12](https://user-images.githubusercontent.com/22335838/30079403-f0544278-9234-11e7-8872-774f80e4ce0e.png)

![unnamed-13](https://user-images.githubusercontent.com/22335838/30079407-f0682874-9234-11e7-99b5-4c1c534a0cbe.png)

![unnamed-14](https://user-images.githubusercontent.com/22335838/30079402-f050b522-9234-11e7-9a27-3a357831b555.png)

![unnamed-15](https://user-images.githubusercontent.com/22335838/30079405-f0650cac-9234-11e7-8a84-f1af13808976.png)

![unnamed-16](https://user-images.githubusercontent.com/22335838/30079406-f065e96a-9234-11e7-8e96-f47acaf78b76.png)

![unnamed-17](https://user-images.githubusercontent.com/22335838/30079408-f06bd1d6-9234-11e7-8451-312ed2340eb8.png)

**Figures 26-32**. Sloppy peaks from PRTC peptides.

Finally, I exported my data. I have two files: [without pivoting replicates](http://owl.fish.washington.edu/spartina/DNR_SRM_20170728/Analyses/2017-09-02-Gigas-SRM-ReplicatesOnly-NoPivot-Report.csv) and [with a pivot](http://owl.fish.washington.edu/spartina/DNR_SRM_20170728/Analyses/2017-09-02-Gigas-SRM-ReplicatesOnly-Report.csv). The settings I used are now saved as "SRM-Gigas-Report" and can be seen below:

![export](https://user-images.githubusercontent.com/22335838/30079508-440c25a2-9235-11e7-9629-a1e004d3b922.png)

Now it's time to do some stats.
