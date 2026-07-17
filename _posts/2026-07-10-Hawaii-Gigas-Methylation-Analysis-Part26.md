---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 26
tags: hawaii gigas-ploidy methylKit
---

## Revising `methylKit` analysis, continued

Now that I've confirmed that I should not use the overdispersion correction for my samples, I wanted to do the following:

1. Identify pH DML!
2. Test various `min.per.group` parameters to optimize DML identification
3. Determine if outlier samples should be included in the analysis
4. View the DML I've identified in IGV

### Identify pH DML

When using CpG loci with 5x coverage in all samples, I identified 10 DML with 25% difference between treatments and only 2 DML 50% difference between treatments. For my ploidy-DML, I have 10 with 25% difference between treatments and only 1 with a 50% difference. All DML files can be found in [this folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_04-methylKit/DML). The fact that there aren't a lot of pH DML is kind of weird, since I would expect to see pH differences manifest in the gill tissue. But I have some ideas about that (see below).

### View identified DML

In the meantime, I figured I could at least look at the DML that were identified to see if I had any confidence in them. Since they were identified with the most stringent parameters, if I did not have confidence in the DML that would be a big issue. I opened my [existing DML IGV session](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_07-DML-characterization/dml.xml) after downloading the latest version of IGV. I removed my previous DML tracks since I had updated DML lists. I couldn't import CSV into the IGV session, so I quickly converted the files into BED files in [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/07-Genomic-Location-of-DML.ipynb). The BED files can be found [in this folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_07-DML-characterization).

<img width="2239" height="1131" alt="Image" src="https://github.com/user-attachments/assets/88e1a659-831a-41ab-8f47-8bcb4a44d0b2" />

**Figure 1**. Screenshot of IGV session.

Then I remembered how annoying it was to mess around with something in IGV. As a reminder for me, samples 1-6 were diploid x high, samples 7-12 were diploid x low, samples 13-18 were triploid x high, and samples 19-24 were triploid x low. I started to look at one locus that had a ploidy-DML (hypomethylated in triploids) and pH-DML (hypermethylated in low pH).

<img width="1281" height="1034" alt="Image" src="https://github.com/user-attachments/assets/a4edd46c-0154-480d-83b0-3065eef841db" />
<img width="1290" height="1046" alt="Image" src="https://github.com/user-attachments/assets/d00caf12-fdd0-4544-ab23-6b51fefadd35" />

**Figure 2**. DML for pH and ploidy.

I do see the hypomethylation in triploids because the bottom 12 samples have lower methylation than the top 12 samples. However, this seems to be driven more by a handful of samples with much visibly lower levels of methylation. The pH DML makes way less sense to me because if I look at it, the levels of methylation look relatively equal between high and low pH samples.

I decided to look at another ploidy-DML to see if that one made sense to me. Since I only had one with a 50% difference, this one had a 25% difference between ploidy conditions.

<img width="2240" height="1071" alt="Image" src="https://github.com/user-attachments/assets/6c518199-15ba-49fa-8ac3-a5dce4380d42" />
<img width="2238" height="1086" alt="Image" src="https://github.com/user-attachments/assets/96f0f58d-965b-4de5-b1ff-11d729df7c86" />

**Figure 3**. ploidy-DML example

Once again, this ploidy-DML makes more sense to me. I see very clear hypomethylation in triploids compared to diploids. I looked other hypomethylated and hypermethylated pH-DML just to ensure that I actually believed what I was seeing.

<img width="1316" height="1077" alt="Image" src="https://github.com/user-attachments/assets/77f77531-006b-417e-900f-e89ddee8cb62" />
<img width="1319" height="1078" alt="Image" src="https://github.com/user-attachments/assets/e4d1684e-8b12-43b0-8ac9-e6abe2c44ba2" />

**Figure 4**. pH-DML example

Okay, this one I believe! There is a clear difference in methylation levels between high and low pH samples. I checked another hypomethylation in low pH DML and that one also looked okay. So I think that the DML currently do (for the most part) inspire confidence, even if there aren't very many of them.

### Test `min.per.group` parameters

One way to optimize DML identification is to determine if there are any believable gains in DML identification when changing the `min.per.group` required. Right now, I need a locus to have 5x coverage in all samples to identify DML. Changing `min.per.group` would allow for less samples to have 5x coverage at a specific locus to call a DML. I wanted to test using 8, 9, 10, and 11 for `min.per.group` for ploidy and pH, then compare the number of DML identified using 25% and 50% thresholds. This is very computationally intensive, and given how long it took me to even identify DML for ploidy and pH, Steven and Sam suggested running this analysis on `klone`. I put together [this R Markdown document](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit-parameter-testing.Rmd) to import data, modify `min.per.group` with `unite`, and identify DML.

### Determine if outlier samples should be included in the analysis

Here's the idea I teased above. I think I have a couple of putative outlier samples. When I look at the [full sample PCA](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/general-stats/Full-Sample-Methylation-PCA-FilteredCov5Destrand.jpeg) samples 3H-2 (sample 14) and 2H-3 (sample 3) are very far away from the other cluster of other samples. Samples 2H-1 (sample 1) and 3H-3 (sample 15) are also far from the cluster but may not be potential outliers as much as the other two. Having outliers could greatly influence DML calling ability. Looks like [past Yaamini noticed these outliers but didn't actually think about removing them, but instead tried `min.per.group`](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part7/). Well present Yaamini is slightly more knowledgeable (maybe??) and figured investigating these samples is a good idea.

I decided to look at the [trimming](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_01-trimgalore/trimmed-data-2/poly-G/multiqc_report_1.html) and [alignment information](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_03-bismark-roslin/multiqc_report.html) to see if there was a technical reason to remove any of the samples. Looking at the trimming information, samples 3 and 14 only have 36-39 M sequences, which is ~10 M sequences less than the rest of the samples. So that's one good potential reason to count these as outliers. Alignment information wasn't particularly different.

In the same [this R Markdown document](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit-parameter-testing.Rmd), I included code to test removing these samples prior to repeating the clustering, PCA, and DML identification steps.

Let's see what happens!

### Going forward

1. Continue parameter testing for DML identification
2. Revise `methylKit` methods and results
4. `methylKIt` randomization test
1. ATAC-Seq data integration
2. `KOG-MWU` for *Crassostrea* methylation comparison
5. Revise discussion
7. Revise introduction
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)

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
