---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 10
tags: hawaii gigas-ploidy bismark SNP
---

## Processing Roslin alignments

### [`bismark`](https://github.com/FelixKrueger/Bismark) output

After transferring the `bismark` output off of `mox` and into [this `gannet` folder](https://gannet.fish.washington.edu/spartina/project-oyster-oa/Haws/bismark-2/) and [this Github repo folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_03-bismark-roslin), I generated a [`multiqc`](https://multiqc.info/) report. I then compared the [new report](http://htmlpreview.github.io/?https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_03-bismark-roslin/multiqc_report.html) with the [old report](http://htmlpreview.github.io/?https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_02-bismark/multiqc_report.html).

![Screen Shot 2021-03-16 at 10 36 12 AM](https://user-images.githubusercontent.com/22335838/111368695-6af85800-8653-11eb-9086-9acc068b997c.png)

**Figure 1**. Alignment stats from both genomes (Roslin = left, old = right)

The alignment percentages are a little lower for the Roslin genome, but generally around ~60% of reads from each sample were mapped. The Roslin genome does have lower percentages of duplicated reads, which gives me more confidence in the alignments. I perused through the rest of the `multiqc` report and didn't see anything else noteworthy. I'll move forward with the Roslin genome alignments for the rest of the analysis.

### Testing SNP identification

Now that I have new alignments, I can test out SNP variant calling workflows! The two we investigated in an earlier Science Hour are [BS-Snper](https://github.com/hellbelly/BS-Snper) and [EpiDiverse/snp](https://github.com/EpiDiverse/snp). I first read the [BS-Snper](https://academic.oup.com/bioinformatics/article/31/24/4006/198124) and [EpiDiverse/snp](https://www.biorxiv.org/content/10.1101/2021.01.11.425926v1) papers, and promptly realized I had no idea what anything meant. So that's fun. From the papers, I learned that BS-Snper was validated against RRBS data, while the EpiDiverse/snp workflow was tested against WGBS data. The validation method differences could impact the quality of the SNPs I call.

Now digging into workflow specifics. BS-Snper only calls variants, while EpiDiverse/snp can also cluster samples using k-mer similarity. I definitely need variant information: I want to remove SNPs from the CpG loci considered as treatment-related DML, and determine if there are any methylation patterns that could be associated with SNPs. I'm not entirely if genetic similarity information provided by k-mer clustering would be groundbreaking, but having the information wouldn't hurt. Although EpiDiverse/snp would provide more information, it seems much more complicated to install. I think I'll still try both approaches and see what happens.

### BS-Snper

BS-Snper seemed the easiest to figure out, so I started there. I created [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/05-BS-SNPer.ipynb) to install and run BS-Snper. The installation went smoothly, so I proceeded to run the perl script. There were a lot of parameters that didn't make sense to me, so I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1151) to get some insight into what options I should set. While waiting for feedback, I ran the following code with a minimum coverage setting that matched my downstream methylation analyses:

```
!perl /Users/Shared/Apps/BS-Snper-master/BS-Snper.pl \
--fa /Volumes/web/spartina/project-oyster-oa/data/Cg-roslin/cgigas_uk_roslin_v1_genomic-mito.fa \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/*sorted.bam \
--output SNP-candidates.txt \
--methcg CpG-meth-info.tab \
--methchg CHG-meth-info.tab \
--methchh CHH-meth-info.tab \
--mincover 5 \
> SNP-results.vcf 2> ERR.log
```

Looking at the error log, I realized it was only calling variants for the first file specified by the wildcard! I [added a comment](https://github.com/RobertsLab/resources/discussions/1151#discussioncomment-491150) in my discussion post to see if I needed to call SNPs separately for each file, or if there was a way to call SNPs for multiple files. I also posted [this issue in the BS-Snper repository](https://github.com/hellbelly/BS-Snper/issues/13) to see if the people that wrote the script had any insight. Since the documentation says that input files need absolute paths, I wondered if I could add two absolute file paths and have the script run on multiple files:

```
#Defaults: vQualMin = 15, nLayerMax = 1000, vSnpRate = 0.100000, vSnpPerBase = 0.020000, mapqThr = 20.
!perl /Users/Shared/Apps/BS-Snper-master/BS-Snper.pl \
--fa /Volumes/web/spartina/project-oyster-oa/data/Cg-roslin/cgigas_uk_roslin_v1_genomic-mito.fa \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_10_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_11_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
--output SNP-candidates.txt \
--methcg CpG-meth-info.tab \
--methchg CHG-meth-info.tab \
--methchh CHH-meth-info.tab \
--mincover 5 \
> SNP-results.vcf 2> ERR.log
```

Once again, it only called SNPs for the first file! I did notice that default a parameters were listed in the error log. I included them in the chunk comments, and looked into those defaults in [the perl script](https://github.com/hellbelly/BS-Snper/blob/master/BS-Snper.pl).

Sam suggested I use `samtools merge` to concatenate files and use that as the BS-Snper input, since the script is written for one input file. Referring to the [`samtools` documentation](http://www.htslib.org/doc/samtools-merge.html), I tried running the following code:

```
!/Users/Shared/Apps/samtools-1.8/samtools merge \
-ur \
-b /Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_1_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_2_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_3_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_4_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_5_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_6_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_7_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_8_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_9_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_10_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_11_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_12_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_13_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_14_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_15_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_16_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_17_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_18_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_19_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_20_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_21_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_22_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_23_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_24_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
merged-sorted-deduplicated.bam
```

but got an error about library versions:

```
dyld: Library not loaded: /usr/local/opt/xz/lib/liblzma.5.dylib
  Referenced from: /Users/Shared/Apps/samtools-1.8/samtools
  Reason: Incompatible library version: samtools requires version 8.0.0 or later, but liblzma.5.dylib provides version 6.0.0
```

Not sure how to resolve it, I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1153). While Sam did suggest updating [the library](http://www.htslib.org/download/), Mac responded to my original discussion stating that she called SNPs for samples individually:

> I ran samples one at a time and merged the C/T SNP files afterward (as a bed file). As a note, I was only using the BS-Snper data to filter potential C/T SNPs from my methylation data (if a locus was called a SNP in any sample, it was removed from further analysis)

I'll return to that suggestion next time I play around with BS-Snper!

### Going forward

1. Try [BS-Snper](https://github.com/hellbelly/BS-Snper) and [EpiDiverse/snp](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
1. Run `methylKit` script on `mox`
5. Investigate comparison mechanisms for samples with different ploidy in oysters and other taxa
4. Test-run [DSS](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#34_DMLDMR_detection_from_general_experimental_design) and [ramwas](https://bioconductor.org/packages/release/bioc/html/ramwas.html)
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)
6. Update methods
7. Update results

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
