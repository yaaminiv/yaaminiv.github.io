---
layout: post
comments: true
title: WGBS Analysis Part 17
tags: manchester gigas-broodstock samtools SNP
---

## Identifying SNPs with `BS-Snper`

Now that [I know how to use `BS-Snper`](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part12/), I'm going to identify merged and individual SNPs in the Manchester data! I set up [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/07-BS-SNPer.ipynb) to start the process.

The first thing I needed to do was merge BAM files with `samtools merge`:

```
%%bash

samtools merge \
/Volumes/web/spartina/project-gigas-oa-meth/output/BS-Snper/merged-sorted-deduplicated.bam \
/Volumes/web/spartina/project-gigas-oa-meth/output/bismark-roslin/zr3616_1_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-gigas-oa-meth/output/bismark-roslin/zr3616_2_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-gigas-oa-meth/output/bismark-roslin/zr3616_3_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-gigas-oa-meth/output/bismark-roslin/zr3616_4_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-gigas-oa-meth/output/bismark-roslin/zr3616_5_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-gigas-oa-meth/output/bismark-roslin/zr3616_6_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-gigas-oa-meth/output/bismark-roslin/zr3616_7_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
/Volumes/web/spartina/project-gigas-oa-meth/output/bismark-roslin/zr3616_8_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam
```

Once I had the [merged BAM](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/BS-Snper/merged-sorted-deduplicated.bam), I looked at the header with `samtools view`:

<img width="1163" alt="Screen Shot 2021-04-07 at 9 18 28 AM" src="https://user-images.githubusercontent.com/22335838/113900005-38d5a400-9782-11eb-80ef-b1ae7c174c5c.png">

I used default parameters, except for `--mincov 5`, with the merged BAM to identify SNP variants in my dataset. Again, I saved all of my output to `gannet` since it was large!

```
#Defaults: --minhetfreq 0.1 --minhomfreq 0.85 --minquali 15 --maxcover 1000 --minread2 2 --errorate 0.02 --mapvalue 20
#Saving output to gannet
!perl /Users/Shared/Apps/BS-Snper-master/BS-Snper.pl \
--fa /Volumes/web/spartina/project-oyster-oa/data/Cg-roslin/cgigas_uk_roslin_v1_genomic-mito.fa \
--input /Volumes/web/spartina/project-gigas-oa-meth/output/BS-Snper/merged-sorted-deduplicated.bam \
--output /Volumes/web/spartina/project-gigas-oa-meth/output/BS-Snper/SNP-candidates.txt \
--methcg /Volumes/web/spartina/project-gigas-oa-meth/output/BS-Snper/CpG-meth-info.tab \
--methchg /Volumes/web/spartina/project-gigas-oa-meth/output/BS-Snper/CHG-meth-info.tab \
--methchh /Volumes/web/spartina/project-gigas-oa-meth/output/BS-Snper/CHH-meth-info.tab \
--mincover 5 \
> /Volumes/web/spartina/project-gigas-oa-meth/output/BS-Snper/SNP-results.vcf \
2> /Volumes/web/spartina/project-gigas-oa-meth/output/BS-Snper/merged.ERR.log
```

I used a loop to run `BS-Snper` on individual samples, making sure to set any variables necessary for the loop within the same cell. Before running the loop, I moved to [this directory](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/bismark-roslin/) where the BAM files were located. After the loop finished running, I moved all the files [this directory for `BS-Snper` output](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/BS-Snper/).

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

mv *SNP-candidates.txt *meth-info.tab *SNP-results.vcf *ERR.log ../BS-Snper/
```

### Going forward

1. Write methods
2. Obtain relatedness matrix and SNPs with [EpiDiverse/snp](https://github.com/EpiDiverse/snp)
2. Revise `methylKit` study design: separate tests for indeterminate and female oysters
3. Run R script on `mox`
3. Write results
4. Identify genomic location of DML
2. Determine if RNA should be extracted
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
