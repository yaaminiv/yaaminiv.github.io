---
layout: post
comments: true
title: SRM Analysis Part 7
tags: DNR SRM
---

## Another Skyline saga

To figure out what was going on with my technical replicates, Emma said she'd take a look at my Skyline document. Here's what she found:

> I found some discrepancies between your settings and settings I had for one of my SRM files (one of which was that you told Skyline you had DIA data...). I've updated the settings and I am re-importing your data. In peptide settings, the 13C modifications weren't checked (PRTC modifications). In transition settings, mine looked like the attached, which were different from yours. I think the most significant would be the Full-scan settings.

I'm not sure why my Skyline settings were off! Emma and I made the Skyline document together, but we may not have actually adjusted the settings. Here are my old, incorrect Skyline settings:

![image-1](https://user-images.githubusercontent.com/22335838/30889849-fc76fbae-a2dd-11e7-9499-db816556ea00.png)

![image-2](https://user-images.githubusercontent.com/22335838/30889854-fc8d023c-a2dd-11e7-83a0-473ff424444e.png)

![image-3](https://user-images.githubusercontent.com/22335838/30889853-fc8c9428-a2dd-11e7-90e0-16b496c80ef2.png)

![image-4](https://user-images.githubusercontent.com/22335838/30889847-fc74efb2-a2dd-11e7-9a06-0f2b49c91484.png)

![image-5](https://user-images.githubusercontent.com/22335838/30889848-fc7526a8-a2dd-11e7-9d86-c88d0d9598db.png)

![image-6](https://user-images.githubusercontent.com/22335838/30889846-fc734fb8-a2dd-11e7-91b5-2ea5a3bf4443.png)

![image-7](https://user-images.githubusercontent.com/22335838/30889851-fc8ad700-a2dd-11e7-9918-3f2499160d84.png)

![image-8](https://user-images.githubusercontent.com/22335838/30889852-fc8b9546-a2dd-11e7-8543-ace27011be79.png)

![image-9](https://user-images.githubusercontent.com/22335838/30889850-fc88d716-a2dd-11e7-99da-33a1ee367dd1.png)

![image-10](https://user-images.githubusercontent.com/22335838/30889855-fc90ccbe-a2dd-11e7-937a-0f5cdcd81c68.png)

![image-11](https://user-images.githubusercontent.com/22335838/30889856-fca188f6-a2dd-11e7-9160-59597ac6a50e.png)

**Figures 1-11**. Old Skyline settings

And here are the actual settings:

<img width="1280" alt="screen shot 2017-09-13 at 2 01 25 am" src="https://user-images.githubusercontent.com/22335838/30889900-514696ee-a2de-11e7-9466-0b5ccc6f3e3a.png">

<img width="1280" alt="screen shot 2017-09-13 at 2 01 30 am" src="https://user-images.githubusercontent.com/22335838/30889899-51464518-a2de-11e7-82d9-cb623398da9f.png">

<img width="1280" alt="screen shot 2017-09-13 at 2 01 32 am" src="https://user-images.githubusercontent.com/22335838/30889896-513808c2-a2de-11e7-9433-c822fdf4d3c2.png">

<img width="1280" alt="screen shot 2017-09-13 at 2 01 34 am" src="https://user-images.githubusercontent.com/22335838/30889897-5141f83c-a2de-11e7-9807-d1f11ccbdf50.png">

<img width="1280" alt="screen shot 2017-09-13 at 2 01 36 am" src="https://user-images.githubusercontent.com/22335838/30889901-514bf6f2-a2de-11e7-9194-22601abe8080.png">

<img width="1280" alt="screen shot 2017-09-13 at 2 01 37 am" src="https://user-images.githubusercontent.com/22335838/30889895-512cc7e6-a2de-11e7-918e-646e59b639eb.png">

<img width="1280" alt="screen shot 2017-09-13 at 2 01 52 am" src="https://user-images.githubusercontent.com/22335838/30889898-51424a44-a2de-11e7-88d8-e65f80d80ba6.png">

<img width="1280" alt="screen shot 2017-09-13 at 2 01 53 am" src="https://user-images.githubusercontent.com/22335838/30889902-514f464a-a2de-11e7-8ea1-4d5c037c483a.png">

<img width="1280" alt="screen shot 2017-09-13 at 2 01 55 am" src="https://user-images.githubusercontent.com/22335838/30889903-5158c684-a2de-11e7-99ff-2270bf9f18e9.png">

<img width="1280" alt="screen shot 2017-09-13 at 2 01 57 am" src="https://user-images.githubusercontent.com/22335838/30889905-517cec76-a2de-11e7-9368-25114adf7478.png">

<img width="1280" alt="screen shot 2017-09-13 at 2 01 58 am" src="https://user-images.githubusercontent.com/22335838/30889904-515f0e36-a2de-11e7-9953-b71341db865d.png">

**Figures 12-22**. Revised Skyline settings

Once again, I fixed all transitions and ensured all peaks were picked correctly using my old Skyline document. Emma was unable to find any chromatograms in samples 14 and 15.

![image-1](https://user-images.githubusercontent.com/22335838/30889946-90f86b50-a2de-11e7-88ae-69274a2155c4.png)

![image-2](https://user-images.githubusercontent.com/22335838/30889943-90f112a6-a2de-11e7-84e2-3ec471e26722.png)

![image-3](https://user-images.githubusercontent.com/22335838/30889944-90f3b95c-a2de-11e7-95c1-63f3222d9ef7.png)

![image-4](https://user-images.githubusercontent.com/22335838/30889945-90f60dd8-a2de-11e7-8ccb-17208f8878c5.png)

**Figures 23-26**. Examples of transitions that needed to be fixed.

After I cleaned the data, I uploaded my [Skyline document](http://owl.fish.washington.edu/spartina/DNR_SRM_20170728/Analyses/2017-09-12-Gigas-SRM-ReplicatesOnly-PostDilutionCurve-RevisedSettings.sky.zip) to OWL and [exported my area data](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-12-Gigas-SRM-ReplicatesOnly-PostDilutionCurve-NoPivot-RevisedSettings-Report.csv).

I ran through my [NMDS](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-06-NMDS-for-Technical-Replication.R) and [ANOSIM](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-10-NMDS-ANOSIM-for-Cluster-Analysis.R) scripts again to see if anything changed. The new settings slightly improved clustering, but not enough for me to feel confident in my technical replication.

![nonnormalized](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-09-08-NMDS-TechnicalReplication-NotNormalized.jpeg)

**Figure 27**. NMDS for technical replication using nonnormalized data.

![normalized](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-09-08-NMDS-TechnicalReplication-Normalized.jpeg) 

**Figure 28**. NMDS for technical replication using normalized data.

Now I just need to figure out how to analyze and present my SRM data for PCSGA............

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
