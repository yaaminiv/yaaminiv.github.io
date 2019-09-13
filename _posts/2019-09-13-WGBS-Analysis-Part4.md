---
layout: post
comments: true
title: WGBS Analysis Part 4
tags: manchester gigas-broodstock WGBS bismark mox IGV bedtools
---

## Looking at preliminary DML

[I have DML!](https://yaaminiv.github.io/WGBS-Analysis-Part3/). To clarify, I have 15,386 DML when filtering my data for 5x coverage. That's more than twice as many DML as I got for my *C. virginica* data. Before I characterize the location of my DML in the *C. gigas* genome, I'm going to do two different things...

### Trying different coverage in `methylKit`

Since I used WGBS, there's a chance that I can use a more stringent coverage filter instead of 5x coverage. I returned to [my R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-MethylKit.Rmd) and reprocessed the data using 10x coverage. I somehow ended up with more DML using 10x coverage! The files I generated can be found below:

- All DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-10x-Locations.bed), [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-50-Cov10.csv)
- Hypermethylated DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-10x-Locations-Hypermethylated.bed), [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-50-Cov10-Hypermethylated.csv)
- Hypomethylated DML: [BEDfile](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-10x-Locations-Hypomethylated.bed), [csv](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-12-MethylKit/2019-09-12-Loci-Analysis/2019-09-12-Differentially-Methylated-Loci-Filtered-Destrand-50-Cov10-Hypomethylated.csv)

Looks like the reason why I have more DML when using 10x coverage is because I gained 300 hypermethylated and 600 hypomethylated DML. The increase in hypomethylated DML is interesting. My *C. virginica* data had a pretty even split of hyper- and hypomethylated DML, whereas with 5x coverage I have 700 more hypomethylated DML and 1,000 more hypomethylated DML with 10x coverage. I don't entirely know why this is happening so good thing I already planned on checking things out in IGV.

**Table 1**. Number of CpG loci in various categories

| **Coverage** | **DML Background** | **All DML** | **Hypermethylated DML** | **Hypomethylated DML** |
|:------------:|:------------------:|:-----------:|:-----------------------:|:----------------------:|
|       5      |     11,285,932     |    15,385   |          7,384          |          8,001         |
|      10      |      5,210,744     |    16,324   |          7,669          |          8,665         |

### Verifying DML in IGV

I need to confirm that my DML are real, and the best way to do that is in IGV. I realized my IGV veresion was outdated, so I downloaded version 2.6.3 from the website. Since I'm looking at different coverage metrics, I also wanted to generate some bedgraphs that matched the coverage settinsg I used. I created [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-13-Generating-Coverage-Tracks.ipynb) by copying my *C. virginica* equivalent and created 5x and 10x begraphs for each sample. After moving the contents of my repository to `gannet`, I used URLs to add the following tracks to a [new IGV session](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-13-IGV-Verification/2019-09-13-DML-Verification.xml):

- [*C. gigas* genome](https://gannet.fish.washington.edu/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-13-IGV-Verification/Crassostrea_gigas.oyster_v9.dna_sm.toplevel.fa)
- [YRVA 5x bedgraph](https://gannet.fish.washington.edu/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-13-IGV-Verification/YRVA_R1_001_bismark_bt2_pe.deduplicated.bismark.cov_5x.bedgraph)
- [YRVA 10x bedgraph](https://gannet.fish.washington.edu/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-13-IGV-Verification/YRVA_R1_001_bismark_bt2_pe.deduplicated.bismark.cov_10x.bedgraph)
- [YRVL 5x bedgraph](https://gannet.fish.washington.edu/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-13-IGV-Verification/YRVL_R1_001_bismark_bt2_pe.deduplicated.bismark.cov_5x.bedgraph)
- [YRVL 10x bedgraph](https://gannet.fish.washington.edu/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-13-IGV-Verification/YRVL_R1_001_bismark_bt2_pe.deduplicated.bismark.cov_5x.bedgraph)
- [DML background 5x coverage](https://gannet.fish.washington.edu/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-12-MethylKit/2019-09-12-Methylation-Information-Filtered-Destrand-Cov5.bed)
- [DML background 10x coverage](https://gannet.fish.washington.edu/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-12-MethylKit/2019-09-12-Methylation-Information-Filtered-Destrand-Cov10.bed)
- [DML 5x coverage](https://gannet.fish.washington.edu/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-5x-Locations.bed)
- [DML 10x coverage](https://gannet.fish.washington.edu/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-12-MethylKit/2019-09-12-DML-Destrand-10x-Locations.bed)

I actually struggled quite a bit to load all of the tracks into the IGV session. I made the mistake of adding the full bedgraphs in the IGV, but couldn't open the file to delete the tracks. When I tried editing the xml file itself, I could successfully add tracks but not remove them. I ended up deleting my session and starting a new one. Once I created a new session, I could do what I needed!

![Screen Shot 2019-09-13 at 3 33 13 PM](https://user-images.githubusercontent.com/22335838/64899197-23000b80-d63f-11e9-9380-9bd8844cea00.png)
![Screen Shot 2019-09-13 at 3 37 03 PM](https://user-images.githubusercontent.com/22335838/64899204-2e533700-d63f-11e9-8758-51c1c610ad2f.png)
![Screen Shot 2019-09-13 at 3 39 20 PM](https://user-images.githubusercontent.com/22335838/64899205-2eebcd80-d63f-11e9-8664-d2aec98ad606.png)
![Screen Shot 2019-09-13 at 3 41 54 PM](https://user-images.githubusercontent.com/22335838/64899206-2eebcd80-d63f-11e9-95a4-601228517a64.png)
![Screen Shot 2019-09-13 at 3 47 14 PM](https://user-images.githubusercontent.com/22335838/64899207-2eebcd80-d63f-11e9-8f2c-fbdc59901f4f.png)
![Screen Shot 2019-09-13 at 3 48 27 PM](https://user-images.githubusercontent.com/22335838/64899208-2eebcd80-d63f-11e9-99d1-46b47b850365.png)

**Figures 1-6**. DML on various chromosomes.

Looking at the DML, I can see that I didn't export my BEDfiles correctly. The DML background and DML themselves don't overlap neatly with CpGs (they're 1 bp off). I need to change how I modify the base pairs in `methylKit` and resave the tracks.

I also don't trust the 10x DML that were not in 5x DML. I don't know what's going on, but it's flaming hot garbage. The other DML in the 10x track that were common to the 5x track looked good, so I think I want to remove all 10x DML that are not common to the 5x track. I will figure out a way to do that.

### Quick `bismark` update

The alignments went well! My first file deduplicated successfully, but my second file was saved with the wrong filename. I realized my wildcard was not specific enough, so I created a modified script from deduplication onwards and saved it [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/scripts/2019-09-12-Bismark-Deduplication.sh). I moved the script to Mox and started the job. Once I get the files reprocssed I'll not only run through my analyses, but also create one master script so I don't have to piecemeal anymore.

### Going forward

2. Get revised `bismark` output from Mox and run through analysis pipeline
3. Fix DML background and DML BEDfiles in `methylKit`
4. Remove 10x DML that are not present in 5x DML track
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
