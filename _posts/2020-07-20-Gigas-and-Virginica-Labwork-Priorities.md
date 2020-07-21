---
layout: post
comments: true
title: Gigas and Virginica Labwork Priorities
tags: virginica gigas-broodstock
---

## Plan of attack for the mountain of labwork

I'm heading into lab tomorrow (!!) so I took time to strategize. Previously, [I outlined the many samples I need to process](https://yaaminiv.github.io/Proposed-Gigas-and-Virginica-Labwork/). Thinking about low-hanging fruit and my time, I have five priorities:

1. MBD enrichment + follow-up library preparation
2. Gonad histology RNA extractions + follow-up library preparation
3. Trizol DNA and RNA extractions + follow-up MBD enrichment and library preparation
4. ATAC-Seq
5. Offspring trireagent extractions and follow-up library preparation

### MBD enrichment

I have two DNA sample sets that need MBD enrichment: [*C. virginica* sperm](https://yaaminiv.github.io/Sperm-Extractions-Part5/) and [*C. gigas* gonad](https://yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part9/). The *C. gigas* gonad samples have low DNA yields since they were from histology blocks, so I'm going to work on the *C. virginica* DNA first. I think we have one unopened MethylMiner kit, which should give me enough reactions for the *C. virginica* samples. I should order another few kits soon.

The first step in the MBD enrichment protocol is to shear DNA fragments to ~500 bp. [Last time I did this](https://yaaminiv.github.io/Virginica-MBDSeq/), Sam went with me to NOAA and Mac showed us how to use their sonicator. This clearly isn't possible now. I asked Sam what I should do, and he reminded me that we have the Diagenode Bioruptor 300 from the Seebs' lab. I read [his lab notebook post](https://robertslab.github.io/sams-notebook/2019/03/06/DNA-Shearing-and-Bioanalyzer-Lotterhos-C.virginica-Mantle-gDNA-from-2018114.html) to look into the settings I need. I'll need to pipet my DNA into 0.5 mL snap cap tubes and run 30 cycles (30 seconds on, 30 seconds off) to shear DNA.

I also noticed that I have a sample with a concentration < 25 ng/µL. I need to increase the sample's concentration! There is a speed vacuum but it's in the Friedman lab, so I probably won't be able to use it. Instead, I'll do an isopropanol precipitation. Shelly taught me how to do one when I was working with *C. gigas* gonad samples (see lab notebook posts [here](https://yaaminiv.github.io/WGBS-Samples/) and [here](https://yaaminiv.github.io/WGBS-Samples-Part2/)).

The last preparation step for today was calculating the amount of reagents needed for the MethylMiner kit. To keep everything for this project in one place, I renamed an [old, unused Github repository](https://github.com/RobertsLab/project-oyster-comparative-omics) meant for this project. I replicated calculations [from *C. virginica* gonad samples](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-01-23-MBDSeq-Labwork/2018-01-23-Virginica-MBDSeq-Labwork-Calculations.xlsx) in [this spreadsheet for sperm samples](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-MBDSeq-Labwork-Calculations.xlsx). Previously, I used a base input of 4 µg of DNA for each sample to keep my pipetting volumes consistent and reduce error. I can't use that same threshold for the sperm samples since I have smaller DNA yields. I could standardize and use 2 µg per sample, or just stick with using 4 µg and have more samples that I'll need to pipet separately. I also noticed that the one sample with a low concentration has < 1 µg of DNA. Since I have to use the full sample anyways, is it worth doing the ethanol precipitation to concentrate it?

I posed those questions in [this issue](https://github.com/RobertsLab/resources/issues/965), and I'll bring it up at lab meeting too. The bigger question is if we're still comfortable doing MBDBS. Looking at my original notebook posts, it seemed like we were strongly considering WGBS for these samples, but that may have been because we were getting a deal for WGBS sequencing that we used for the *C. gigas* exploratory samples. If we don't want to do MBDBS, I think I should easily be able to prepare libraries for these samples and the *C. gigas* samples. It'll be nice to confirm we're going with what I pitched during my generals.

### Going foward

1. Complete MBD enrichment (if necessary) and library preparation for extracted DNA samples
2. Order necessary materials for extractions and protocol tests
3. Test and complete RNA extractions with *C. gigas* tissue in histology blocks
4. Test ATAC-Seq protocol on adult tissue
5. Get additional *C. virginica* sperm samples for ATAC-Seq
6. Test and complete DNA and RNA extractions with Trizol
7. Extract DNA and RNA from *C. gigas* larvae

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
