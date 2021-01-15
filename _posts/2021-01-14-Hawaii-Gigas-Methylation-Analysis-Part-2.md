---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 2
tags: hawaii gigas-ploidy mox trimgalore bismark
---

## Checking trimmed data and starting `bismark`

### Reviewing `trimgalore` and `multiqc` output

Once the trim job finished, the first thing I did was transfer all the output to `gannet`:

```
rsync —archive —progress —verbose /gscratch/scrubbed/yaaminiv/Hawes/analyses/trimgalore/* yaamini@172.25.149.226:/Volumes/web/spartina/project-oyster-oa/data/Haws/trimmed #Transfer files off of mox and onto gannet
```

After moving the files, I opened the [MultiQC report](https://htmlpreview.github.io/?https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/Haws_01-trimgalore/multiqc_report_1.html) to look at sample quality post-trimming. For each sample, `cutadapt` generally trimmed ≤ 10 bp of adapter sequences from each sample. This was in addition to the 10 bp hard trim from each end.

One thing I noticed is that samples 7 and 8 have more unique reads than the others. When looking at the percentage of unique vs. duplicate reads, all samples look roughly the same.

![Screen Shot 2021-01-14 at 12 34 02 PM](https://user-images.githubusercontent.com/22335838/104649286-5c002400-5669-11eb-8610-906009e7e3a4.png)

![Screen Shot 2021-01-14 at 12 34 13 PM](https://user-images.githubusercontent.com/22335838/104649288-5c98ba80-5669-11eb-9c71-7549266c21d8.png)

**Figures 1-2**. FastQC sequence counts.

Sequence quality, per sequence quality scores, per base sequence content, per base N content, sequence duplication levels, and adapter conetent were good for all samples! The per sequence GC content failed for all samples. Most samples had slight abnormalities or very unusual overrepresented sequences. All samples had slightly abnormal sequence length distributions, amnd most had slightly abnormal per tile sequence quality scores.

![Screen Shot 2021-01-14 at 11 32 08 AM](https://user-images.githubusercontent.com/22335838/104649274-5a366080-5669-11eb-879d-ce1af29fe70a.png)

![Screen Shot 2021-01-14 at 11 32 28 AM](https://user-images.githubusercontent.com/22335838/104649278-5b678d80-5669-11eb-8b64-30b2ea82b4c9.png)

![Screen Shot 2021-01-14 at 11 32 56 AM](https://user-images.githubusercontent.com/22335838/104649281-5b678d80-5669-11eb-9b50-0483e1427a6c.png)

![Screen Shot 2021-01-14 at 11 33 04 AM](https://user-images.githubusercontent.com/22335838/104649285-5c002400-5669-11eb-8624-b0d6e85512a7.png)

**Figures 3-7**. Status checks for metrics that were slightly abnormal or very unusual.

In looking at some of the [FastQC reports from Sam](https://robertslab.github.io/sams-notebook/2020/12/06/FastQC-MultiQc-C.gigas-Ploidy-pH-WGBS-Raw-Sequence-Data-from-Haws-Lab-on-Mox.html), I noticed that the per sequence GC content remained the same before and after trimming for several samples. Many of the samples with overrepresented sequences were the 2nd paired files, and had a sequence with no hit.

![Screen Shot 2021-01-14 at 2 21 41 PM](https://user-images.githubusercontent.com/22335838/104661516-85c34600-567d-11eb-893f-c55b36bc06df.png)

**Figure 8**. Most common overrepresented sequence.

It's been a while since I've looked at FastQC and MultiQC reports, so I looked at the coral data. Per sequence GC content, sequence length distribution, and overrepresented sequences were potential problem areas with the [coral data as well](https://gannet.fish.washington.edu/metacarcinus/FROGER_meth_compare/20200311/WGBS_MBD/FASTQC/multiqc_report.html). I also looked at my [*C. virginica* gonad methylation data MultiQC report](http://owl.fish.washington.edu/Athaliana/20180409_fastqc_Cvirginica_MBD/multiqc_data/multiqc_report.html#fastqc_adapter_content). Most samples had slightly unusual per sequence GC content and per base sequence content, and all samples had slighty unusual sequence length distribution. Since I followed the trimming protocol suggested and have similar-ish results to some other data, I decided the files were decent enough to start `bismark`. I also posted [this issue]() to confirm my thinking with others.

### Starting `bismark`

First, I downloaded the [*C. gigas* bisulfite-converted genome that Sam created previously](https://robertslab.github.io/sams-notebook/2019/02/21/Data-Management-Create-C.gigas-Bisulfite-Genome-with-Bismark-on-Mox.html). I used `rsync` to transfer it to my data directory on `mox`. Then, I created [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/02-bismark.sh) based on the [coral methylation workflow](https://github.com/hputnam/Meth_Compare/blob/master/code/00.01-DNA-sequence-processing.md#script-that-ran-bismark-on-mox). I updated the paths for `bismark`, `bowtie2`, and `samtools` for the latest program versions, and also added `set -e` to the top of the script. This will cause the script to exit if any command fails,  instead of rnning through the entire script spitting out errors.

I transferred the script to my user directory so I could run it:

```
rsync —archive —progress —verbose yaamini@172.25.149.226:/Users/yaamini/Documents/project-oyster-oa/code/Haws/02-bismark.sh . #Transfer to user directory
sbatch 02-bismark.sh #Run script
```

When I ran the script, it immediately failed! So at least I knew `set -e` was working well. When I checked the script, I realized I did not update the filenames based on the trimmed files, so `xargs` could not find the trimmed files to use. I updated the filenames, transferred the revised script to `mox`, then ran the script. I still got the same error! I checked the script again and couldn't find any error. When I looked at my file structure, I realized that changing the "Hawes" directory to "Haws" made it such that there were two scripts. Since my script was open in Atom when I renamed the directory, I was still editing a version saved at a path different from the one I was transferring to `mox`! I quit Atom, moved the most updated version of the script to the correct directory, then transferred it to `mox`.

This time it ran successfully...kind of. It was able to find the trimmed files, but it couldn't find the *C. gigas* genome:

```
Failed to move to directory /gscratch/scrubbed/yaaminiv/Hawes/data/Cg-genome/Bisulfite_Genome/CT_conversion/: No such file or directory
```

The genome was in a directory nested inside `/gscratch/scrubbed/yaaminiv/Hawes/data/Cg-genome/`. I changed the directory path so that `bismark` could find the bisulfite genome folder in the directory I specified: `/gscratch/scrubbed/yaaminiv/Hawes/data/Cg-genome/Crassostrea_gigas.oyster_v9.dna_sm.toplevel/`

Finally, the script started running! I'll keep tabs on the progress over the next few weeks.

### Going forward

1. Look at `bismark` output
2. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)
3. Identify methylation analysis framework beyond `methylKit`

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
