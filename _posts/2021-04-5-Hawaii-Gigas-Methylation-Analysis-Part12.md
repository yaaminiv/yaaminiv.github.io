---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 12
tags: hawaii gigas-ploidy samtools SNP
---

## Identifying SNPs

Now that [`samtools` is installed and I merged BAM files](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part11/), I'm ready to identify SNPs with `BS-Snper`! I'm going to identify SNPs from the merged BAM file and from individual BAM files. Steven suggested that once I have SNP vcf files, I could use `bedtools intersect` to figure out where the SNPs overlap with the CG track and DML I'll identify later, so I won't need to filter SNPs once I identify them.

### Merged SNPs

First, I decided to identify SNPs from the [merged BAM file](https://gannet.fish.washington.edu/spartina/project-oyster-oa/Haws/BS-Snper/merged-sorted-deduplicated.bam) I created with `samtools merge`. Based on [Mac's suggestion](https://github.com/RobertsLab/resources/discussions/1151), I ran `BS-Snper` with the default parameters. However, I set `--mincov 5` instead of using the default coverage of 10 to be consistent with `bismark` and `methylKit` parameters.

```
#Defaults: --minhetfreq 0.1 --minhomfreq 0.85 --minquali 15 --maxcover 1000 --minread2 2 --errorate 0.02 --mapvalue 20
#Saving output to gannet
!perl /Users/Shared/Apps/BS-Snper-master/BS-Snper.pl \
--fa /Volumes/web/spartina/project-oyster-oa/data/Cg-roslin/cgigas_uk_roslin_v1_genomic-mito.fa \
--input /Volumes/web/spartina/project-oyster-oa/Haws/BS-Snper/merged-sorted-deduplicated.bam \
--output /Volumes/web/spartina/project-oyster-oa/Haws/BS-Snper/SNP-candidates.txt \
--methcg /Volumes/web/spartina/project-oyster-oa/Haws/BS-Snper/CpG-meth-info.tab \
--methchg /Volumes/web/spartina/project-oyster-oa/Haws/BS-Snper/CHG-meth-info.tab \
--methchh /Volumes/web/spartina/project-oyster-oa/Haws/BS-Snper/CHH-meth-info.tab \
--mincover 5 \
> /Volumes/web/spartina/project-oyster-oa/Haws/BS-Snper/SNP-results.vcf 2> merged.ERR.log
```

I wrote the output to [this `gannet` folder](https://gannet.fish.washington.edu/spartina/project-oyster-oa/Haws/BS-Snper/) instead of trying to deal with large files my Github repository.

### Individual SNPs

Since I ran the script successfully, I moved onto SNP identification from individual files! I modified a loop from Mac to identify SNPs in my files. First, I changed my working directory to the one with BAM files. Within the same code chunk, I needed to create variables for the file names, then use the modified file names within the BS-Snper loop:

```
%%bash

FILES=$(ls *deduplicated.sorted.bam)
echo ${FILES}

for file in ${FILES}
do
    NAME=$(echo ${file} | awk -F "." '{print $1}')
    echo ${NAME}

    perl /Users/Shared/Apps/BS-Snper-master/BS-Snper.pl \
    --fa /Volumes/web/spartina/project-oyster-oa/data/Cg-roslin/cgigas_uk_roslin_v1_genomic-mito.fa \
    --input ${NAME}.deduplicated.sorted.bam \
    --output ${NAME}.SNP-candidates.txt \
    --methcg ${NAME}.CpG-meth-info.tab \
    --methchg ${NAME}.CHG-meth-info.tab \
    --methchh ${NAME}.CHH-meth-info.tab \
    --mincover 5 \
    > ${NAME}.SNP-results.vcf 2> ${NAME}.ERR.log

done
```

Initially I specified `--fa` with a FASTA variable, but I kept running into an error! I then remembered that within Jupyter notebooks I had issues specifying variables within loops, so I used the absolute file path instead. Once I had all the files, I moved them to the `BS-Snper` directory on `gannet`.

```
# Move files to BS-Snper directory
!mv *SNP-candidates.txt *meth-info.tab *SNP-results.vcf *ERR.log ../BS-Snper/
```

Katherine suggested I merge with [`vcf-merge`](https://vcftools.github.io/perl_module.html#vcf-merge), but I don't think I need to do that if I could run a loop with `bedtools intersect` to determine if any individual SNPs overlap with DML. Now that I identified SNPs for this dataset, I can do something similar with the Manchester data! I'm also going to try [EpiDiverse/snp](https://github.com/EpiDiverse/snp) since it would  give me a relatedness matrix in addition to SNP variants.

### Going forward

1. Try [EpiDiverse/snp](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
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
