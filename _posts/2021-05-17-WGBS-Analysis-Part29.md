---
layout: post
comments: true
title: WGBS Analysis Part 29
tags: manchester gigas-broodstock mox methylKit
---

## Running randomization tests on `mox`

I initially tried running the randomization tests on the R Studio server, but encountered [several timeouts](https://yaaminiv.github.io/WGBS-Analysis-Part24/). I decided to run an R script through a SLURM script, like [I've tried previously](https://yaaminiv.github.io/WGBS-Analysis-Part22/).

### Editing code

First, I removed my randomization test code from [my DML identification script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.R), and copied my code into a [new randomization test script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit-randtest.R). I trimmed down the randomization test code to only include the 75% difference for female samples and 99% difference for indeterminate samples in `calculateDiffMeth`. I also pointed to the revised script in [this SLURM script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit-SLURM.sh), and modified my output text file name so I could easily find it.

Then I ran the SLURM script! Of course I ran into package installation issues. When loading `methylKit`, R could not find the `GenomicRanges` package. Thinking this had something do to with the `~/.Renviron` file I modified for the R Studio server, I set a new path for my R packges: `/gscratch/srlab/rpackages`. I realized soon after that this wouldn't solve the problem, since `methylKit` and all its associated packages are nested in my user directory: `/gscratch/home/yaaminiv/R/x86_64-pc-linux-gnu-library/3.6/`! In a build node, I went down the rabbit hole of packages that needed to be explicitly loaded, and ended up with the following list to get `methylKit` loaded successfully:

```
require(BiocGenerics, lib.loc = "/gscratch/home/yaaminiv/R/x86_64-pc-linux-gnu-library/3.6/")
require(S4Vectors, lib.loc = "/gscratch/home/yaaminiv/R/x86_64-pc-linux-gnu-library/3.6/")
require(IRanges, lib.loc = "/gscratch/home/yaaminiv/R/x86_64-pc-linux-gnu-library/3.6/")
require(GenomeInfoDb, lib.loc = "/gscratch/home/yaaminiv/R/x86_64-pc-linux-gnu-library/3.6/")
require(GenomicRanges, lib.loc = "/gscratch/home/yaaminiv/R/x86_64-pc-linux-gnu-library/3.6/")
require(methylKit, lib.loc = "/gscratch/home/yaaminiv/R/x86_64-pc-linux-gnu-library/3.6/")
```

### Another `mox` error

Once I cleared this hurdle, I encountered another one. My randomization test was not running because the sample ID information was not formatted correctly. I thought this was strange becaus I ran this code on the R Studio server without running into this error. In any case, I looked at the output file and saw that the sample ID information was saved as factors, and not characters. After creating the `sampleMetadataFem` dataframe with information, I converted the factors to characters:

```
sampleMetadataFem <- data.frame("sampleID" = c("1", "3", "4",
                                               "6", "7", "8"),
                             "slide-label" = c("02-T1", "06-T1", "08-T2",
                                               "UK-02", "UK-04", "UK-06"),
                             "pHTreatment" = c(rep(1, times = 3),
                                               rep(0, times = 3))) #Create metadata table
sampleMetadataFem$sampleID <- as.character(sampleMetadataFem$sampleID) #Convert to character format
```

I did this for the indeterminate samples too, since I'd likely run into the same error! When I ran my revised script, it started cranking through. Then I encountered other errors I hadn't seen before:

![Screen Shot 2021-05-17 at 5 48 46 PM](https://user-images.githubusercontent.com/22335838/118573985-6aa23900-b738-11eb-9519-139184e4ed61.png)

I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1213) to get feedback, and started running the randomization test without the `mc.cores` argument to see if I could reproduce the error. Two hours after running the test without `mc.cores` in `calculateDiffMeth`, I still hadn't encountered that error. I wonder if these errors could have contributed to the R Studio server crashing when I used `mc.cores` previously. In any case, I'm not sure how I can run these tests now...

### Going forward

1. Run `methylKit` randomization test
2. Update `mox` handbook with R information
2. Obtain relatedness matrix and SNPs with [EpiDiverse/snp](https://github.com/EpiDiverse/snp)
5. Identify overlaps between SNP data and other epigenomic data
2. Write methods
3. Write results
6. Identify significantly enriched GOterms associated with DML
7. Identify methylation islands and non-methylated regions
3. Determine if larval DNA/RNA should be extracted

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
