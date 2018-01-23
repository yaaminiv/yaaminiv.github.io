---
layout: post
comments: true
title: SRM Analysis Part 6
tags: DNR SRM
---

## Retracing my steps

Since I don't know where things are going wrong, I'm going back to everything I've done and making sure things are okay. The first thing I did was go through my [R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-06-NMDS-for-Technical-Replication.R) to see if there was anything that was incorrectly assigning samples to data or eliminating them. I couldn't find anything, so I started writing a [new R script for an NMDS plot after averaging technical replciates](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-10-NMDS-ANOSIM-for-Cluster-Analysis.R). As I was writing this script, I found that my dataframe `technicalReplicates` were having sample names alternating between -1 and -2, instead of just -1. I went all the way back and found that my [exported report from Skyline](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-06-Gigas-SRM-ReplicatesOnly-PostDilutionCurve-NoPivot-Report.csv) did not have samples O106-1 and O71-1!

I added in the [RAW files](http://owl.fish.washington.edu/spartina/DNR_SRM_20170728/Raw-Files/) for my missing replciates, files 10 and 28. These two samples didn't have any missing data, so I'm not sure why they weren't included in the first place! I also decied to readd the files that [originally displayed no chromatograms](http://yaaminiv.github.io/SRM-Analysis/) just to see if there might have been an importing error: files 3, 16, 22 and 34. Files 3, 22 and 34 displayed data so I kept them, but 16 did not. I removed it and tried imorting it a third time. It still didn't work, so I removed the file.

While I was in Skyline, I decided to check my PRTC peptides as well. I started by seeing what peaks were present in each PRTC peptide chromatogram. They all had one very clearly defined peak. The only peptide that did not have any peak picked was TASEFDSAIAQDK.

![unnamed-2](https://user-images.githubusercontent.com/22335838/30359183-e099a64c-97fd-11e7-9abc-5b4452d065f9.png)

![unnamed-3](https://user-images.githubusercontent.com/22335838/30359184-e0a30ec6-97fd-11e7-8813-f56c435ae40c.png)

![unnamed-4](https://user-images.githubusercontent.com/22335838/30359180-e08ecb46-97fd-11e7-829e-a0ef6f228aaa.png)

![unnamed-5](https://user-images.githubusercontent.com/22335838/30359178-e08dd5a6-97fd-11e7-8614-0d5ee8dd584b.png)

![unnamed-6](https://user-images.githubusercontent.com/22335838/30359182-e09451ce-97fd-11e7-87e3-1eed6dacf225.png)

![unnamed-7](https://user-images.githubusercontent.com/22335838/30359179-e08e2fd8-97fd-11e7-8e6f-819d171ef7c9.png)

![unnamed-8](https://user-images.githubusercontent.com/22335838/30359186-e0abcdcc-97fd-11e7-87d2-9eb5ea031525.png)

![unnamed-9](https://user-images.githubusercontent.com/22335838/30359185-e0a7f648-97fd-11e7-871a-99594019b044.png)

![unnamed-10](https://user-images.githubusercontent.com/22335838/30359187-e0afe452-97fd-11e7-872c-cb017476a0f8.png)

**Figures 1-9*. Peaks present in PRTC peptides. 

Using this document to [predict retention times](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_TransitionSelection_20170707/2017-07-08-Final-Transitions/2017-07-11-Predicted-SRM-Retention-Times.xlsx), I ensured that all of the proper peaks were picked. 

![unnamed-14](https://user-images.githubusercontent.com/22335838/30359553-749f8d96-9800-11e7-80ef-3851e332691c.png)

**Figure 10**. Proper peak selected for PRTC peptide.

I then (accidentally) saved my revised Skyline file under the same name and overwrote the original file in OWL. The Skyline document can be found [here](http://owl.fish.washington.edu/spartina/DNR_SRM_20170728/Analyses/2017-09-06-Gigas-SRM-ReplicatesOnly-PostDilutionCurve.sky.zip).

![unnamed-15](https://user-images.githubusercontent.com/22335838/30359554-74a04b3c-9800-11e7-9197-56dc5d9781e0.png)

**Figure 11*. Export settings for revised data.

I made sure the sequence file contained information for all of my samples, with the exception of O12 (one technical replicate is file 16, which has no data. I had to manually add the protein name CHOYP_ACAA2.1.1|m.30666 to the file since that protein name was the only one that wasn't exporting. Finally, I made sure I was missing no data when going through my R scripts. I resaved dataframes and images that were affected by the readdition of my samples. My NMDS clustering and ANOSIMs are still not producing anything desirable. I'm going to focus on making protein bar graphs for PCSGA instead.

![29ebc15e-0596-11e7-9f2d-8d40d9849dd5](https://user-images.githubusercontent.com/22335838/30359530-5207a944-9800-11e7-8b84-f22e82cb2f83.png)

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://the-responsible-grad-student.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>

{% endif %}

<script id="dsq-count-scr" src="//the-responsible-grad-student.disqus.com/count.js" async></script>
