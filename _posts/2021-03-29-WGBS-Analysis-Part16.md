---
layout: post
comments: true
title: WGBS Analysis Part 16
tags: manchester gigas-broodstock bismark mox
---

## Reviewing revised alignments

I got my Roslin genome-aligned data back! Last week the [paper for this genome](https://academic.oup.com/gigascience/article/10/3/giab020/6187865) was published as well, so it'll be nice to have that resource while I work with this data. Steven noticed that they have their own version of the mitochondrial genome, which is different than the [sequence I used](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part6/). This shouldn't be a problem since mitochondrial DNA isn't methylated.

### [`multiqc`](https://multiqc.info/)

I transferred all `mox` [`bismark`](https://github.com/FelixKrueger/Bismark) output to [this `gannet` folder](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/bismark-roslin/). I didn't run `multiqc` on `mox`, so I did that in the same folder. I then copied the HTML reports and `multiqc` output to [this repo folder](https://github.com/RobertsLab/project-gigas-oa-meth/tree/master/output/05-bismark-roslin)! Once I was done moving files around, I looked at the [`multiqc` report](https://nbviewer.jupyter.org/github/RobertsLab/project-gigas-oa-meth/blob/master/output/05-bismark-roslin/multiqc_report.html).

Similar to [what I saw with the Hawaii samples](), alignment to the Roslin genome was slightly lower, ranging between 61.5% to 62.0%. Percent duplicate reads was also lower when aligning to the Roslin genome! Percent duplication is much higher for this dataset than the Hawaii one (22.6%-26.4%), but that's probably because the histology DNA wasn't high quality. I noticed that the samples with more M C's/M Aligned had higher percent duplication, but overall percent aligned wasn't higher for these samples.

<img width="1370" alt="Screen Shot 2021-03-30 at 11 02 30 AM" src="https://user-images.githubusercontent.com/22335838/113035579-3652c900-9148-11eb-98bd-a7ca823dce5b.png">

**Figure 1**. General alignment statistics for oyster_v9 genome (left) and Roslin genome (right)

### IGV

Since my alignment statistics generally looked good, I got to move onto a more important step: checking the alignments in IGV to make sure there isn't any methylation in the mitochondrial sequence! I opened a new IGV session and entered in the [genome URL](), but it wouldn't load since I didn't have an index file. I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1166) to determine how to create a .fai file. After posting the discussion, I did a quick search and found [`samtools faidx`](http://www.htslib.org/doc/samtools-faidx.html). I decided to try `samtools` to create the index file. I returned to [this discussion post](https://github.com/RobertsLab/resources/discussions/1153) where I had some issues running `samtools`, and successfully installed the latest version of `htslib` using [these instructions](http://www.htslib.org/download/). However, I ran into an error installing the latest `samtools` version!  I downloaded the latest version on genefish and ran `./configure --prefix=/Users/Shared/Apps/samtools-1.12`. Got the following error:

```
config.status: creating config.mk
dyld: Library not loaded: /usr/local/opt/mpfr/lib/libmpfr.4.dylib
  Referenced from: /usr/local/bin/gawk
  Reason: image not found
./config.status: line 879: 83237 Done(141)               eval sed \"\$ac_sed_extra\" "$ac_file_inputs"
     83238 Abort trap: 6           | $AWK -f "$ac_tmp/subs.awk" > $ac_tmp/out
config.status: error: could not create config.mk
```

Sam suggested I upgrade `gawk` using `brew upgrade gawk`. When I tried that, I got another error:

![Screen Shot 2021-03-30 at 11 47 14 AM](https://user-images.githubusercontent.com/22335838/113040485-aca5fa00-914d-11eb-8b72-178c5daebc83.png)

Sam then suggested I install `gawk` before running `samtools`. While I was going down that rabbit hole, Steven commented on my original discussion post and said I should make an index file with IGV. IGV didn't give me an option to create an index file when I added the genome URL, but I found [this page](https://software.broadinstitute.org/software/igv/LoadGenome) in their documentation that has instructions to create a genome. Under Genomes > Create .genome File, I added in the FASTA URL and named the genome "Roslin-gigas-mito.genome." It worked! I saved the genome and index file [in this directory](https://gannet.fish.washington.edu/spartina/project-oyster-oa/data/Cg-roslin/) with my combined FASTA sequence. At last, I was able to load in sample bedgraphs and look at the mitochondrial sequence.

![Screen Shot 2021-03-30 at 1 20 52 PM](https://user-images.githubusercontent.com/22335838/113051396-c26dec00-915a-11eb-87b6-c87d4a1f59c6.png)

**Figure 2**. Mitochondrial sequence in IGV

The mitochondrial sequence was predominantly unmethylated, but there were some methylated CpGs in individual samples. I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1169) to determine what happened. Steven suggested I look only at 5x coverage. When he mentioned this, I realized I already created 5x bedgraphs, and should have been using those from the beginning! I loaded the 5x bedgraphs into IGV and looked at the mitochondrial genome.

<img width="1344" alt="Screen Shot 2021-04-04 at 1 14 16 PM" src="https://user-images.githubusercontent.com/22335838/113520425-d98c4f80-9547-11eb-9cc0-612091886d0f.png">

<img width="1344" alt="Screen Shot 2021-04-04 at 1 14 32 PM" src="https://user-images.githubusercontent.com/22335838/113520429-e01ac700-9547-11eb-8534-eef968f96916.png">

<img width="1344" alt="Screen Shot 2021-04-04 at 1 14 51 PM" src="https://user-images.githubusercontent.com/22335838/113520444-f32d9700-9547-11eb-8b8b-2eedd36cb24e.png">

**Figures 3-5**. Mitochondrial sequence in IGV using 5x begraphs

Looking at the full sequencence with 5x begraphs is definitely an improvement, but there are two areas with very slight methylation. I don't see a lot of consistency between samples either, so I don't think it's likely any of these areas will be called as DML. In any case, these methylation traces may be a sign of something else going on. I could remove these locations from my dataset, bump up coverage to 10x to see if that reduces methylation, or try mapping to the Roslin mitochondrial sequence. I saved [my IGV session](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/05-bismark-roslin/igv_session.xml) and commented on the discussion to get input on how to proceed.

### Going forward

1. Write methods
2. Identify SNPs in WGBS data mapped to Roslin genome
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
