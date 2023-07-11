---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 22
tags: hawaii gigas-ploidy methylKit
---

## The final countdown (for the Hawaii paper)

Two years and ten days later...I'm ready to finish off this paper. In the meantime, I've discussed the paper with Steven a few times and decided on the following:

- We gotta go back to `methylKit` results. While [the `DSS` methods](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part18/) seemed useful and are what I used for my dissertation, just spot-checking what the algorithm identified as DML did not make sense. I also can't set a methylation threshold for DML with `DSS`, which makes it trickier to interpret or compare with other studies.
- I had [Sam run `EpiDiverse/snp`](https://github.com/RobertsLab/resources/issues/1558) with the Hawaii data, thinking that I could compare C->T SNP identification between methods. However, the `EpiDiverse/snp` output doesn't provide a list of C->T SNPs. Sam and Steven are trying to troubleshoot this with the CEABIGR data, but for now I'm going to stick to BS-Snper output for SNP identification. The `EpiDiverse/snp` output information, however, could give us genotypic information that we could incorporate later on, since Maria was unable to tell us if the diploid female and tetraploid male oysters used were from related lines, or how many half- or full-sibling families were used for the triploid oysters I eventually got.
- A few methodological novelties I want to try with this dataset include a randomization test with `methylKit`, integration with *C. gigas* ATAC-Seq, csRNA-Seq, and 5'-GRO data, and `KOG-MWU` for DML comparison with other *Crassostrea spp.* epigenetic studies.
- Figure out if we can get the pH and water quality data

### Original `methylKit` results

First things first, I wanted to find my original `methylKit` results. Thankfully, I didn't delete anything from the Github repository (and if I did, I could always go back to a different version). How people ever find anything without Github, an online lab notebook, and large file storage with web links is something I will never understand.

To remind myself of what I did previously, I went through the paper and my [lab notebook](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part14/). I used a 25% cutoff to identify DML, which is different than the 50% I normally use. I also used `min.per.group = 8L`, which means a loci needs to have suitable coverage in eight samples per treatment.

The `methylKit` output is [here](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_04-methylKit), and I saved my `.RData` [here](https://gannet.fish.washington.edu/spartina/project-oyster-oa/Haws/methylKit/). Turns out I never made figures with the `methylKit` version of the results, but I did find [this lab notebook entry](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part16/) with the number of `methylKit` DML in each genome feature. My [Jupyter notebook examining DML genomic location](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/07-Genomic-Location-of-DML.ipynb) still has code that uses `methylKit` output. The numbers were consistent between the lab notebook and Jupyter notebook. However, I noticed that C->T SNPs were not removed prior to getting the count information! I removed the SNPs, then got updated counts. I needed to just modify a few lines of `sed` code at the end of the Jupyter notebook to get a table with the number of `methylKit` DML in each genome feature for contingency tests.

**Table 1**. Number of DML in each genome feature.

|   **Genome Feature**  | **pH DML (%)** | **Ploidy DML (%)** | **Common DML (%)** |
|:---------------------:|:--------------:|:------------------:|:------------------:|
|       Total DML       |       34       |         24         |          1         |
|  Hypermethylated DML  |       24       |      8 (33.3%)     |          0         |
|   Hypomethylated DML  |       10       |     16 (66.6%)     |          1         |
|         Genes         |       28       |         20         |          1         |
|        Exon UTR       |        5       |          0         |          0         |
|          CDS          |        3       |          5         |          1         |
|        Introns        |       20       |         15         |          0         |
|    Upstream flanks    |        0       |          0         |          0         |
|   Downstream flanks   |        4       |          1         |          0         |
|   Intergenic regions  |        2       |          3         |          0         |
|         lncRNA        |        3       |          0         |          0         |
| Transposable elements |       15       |          6         |          0         |


- Create `methylKit` genome location counts
- Revise code to use `methylKit` output instead of DSS output
- Change color scheme

### Randomization test

### Going forward

1. Contingency tests for `methylKit` genome location
2. Revise code to use `methylKit` output instead of DSS output
3. Change color scheme for figures
4. `methylKIt` randomization test
5. Add Rajan's comments to the Google Doc
1. Update methods
7. Update results
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
