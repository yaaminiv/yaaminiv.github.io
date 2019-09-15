---
layout: post
comments: true
title: WGBS Analysis Part 5
tags: manchester gigas-broodstock WGBS bismark mox methylKit IGV
---

## Finalizing the analysis pipeline

I'm still waiting on final `bismark` output, so I figured I'd finalize my analysis pipeline and start working on a draft talk. I should have my final files tomorrow, which will allow me to make any tweaks before my practice talk Monday!

### More `bismark` script issues

I woke up this morning and saw my Mox job completed! When I checked my file list, I saw that my low pH sample had no deduplicated or sorted file. Instead, the script created 2 different ambient samples. Frustrated with wildcards, I modified [this script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/scripts/2019-09-12-Bismark-Deduplication.sh) to spell out each file I wanted processed. I ran the script and peeked at the output — my low pH sample is being processed correctly! When I make my master script, I need to include the specific wildcard for one character (I think it's `?`) to ensure all my files will be processed and the output will be named properly.

### Checking start and end positions for IGV

[Yesterday](https://yaaminiv.github.io/WGBS-Analysis-Part4/) I noticed that my bedgraphs, DML background, and DML locations weren't aligning. I tried shifting the DML background up one position but that didn't line up either. Looking at [my *C. virginica* IGV screenshots](https://yaaminiv.github.io/DML-Analysis-Part34/) I noticed a similar visual discrepancy for bedgraphs vs. my other tracks. I decided to load the *C. gigas* CG motif track to compare my tracks to something that's already been tested.

![Screen Shot 2019-09-14 at 2 21 43 PM](https://user-images.githubusercontent.com/22335838/64913806-bf87e380-d6fb-11e9-9413-b60220ba3025.png)

**Figure 1**. CG motif track with other DML and DML background tracks.

They don't line up! I definitely need to move the loci start position down 2 and end position down 1. I was able to fix that just fine. However, I noticed that the root problem may be that the unite output has the start and end positions of the switched and potentially switched.

![Screen Shot 2019-09-14 at 2 39 52 PM](https://user-images.githubusercontent.com/22335838/64913909-b13ac700-d6fd-11e9-9c9a-06a35297c0b6.png)

**Figure 2**. `unite` output.

If I use `destrand = FALSE`, I don't get that problem.

![Screen Shot 2019-09-14 at 2 43 21 PM](https://user-images.githubusercontent.com/22335838/64913929-f6f78f80-d6fd-11e9-880c-1057f52312df.png)

**Figure 3**. `unite` output with `destrand = FALSE`.

I guess I have a lot more data on the reverse strand than I expected. I know I need to `destrand`, but perhaps I don't need to modify base pair positions for the DML background. There's also a chance that loading in finalized data from my Mox run will change some things. I'll stick to just getting my loci to look good and worry about the DML background after PCSGA.

### Changing `getMethylDiff` parameters

Since [I was getting over 15,000 DML](https://yaaminiv.github.io/WGBS-Analysis-Part4/), Steven suggested I change the stringency of `gethMethylDiff` parameters. This made sense because I am only working with two different samples, and I want to ensure my results are "real." With the 5x coverage data, I tried setting my methylation difference minimum to 75% and 99% in [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-MethylKit.Rmd). I used 99% since using 100% difference gave me an error, but still only returned loci that were 100% between samples. Using a 100% difference between samples, I still had ~2,000 DML. I then made my q-value stricter, going from 0.01 to 0.001. These settings gave me ~800 DML, which I felt was reasonable for WGBS. I used 100% difference and q-value < 0.001 with the 10x coverage data as well. This time, I had less DML with 10x coverage data! I links to the files I saved can be found below:

**5x coverage**:
- All DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-5x-Locations-100diff.bed); [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-100-Cov5.csv)
- Hypermethylated DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-5x-Locations-Hypermethylated-100diff.bed); [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-100-Cov5-Hypermethylated.csv)
- Hypomethylated DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-5x-Locations-Hypomethylated-100diff.bed); [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-100-Cov5-Hypomethylated.csv)

**10x coverage**:
- All DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-10x-Locations-100diff.bed); [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-100-Cov10.csv)
- Hypermethylated DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-10x-Locations-Hypermethylated-100diff.bed); [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-100-Cov10-Hypermethylated.csv)
- Hypomethylated DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-10x-Locations-Hypomethylated-100diff.bed); [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-100-Cov10-Hypomethylated.csv)

### Verifying DML in IGV

I was still unsure if there are some DML that are more artifacts of losing coverage, so I went back to IGV.

![Screen Shot 2019-09-14 at 3 15 59 PM](https://user-images.githubusercontent.com/22335838/64914173-856e1000-d702-11e9-871e-e626884724f5.png)

<img width="1149" alt="Screen Shot 2019-09-14 at 3 26 50 PM" src="https://user-images.githubusercontent.com/22335838/64914335-1691b680-d704-11e9-9694-ab7b27215f0b.png">

<img width="1152" alt="Screen Shot 2019-09-14 at 3 32 50 PM" src="https://user-images.githubusercontent.com/22335838/64914380-e1d22f00-d704-11e9-897f-136e6d0e9b35.png">

**Figures 4-6**. Example of 10x DML that does not exist in 5x data.

Looking at the DML above, it looks like it was not included for the 5x coverage data since there was missing data at 5x coverage, but the 10x coverage data was there for both samples (no second lump for 10x data, second lump there for 5x low pH data but not ambient pH data). The second locus, however, makes no sense to me. There loci isn't there in the 10x coverage data, so why is it a DML?! Then there's the third locus. Again, doesn't make sense to have a 10x 100% diff DML there, but it also doesn't align with the CG motif. This visualization is going to be difficult, but at least it overlaps with the CG motif so I can still use `bedtools`.

### Cleaning up DML

I want to remove the any DML in the 10x 100% difference data that are not in the 5x 100% difference data. I figured I could do this with `bedtools` by extracting the DML that are in both datasets. I don't think it makes sense to have a 10x DML that is not in the 5x data, so this allows me to remove any extra 10x DML. At the end of [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-MethylKit.Rmd) I used `intersectBed` to remove extraneous DML:

```{bash}
#Path to intersectBed
#Print line a information for overlaps
#File a: 10x 100% diff DML
#File b: 5x 100% diff DML
#Output filename
/Users/Shared/bioinformatics/bedtools2/bin/intersectBed \
-wa \
-a 2019-09-12-DML-Destrand-10x-Locations-100diff.bed \
-b 2019-09-12-DML-Destrand-5x-Locations-100diff.bed \
> 2019-09-12-DML-Destrand-10x-Locations-100diff-NoExtras.bed
```

<img width="1154" alt="Screen Shot 2019-09-14 at 4 45 02 PM" src="https://user-images.githubusercontent.com/22335838/64914860-f74c5680-d70e-11e9-8bb1-fb1caffe305b.png">

**Figure 7**. Looking at the new track in IGV

Succss! The code worked. The files can be found [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-10x-Locations-100diff-NoExtras.bed)

### Going forward

2. Get revised `bismark` output from Mox and run through analysis pipeline
3. Obtain *C. gigas* feature tracks
4. Characterize locations of DML
5. Conduct a gene enrichment and/or identify genes with DML and discern what it means

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
