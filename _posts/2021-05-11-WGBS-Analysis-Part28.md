---
layout: post
comments: true
title: WGBS Analysis Part 28
tags: manchester gigas-broodstock bedtools BS-Snper
---

## Working with `BS-Snper` SNPs

I'm returning to the [`BS-Snper` analysis](https://yaaminiv.github.io/WGBS-Analysis-Part17/) so I can characterize overlaps with SNPs and DML! This will help me understand if the differential methylation is solely attributable to treatment differences, or if there is also a genetic component.

### Understanding VCF output

First, I wanted to understand (roughly) the [VCF output](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/BS-Snper/) I got from `BS-Snper`. I found [this link](https://gatk.broadinstitute.org/hc/en-us/articles/360035531692-VCF-Variant-Call-Format) that had good explanations of the header and columns. The first ~100 lines of the file are part of the VCF header, and I ignored it for the most part. Once I got to the column header information and actual output, I appreciated the clear information from the GATK website. Based on my workflow, I needed a way to filter SNPs with a C reference allele and T alternative allele.

<img width="1331" alt="Screen Shot 2021-05-15 at 8 12 06 PM" src="https://user-images.githubusercontent.com/22335838/118384332-196c3b00-b5ba-11eb-81cd-e9797bbfee80.png">

### Filtering C/T SNPs

Steven suggested a few weeks ago that I could use `bedtools intersect` to look at overlaps between SNPs and CG sites or DML. I opened [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/07-BS-SNPer.ipynb) to first filter C/T SNPs. I thought knowing how many C/T SNPs were present in my data would be helpful.

I knew I could use VCF files with `intersectBed`, and at first I tried intersecting the VCF with the CpG methylation output. However, `intersectBed` wouldn't accept the format of the CpG tab-delimited output, since it only had one column with base pair position information. Instead, I decided to use the CG motif track I generated. Before writing out the file, I looked at the `head` and saw there were G/A SNPs that intersected with my CG motif track! This makes sense, because the CG motif track has positions for the Cs **AND** Gs that make up the CpG dinucleotide. Based on [Mac's comment on my previous discussion](https://github.com/RobertsLab/resources/discussions/1151#discussioncomment-494935), she was looking at C/T SNPs only. I then used `grep` to filter the standard input so the reference and alternative allele columns were C and T, respectively:

```
#Intersect VCF with SNP locations and CG motif track
#BEDtools output has C/T and non-C/T SNPs
#Use grep to isolate C/T SNPs (tab between C and T required)

!{bedtoolsDirectory}intersectBed \
-u \
-a /Volumes/web/spartina/project-gigas-oa-meth/output/BS-Snper/SNP-results.vcf \
-b ../../genome-feature-files/cgigas_uk_roslin_v1_fuzznuc_CGmotif.gff \
| grep "C T" \
> /Volumes/web/spartina/project-gigas-oa-meth/output/BS-Snper/CT-SNPs.tab
```

My filtered SNP file can be found [here]()! I proceeded to do something similar with SNPs from individual samples:

```
%%bash

for f in zr3616*vcf
do
    /Users/Shared/bioinformatics/bedtools2/bin/intersectBed \
    -u \
    -a ${f} \
    -b /Users/yaamini/Documents/project-gigas-oa-meth/genome-feature-files/cgigas_uk_roslin_v1_fuzznuc_CGmotif.gff \
    | grep "C	T" \
    > ${f}_CT-SNPs.tab
    head ${f}_CT-SNPs.tab
    wc -l ${f}_CT-SNPs.tab
done
```

At this point, I ended up with some really ugly file names (ex. `zr3616_1_R1_val_1_val_1_val_1_bismark_bt2_pe.SNP-results.vcf_CT-SNPs.tab
`). I couldn't figure out how to get `xargs | basename` to work within a loop, so I looked for another way to rename files en masse. [Stack Overflow reminded me](https://stackoverflow.com/questions/12174947/removing-part-of-a-filename-for-multiple-files-on-linux) that `mv` existed beyond moving files from one place to another! The syntax looked a lot like `sed`, but it allowed me to remove the same pattern from all my filenames:

```
%%bash

for f in zr3616*CT-SNPs.tab
do
    [ -f ${f} ] || continue
    mv "${f}" "${f//_R1_val_1_val_1_val_1_bismark_bt2_pe.SNP-results.vcf/}"

done
```

The merged BAM file contained ~5 million SNPs, and ~120,000 of those were C/T SNPs (Table 1). Looking at individual files, there were ~3 million overall SNPs for each sample, with ~65,000-70,000 C/T SNPs. That means about 2% of SNPs in any BAM file were C/T SNPs. I meant to look at the C/T SNPs in IGV, but after I added my BAM files and was adding SNP files, the application crashed. I'll try again later.

### Overlaps with DML

I used `intersectBed` to look at overlaps between merged SNPs or individual SNPs, and either female-, indeterminate- or common DML in [this notebook](github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.ipynb).

**Table 1**. Number of overall SNPs, C/T SNPs, and SNPs overlapping with various DML categories.

| **BAM** | **All SNPs** | **C/T SNPs** | **Female-DML** | **Indeterminate-DML** | **Common DML** |
|:-------:|:------------:|:------------:|:--------------:|:---------------------:|:--------------:|
|  Merged |   5,519,308  |    122,306   |       38       |          298          |        0       |
|    1    |   3,083,382  |    69,845    |       18       |          130          |        0       |
|    2    |   3,105,080  |    70,244    |       17       |          201          |        0       |
|    3    |   3,202,988  |    72,064    |       15       |          145          |        0       |
|    4    |   3,232,583  |    73,664    |       17       |          138          |        0       |
|    5    |   3,007,518  |    66,965    |       18       |          155          |        0       |
|    6    |   3,204,395  |    72,123    |       19       |          138          |        0       |
|    7    |   3,083,706  |    68,856    |       21       |          128          |        0       |
|    8    |   3,100,801  |    69,695    |       23       |          138          |        0       |

For the merged BAM file, ~12% of female-DML are SNPs, while ~4% of the indeterminate-DML are! I'm not really sure what this means, and how many of the individual DML overlap with eachother or the merged file. I think I'll need to mess around with the SNP-DML overlap lists in R to understand how many unique locations are SNPs, and create some sort of visualization. 

### Going forward

1. Finish running `methylKit` randomization tests on R Studio server
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
