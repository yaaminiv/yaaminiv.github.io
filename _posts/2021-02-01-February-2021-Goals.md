---
layout: post
comments: true
title: February 2021 Goals
tags: goals
---

![meow-meow](https://user-images.githubusercontent.com/22335838/106535201-8562e180-64aa-11eb-8861-d85f9c085564.jpg)

With the amount of work I need to get done this short month, I'm definitely on the meow meow train. But I think at the end of this month, I'll have a much clearer understanding of how much work is left for me before I'm solely in an analysis and writing phase, and when may be a reasonable time to defend!

### [January Goals Recap](https://yaaminiv.github.io/January-2021-Goals/)

**Hawaii Gigas Methylation**:

- [Trimmed data!](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis/) Also dealt with [overrepresented poly-G sequences in my samples](https://github.com/RobertsLab/resources/issues/1065) by [trimming three times total](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part4/).
- [Ran trimmed data through pipeline from the coral paper through `bismark`!](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part4/) Forgot that this takes a while so I wasn't able to complete any other steps.
- Investigated [DSS](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#34_DMLDMR_detection_from_general_experimental_design) as a potential package for identifying differentially methylated loci with multiple experimental conditions

**Gigas Gonad Methylation**:

- I didn't get any of my data until last week, so I haven't touched it yet!

**Virginica Labwork**:

- Obtained WGBS and RNA-Seq quotes from Zymo
- Started getting PO numbers for each quote
- Was not able to send samples for sequencing since I don't have both PO numbers
- Did not determine if there are additional samples that should be extracted sequenced

My plate was pretty full with bioinformatic analyses and quarantining after getting back from California, so I didn't make any progress on **ATAC-Seq Labwork** or **Gigas Labwork**!

### February goals

**Hawaii Gigas Methylation**:

- Evaluate [`bismark`](https://github.com/FelixKrueger/Bismark) output using coral methylation workflow
- Complete preliminary assessment of DML with `methylKit`
- Try identifying DML with `DSS`
- Compare `methylKit` and `DSS` DML output and determine which approach is suitable
- Determine genomic location of DML
- Identify significantly enriched GOterms associated with DML
- Identify methylation islands and non-methylated regions
- Locate an ATAC-Seq dataset that could be used to practice integrating chromatin information with methylation
- Start drafting manuscript

**Gigas Gonad Methylation**:

- Trim samples
- Align samples with [`bismark`](https://github.com/FelixKrueger/Bismark) and evaluate output
- Identify DML using either [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html) or `DSS`
- Determine genomic location of DML
- Identify significantly enriched GOterms associated with DML
- Identify methylation islands and non-methylated regions
- Decide if it's worth extracting gonad RNA for integrated RNA-Seq and methylation analyses
- Start drafting manuscript

**Virginica Labwork**:

- Send samples for sequencing!

**ATAC-Seq Labwork**:

- Purchase reagents and identify samples to test cell dissociation protocols
- Ensure protocol is easy to follow and is accessible for the lab
- Dissociate and cryopreserve some cells

**Other**:

- Continue work on ocean acidification and reproduction review in order to submit the manuscript this quarter
- Complete review for Molecular Ecology
- Watch SICB talks and prepare for live discussion session

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
