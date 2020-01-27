---
layout: post
comments: true
title: Gigas Labwork Plans
tags: gigas-broodstock larvae
---

## Gearing up for *C. gigas* labwork

Now that my *C. virginica* gonad methylation paper is in review (*crosses fingers*), I'm ready to tackle the next phase of sample preparation and sequencing. I already [processed pooled *C. gigas* gonad DNA samples for WGBS](https://yaaminiv.github.io/WGBS-Samples-Part2/) and [completed a preliminary analysis of that data](https://yaaminiv.github.io/WGBS-Analysis-Part8/). I still have a good amount of [gonad DNA that can be processed for bisulfite sequencing](https://github.com/RobertsLab/project-oyster-oa/blob/6cf11ef90159df249473e3c2ae4130a695b65bf4/data/Manchester/2018-10-2018-Broodstock-DNA-Extractions/2018-10-09-DNA-Extraction-Results.csv), remaining tissue on histology blocks that can be used for RNA-Seq, [other broodstock tissues from the same oysters](https://github.com/RobertsLab/project-oyster-oa/blob/6cf11ef90159df249473e3c2ae4130a695b65bf4/data/Manchester/2017-Adult-Gigas-Tissue-Sampling/20170408-GigasTissueSamplingInformation.xlsx) in the freezer, and even some larvae. After chatting with Steven, we outlined 2.5ish projects for me to work on:

### 1. Potential for intergenerational epigenetic inheritance in *C. gigas* larvae

After counting larvae 18 hours post-fertilization and stocking larval rearing buckets at the appropriate density, I had some larvae left over from each family:

- ambient pH females (pool) x ambient pH males
- ambient pH females (pool) x low pH males
- low pH females (pool) x ambient pH males
- low pH females (pool) x low pH males

With only four samples total I don't have a very robust experiment, but it should be very easy for me to extract DNA for MBD-Seq and RNA and compare patterns in larvae with the adult gonad samples.

### 2. Examination of various epigenetic and genetic mechanisms in response to ocean acidification

I'm interested in seeing how epigenetic and genetic mechanisms play together to influence responses to ocean acidification! I have remaining gonad tissue in histology blocks and frozen adductor, ctenidia, and mantle tissues from *C. gigas* broodstock in low pH (10 individuals) and ambient pH (also 10 individuals). I have more than enough frozen tissue for DNA and RNA extractions, ATAC-Seq, and maybe even proteomics.

Since some gonad DNA has already been used for WGBS libraries, I will look into using [low volume MBD-BSSeq](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5739096/) for the remaining gonad DNA. I've already looked into low-input Qiagen kits (and I think it's discontinued?), but using a modified protocol of the MethylMiner kit that I've already used before and know works seems like a safer option.

I'll extract RNA from the remaining tissue in the histology blocks, which is something Laura did previously. I'll look at her protocol and try it out on a few samples. Extracting DNA and RNA from frozen somatic tissues shouldn't be any problem at all! There may be a kit I could use to extract both DNA for WGBS and RNA for RNA-Seq from the tissue, but I also have more than enough tissue to do separate extractions. I'll also save some tissue for proteomics if we decide to go down that route later. My goal is to prepare as many samples as I can and worry about sequencing decisions later.

Now the complicated part: ATAC-Seq. I'd like to replicate the analysis in the [Gatzmann et al. 2018 paper](https://epigeneticsandchromatin.biomedcentral.com/articles/10.1186/s13072-018-0229-6) to link gene expression with gene accesibility. It seems like every ATAC-Seq protocol requires single-cell suspensions, so [scATAC-Seq](https://www.nature.com/articles/nature14590) may be the only option available. During Science Hour, Shelly emailed the individual that developed scATAC-Seq and graduate student working on ATAC-Seq in frozen salmon tissue to see if they had any suggestions about doing ATAC-Seq with frozen mollusc tissue.

### 2.5 Ocean acidification impacts on fertilization with ATAC-Seq

While talking about ways to get single cells from frozen tissues or gonad histology blocks, Steven realized that Mac is fertilizing *C. giags* gametes and isolating single cells from embryos and larvae every other Wednesday for scRNA-Seq. If I were able to fertilize some gametes in low pH conditions and others in ambient pH conditions, I could isolate single cells from these larvae. Since the scRNA-Seq could be paid for on another grant, I could do complimentary scATAC-Seq. I will shadow Mac next Wednesday to see what she's doing and figure out if I could use similar techniques.

### Going forward

1. Shadow Mac for single cell experiments
2. Determine what kits to use and order necessary materials
2. Test RNA extraction protocol with tissue in histology blocks
3. Start processing frozen tissues
3. Extract DNA and RNA from larvae
4. Identify an ATAC-Seq protocol and start testing it
2. Figure out what to do with [*C. virginica* sperm](https://yaaminiv.github.io/Sperm-Extractions-Part5/) and other potential samples


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
