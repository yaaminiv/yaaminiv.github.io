---
layout: post
comments: true
title: SRM Analysis Part 2
---

## ...Everybody Everywhere 

(i.e. Cleaning Round 2)

Before I can normalize my data, I need to check my [dilution curve](https://yaaminiv.github.io/SRM-Dilution-Calculations2/). The reason we make a dilution curve is to confirm that as the concentration of protein in my sample decreases, there is a concurrent decrease in abundance picked up by the mass spectrometer. If this isn't the case, then there's something funky with the assay!

The first thing I did was [duplicate the dilution curve Skyline file](http://owl.fish.washington.edu/spartina/DNR_SRM_20170728/Analyses/2017-09-06-GigasDilutionCurve.sky.zip) that was on Woodpecker so I could modify it. Laura made this document with both of our sample files. I then deleted all of the geoduck protein information and sample files.

![untitled](https://user-images.githubusercontent.com/22335838/30131003-02ab515c-9300-11e7-8049-f5d35395c0bc.png)

![untitled-2](https://user-images.githubusercontent.com/22335838/30131004-02afbc9c-9300-11e7-8dc1-0c05444f41f7.png)

![untitled-3](https://user-images.githubusercontent.com/22335838/30131005-02b5eb44-9300-11e7-9ae4-591a669a036d.png)

**Figures 1-3**. Creation of new Skyline document for oyster dilution curve

To select the correct peak, I looked at the retention time for the last sample I ran, file 132. Using that retention time, I found the correct peak. Because the diultions were run after Laura's samples, the peptides had earlier retention times than sample 132. As usual, Skyline had some trouble selecting the correct peak even after I used "Apply All." For each peptide, I made sure the correct peak was picked and adjusted the peak boundaries. Some peptides had really clean decreases in abundances. Others, like the one below, had a general decrease in abundance, with a slight increase in abundance in the middle. Since these peptides followed the overall pattern set by the dilution, I'll keep them for analysis.

![unnamed-4](https://user-images.githubusercontent.com/22335838/30131747-c57e7176-9302-11e7-8017-eb34197d1e58.png)

**Figure 4**. Example of a general decrease in abundance due to dilutions.

For dilutions 6, 7 and 8, the concentration of oyster proteins were really small. It was difficult to distinguish the peak in Skyline, so I did my best to pick the correct peak based on retention time. I also had to keep in mind that all of my peptides had a slight increase in retention time for the seventh dilution.

![unnamed-6](https://user-images.githubusercontent.com/22335838/30131820-0838676a-9303-11e7-9068-f5f26c802bd0.png)

**Figure 5**. Example of a peak that's difficult to pick out due to low abundance.

Sloppy peaks were a big problem for my multidrug resistant protein data from the fourth dilution onwards! Again, I tried my best to pick the correct peaks. If Emma thinks this is too messy to be reliable when I meed with her tomorrow, I can just exclude this protein from my analysis.

![unnamed-11](https://user-images.githubusercontent.com/22335838/30131910-4fa79d64-9303-11e7-9485-026252f33f45.png)

![unnamed-12](https://user-images.githubusercontent.com/22335838/30131906-4f8ce5f0-9303-11e7-9737-7c2ab7391126.png)

![unnamed-13](https://user-images.githubusercontent.com/22335838/30131909-4f97d32a-9303-11e7-9248-cf863cb9bcc4.png)

![unnamed-14](https://user-images.githubusercontent.com/22335838/30131908-4f94bd7a-9303-11e7-893b-433847d2115d.png)

![unnamed-15](https://user-images.githubusercontent.com/22335838/30131907-4f8efebc-9303-11e7-8ca6-55e8453aefee.png)

**Figures 6-10**. Multidrug ressitant protein data

Unfortunately, there were a few peptides that increased in abundance as the amount of oyster protein added decreased. This could mean a few things. Most likely, the assay was not correctly designed for these proteins. However, there's also the possibility that there is a geoduck protein that is very similar to my oyster protein confounding the results. Since the total concentration of protein added was the same for all dilutions, a combination of oyster and geoduck protein detected should lead to the same peak area across dilutions. Therefore, I don't think it was this second scenario, and that the assay was incorrectly designed.

![unnamed-16](https://user-images.githubusercontent.com/22335838/30131999-b728bc98-9303-11e7-96c0-6f164da02236.png)

![unnamed-17](https://user-images.githubusercontent.com/22335838/30131998-b725411c-9303-11e7-99a9-a74c7cb5668d.png)

![unnamed-18](https://user-images.githubusercontent.com/22335838/30132001-b737cd1e-9303-11e7-825c-b1e5f77be7d8.png)

**Figures 11-13**. Peptides that increase in concentration when they should be more dilute. I originally deleted them from my dilution curve document, but undid the deletion to save the dilution information.

I was iffy about the multidrug resistant protein, but decided to keep it since peptide abundances decreased as I added less oyster protein.

![unnamed-19](https://user-images.githubusercontent.com/22335838/30132391-0a9cd99e-9305-11e7-9f1e-98a37036e520.png)

**Figure 14**. Dilution curve data for multidrug resistant protein, showing a general trend of decreasing peptide abundance.

I [duplicated my Skyline document](http://owl.fish.washington.edu/spartina/DNR_SRM_20170728/Analyses/2017-09-06-Gigas-SRM-ReplicatesOnly-PostDilutionCurve.sky.zip) and deleted these peptides from my data before proceeding with my analysis. There were two peptides in proteins with three peptides that had increasing concentrations, so I think I can keep the proteins. Both peptides from HSP74 increased in concentration, so I deleted the protein entirely. 

![unnamed-20](https://user-images.githubusercontent.com/22335838/30132297-bd9defa2-9304-11e7-83b1-b07e53da0941.png)

![unnamed-21](https://user-images.githubusercontent.com/22335838/30132296-bd9d07e0-9304-11e7-9c8e-da0f49b8dcff.png)

![unnamed-22](https://user-images.githubusercontent.com/22335838/30132298-bda13cb6-9304-11e7-8a65-76e9e354a898.png)

**Figures 15-17**. Peptides deleted from Skyline document with SRM replicate data.

Finally, I re-exported my data. This time, I added total ion current as well. I have two versions: one with [pivoting the replicate](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-06-Gigas-SRM-ReplicatesOnly-PostDilutionCurve-Report.csv), and one [without](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-09-06-Gigas-SRM-ReplicatesOnly-PostDilutionCurve-NoPivot-Report.csv).

![unnamed-23](https://user-images.githubusercontent.com/22335838/30132382-03fcffce-9305-11e7-8bf2-55f1d97b5c0b.png)

![unnamed-24](https://user-images.githubusercontent.com/22335838/30132381-03f87a94-9305-11e7-8dfa-812e738abbd0.png)

**Figures 18-19**. Export settings for corrected data.

In hindsight, I probably should have done this before my [initial data cleaning](), but oh well.

Tomorrow, Laura and I are meeting with Emma to go over our PRTC abundances. When I emailed her about my [loose batch effects and sloppy peptides](https://yaaminiv.github.io/SRM-Analysis/), she suggested the following:

```
I think the best option at this point is to use PRTC peptides to predict RT and normalize data by TIC. I still think you and Laura should make sure your assay peptides are not similarly affected to rule out differences in abundance due to the mass spec. 
Definitely choose the peaks that most closely match your predicted RT. 
```

Hopefully tomorrow I have a solution so I can move forward!

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

