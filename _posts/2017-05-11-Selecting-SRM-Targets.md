---
layout: post
title: Selecting SRM targets
---

## Who are the lucky winners?

Now that I have a [full Skyline document](https://yaaminiv.github.io/Full-Skyline-Analysis/), I can select proteins I want to use for a Selected Reaction Monitoring assay. I'm going to pick proteins that are candidate biomarkers, either because they show clear patterns of variation between experimental conditions, or because they are ecologically significant.

### Step 1: Reimport data files

After Laura posted [this issue](https://github.com/sr320/LabDocs/issues/613), I realized I imported my raw files into Skyline instead of my [demultiplexed files](https://yaaminiv.github.io/Demultiplexing-Update-3/). Before I could proceed with any analyses, I had to replace the raw files with demultiplexed ones.

Under Edit >> Manage Results, I selected "Remove All" to clear the raw files.
![image-1](https://cloud.githubusercontent.com/assets/22335838/25960910/4183a6c0-362d-11e7-8e74-00ca50b175c8.png)
![image-2](https://cloud.githubusercontent.com/assets/22335838/25960911/41850ab0-362d-11e7-9ab0-afbeb3554f2e.png)
![image-3](https://cloud.githubusercontent.com/assets/22335838/25960908/41810c4e-362d-11e7-8a0d-036a65e013ff.png)

I then imported my demultiplexed files.
![image-4](https://cloud.githubusercontent.com/assets/22335838/25960909/4182842a-362d-11e7-8663-7187f479da59.png)
![image-5](https://cloud.githubusercontent.com/assets/22335838/25960912/418520cc-362d-11e7-8b53-121ed8caf063.png)

### Step 2: Remove files with no data

If you look at the top right corner of my Skyline document, you'll see there are files with no data (no associated histograms).
![image](https://cloud.githubusercontent.com/assets/22335838/25961029/ab4bede2-362d-11e7-978f-de95910aacd4.png)

Samples 17, 21, 22 and 23 produced no data in the Skyline document. They also weren't included in the [full .blib](https://yaaminiv.github.io/Full-Skyline-Analysis/). I checked the [mass spectrometery sequence file](http://owl.fish.washington.edu/spartina/January_2017_DNR_Raw_Data/2017_January_23.csv) and found that oyster 21 was my blank (so there should be no data in it!) and oyster 17, 22 and 23 corresponded with my original 0107 sample.

If you remember my mass spectrometer struggles, you'll recall that I had a bubble in that exact sample. This lead to three failed runs on the mass spectrometer (find more information [here for the first struggle run](https://yaaminiv.github.io/Mass-Spec-Updates/) and [here for the second and third struggle runs](https://yaaminiv.github.io/Mass-Spec-Update-2/)). After preparing a new O107 sample, I had a [successful run](https://yaaminiv.github.io/Mass-Spec-Update-3/), which lead to data-filled files of oyster 24 and 25. Emma confirmed that oyster 17, 21, 22 and 23 had no data, and that 24 and 25 had data by looking at [TIC content](http://owl.fish.washington.edu/spartina/January_2017_DNR_Raw_Data/2017_January_23_TIC_values.xlsx).

I again navigated to Edit >> Manage Results. I selected the files I wanted to remove and clicked "Remove."

![image-1](https://cloud.githubusercontent.com/assets/22335838/25961643/cc942eb8-362f-11e7-8455-5f47b9638401.png)
![image-2](https://cloud.githubusercontent.com/assets/22335838/25961644/cc956684-362f-11e7-90a7-7a18a82b3a73.png)
![image-3](https://cloud.githubusercontent.com/assets/22335838/25961642/cc936ce4-362f-11e7-906a-213309b5b5f4.png)

I'm still not sure what happened with oyster 8 and oyster 11 with regards to the full blib, so I left them in the Skyline document.

### Step 3: Export results

Under File >> Export >> Report...
![image-1](https://cloud.githubusercontent.com/assets/22335838/25961720/20f99a6a-3630-11e7-8227-b6093b439a4d.png)

...you get the following settings.
![image-2](https://cloud.githubusercontent.com/assets/22335838/25961719/20f82de2-3630-11e7-99ac-93a55562af38.png)

I selected "Transition Results" then clicked "Edit" to choose the parameters I was interested in.
![image-1](https://cloud.githubusercontent.com/assets/22335838/25961813/6d3383b4-3630-11e7-807f-76acbd4a9413.png)
![image-2](https://cloud.githubusercontent.com/assets/22335838/25961814/6d3411ee-3630-11e7-95af-9f7908dce1ec.png)

Then I exported my results!
![unnamed](https://cloud.githubusercontent.com/assets/22335838/25961925/c229c52c-3630-11e7-937b-fd3c5360d626.png)

I uploaded [my results](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170505/2017-05-11-transition-results.csv) to OWL, as well as my [revised Skyline document](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170505/Gigas-4-27-oyster1-test.sky).

The rest of my process is documented in [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/2017-05-11-Selecting-SRM-Targets.ipynb).
