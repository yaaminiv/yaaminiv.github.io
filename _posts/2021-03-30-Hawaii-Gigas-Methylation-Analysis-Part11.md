---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 11
tags: hawaii gigas-ploidy samtools SNP
---

## Merging BAM files

### The plan

Based on Pubathon conversations, my plan is to call SNPs two different ways. First, I'll call SNPs for each individual sample. If any sample has a SNP, that will certainly affect DML calls. Katherine also suggested I merge samples and call SNPs. Loci that are SNPs across samples but not present in an individual sample can still change the meaning of a DML (that DML could be more associated with genetic differences than treatment). Both Mac and Katherine implied that default parameters would be more than enough to identify C/T SNPs for my goals, so I won't mess around with them.

### Headaches with `samtools` installation

Which returns me to the [original problem I had trying to merge BAM files](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part10/): [library versions](https://github.com/RobertsLab/resources/discussions/1153). Sam suggested I update the `htslib` library based on [these instructions](http://www.htslib.org/download/), and I was able to do that successfully! But after updating this library, I still had the same `samtools` installation error!

```
config.status: creating config.mk
dyld: Library not loaded: /usr/local/opt/mpfr/lib/libmpfr.4.dylib
  Referenced from: /usr/local/bin/gawk
  Reason: image not found
./config.status: line 879: 83237 Done(141)               eval sed \"\$ac_sed_extra\" "$ac_file_inputs"
     83238 Abort trap: 6           | $AWK -f "$ac_tmp/subs.awk" > $ac_tmp/out
config.status: error: could not create config.mk
```

Since the installation was referencing a library from `gawk`, Sam suggested I update `gawk`. I ran `brew update gawk` and got an error that suggested I don't have `gawk` installed on this machine:

![Screen Shot 2021-03-30 at 11 47 14 AM](https://user-images.githubusercontent.com/22335838/113040485-aca5fa00-914d-11eb-8b72-178c5daebc83.png)

I decided to reinstall Homebrew just to be safe, and then ran `brew install gawk`. Once I had everything installed, I tried once more with my `samtools` installation and STILL got the same error!

![Screen Shot 2021-03-30 at 1 32 40 PM](https://user-images.githubusercontent.com/22335838/113052792-686e2600-915c-11eb-981a-d216c4e48cf2.png)

At this point, I started digging around for specific solutions for Homebrew potentially interfering with `samtools` installations. I found [this issue](https://github.com/Homebrew/legacy-homebrew/issues/42209) that suggested running `brew install xz` prior to configuration. I still encountered the same error! I realized installing `xz` was specific to the error message the user got after trying to configure their program after reading [this Stack Overflow](https://stackoverflow.com/questions/31612086/cannot-get-ffmpeg-to-work-after-installing-from-homebrew) question linked in the issue. Based on one of the suggestions, I ran `brew search libmpfr` (the library that wasn't loaded in any error above): `Error: No formulae or casks found for "libmpfr".`

Then, I ran `brew search mpfr`:

```
Warning: mpfr 4.1.0 is already installed and up-to-date.
To reinstall 4.1.0, run:
  brew reinstall mpfr
```

I reinstalled `mpfr` with `brew reinstall mpfr` and see if that resolves the issue (and obviously it did not). Since `mpfr` is part of Homebrew, I realized I should just try installing `samtools` with `brew install samtools`! AND THAT FINALLY WORKED!

![Screen Shot 2021-03-30 at 2 00 09 PM](https://user-images.githubusercontent.com/22335838/113056122-3f4f9480-9160-11eb-9e5b-086392820375.png)

If anything, graduate school has taught me how to conduct very specific Google searches until you find someone else who solves your problem for you. Now that I had `samtools` installed, I could get to work.

### Merging BAM files

Before I could identify SNPs across all samples, I needed to merge my BAM files! Based on the [`samtools merge`]() documentation, I set up the following code in my [Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/05-BS-SNPer.ipynb):

```
%%bash

samtools merge \
/Volumes/web/spartina/project-oyster-oa/Haws/BS-Snper/merged-sorted-deduplicated.bam \
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_1_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam \
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
/Volumes/web/spartina/project-oyster-oa/Haws/bismark-2/zr3644_24_R1_val_1_val_1_val_1_bismark_bt2_pe.deduplicated.sorted.bam
```

I decided to save the output file (`merged-sorted-deduplicated.bam`) to `gannet` because there was no way that file would be small enough for Github. I contemplated adding the `-r` to add an `@RG` tag for each alignment, but I thought I'd run the code with no options to see what happens first.

After a few hours (that's what I get for not including a `-threads` specification), I had a merged file (found [here](https://gannet.fish.washington.edu/spartina/project-oyster-oa/Haws/BS-Snper/merged-sorted-deduplicated.bam)). I looked at the output and saw there was no `@RG` tag (which makes sense because I didn't specify where it should pull that information from). My next step is to run that file through BS-Snper to see if 1) it works and 2) the output is usable.

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
