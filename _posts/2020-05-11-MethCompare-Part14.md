---
layout: post
comments: true
title: MethCompare Part 14
tags: MethCompare bedtools
---

## Reworking flanking and intergenic regions

One of my tasks this week was to [split the flanking region track](https://github.com/hputnam/Meth_Compare/issues/65) into separate upstream and downstream flanking tracks. The main hypothesis associated with RRBS is that it captures methylation in promoter regions. Although [my stacked barplots](https://yaaminiv.github.io/MethCompare-Part13/) did not show any large amount of methylation occuring in flanking regions, we thought this was a necessary step to address the RRBS hypothesis.

### Creating upstream and downstream flanks

[In this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Generating-Genome-Feature-Tracks.ipynb) I started to separate out entries in the combined flanking region track. I used the following code to parse out every other line as an upstream or downstream flank, depending on whether the strand was a forward or reverse strand:

```
#Select forward strands
#Separate upstream and downstream flanks
!grep "+" Mcap.GFFannotation.flanks.gff \
| awk '{ if (NR%2) print > "Mcap.GFFannotation.flanks.forwardUpstream.gff"; \
else print > "Mcap.GFFannotation.flanks.forwardDownstream.gff" }'
```

I then used `cat` to combine up- or downstream flanks from forward and reverse strands. I used these revised tracks to characterize the locations of CpGs! Feeling good, I realized I needed to look at my tracks in IGV. I opened up the [*M. capitata*](https://github.com/hputnam/Meth_Compare/blob/master/genome-feature-files/Mcap-Genome-Feature-Tracks.xml) and [*P. acuta*](https://github.com/hputnam/Meth_Compare/blob/master/genome-feature-files/Pact-Genome-Feature-Tracks.xml) IGV sessions realized that my code did not parse things correctly:

![Screen Shot 2020-05-11 at 1 35 54 PM](https://user-images.githubusercontent.com/22335838/81624735-cad70000-93ab-11ea-8f60-4348de9995fc.png)

**Figure 1**. Error in *M. capitata* flank parsing.

Since I used the same code for both species, I assumed there was an error in the *P. acuta* tracks too. I looked at the [`flankBed`](https://bedtools.readthedocs.io/en/latest/content/tools/flank.html) documentation and noticed I could specify a flank length for a left (`-l`) or right (`-r`) flank. I tried running the following code but got an error:

![Screen Shot 2020-05-11 at 3 04 59 PM](https://user-images.githubusercontent.com/22335838/81616685-cbb26680-9398-11ea-979b-b30f46b0a49b.png)

I posted [this issue](https://github.com/RobertsLab/resources/issues/930), and Shelly suggested I create new tracks using `-l` and `-r` specification. Instead of using these separately, I could set one to be 1000 and another to be 0. If I combined that wiht `-s`, I could get upstream and downstream flanks correctly while incorporating strand direction:

```
#Create flanking regions
#Create upstream flanks (-l) based on strand (-s)
#Subtract existing genes so flanks do not have any overlap
!{bedtoolsDirectory}flankBed \
-i Pact.GFFannotation.Genes.gff \
-g Pact.genome_assembly-sequence-lengths.txt \
-l 1000 \
-r 0 \
-s \
| {bedtoolsDirectory}subtractBed \
-a - \
-b Pact.GFFannotation.Genes.gff \
> Pact.GFFannotation.flanks.Upstream.gff

#Create flanking regions
#Create downstream flanks (-r) based on strand (-s)
#Subtract existing genes so flanks do not have any overlap
!{bedtoolsDirectory}flankBed \
-i Pact.GFFannotation.Genes.gff \
-g Pact.genome_assembly-sequence-lengths.txt \
-l 0 \
-r 1000 \
-s \
| {bedtoolsDirectory}subtractBed \
-a - \
-b Pact.GFFannotation.Genes.gff \
> Pact.GFFannotation.flanks.Downstream.gff
```

I confirmed these tracks in IGV, then uploaded them to [this `gannet` folder](https://gannet.fish.washington.edu/spartina/Meth_Compare/genome-feature-files/). I used the URL version of the final tacks in the sessions I saved so they can be viewed from any machine. I then proceeded to characterize genomic locations of CpGs with my new flank tracks.

***M. capitata***:

- [Upstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/genome-feature-files/Mcap.GFFannotation.flanks.Upstream.gff)
- [Downstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/genome-feature-files/Mcap.GFFannotation.flanks.Downstream.gff)

***P. acuta***:

- [Upstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/genome-feature-files/Pact.GFFannotation.flanks.Upstream.gff)
- [Downstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/genome-feature-files/Pact.GFFannotation.flanks.Downstream.gff)

### New intergenic tracks

After I characterized the location of CpGs with upstream and downstream flanks for the second time, I was ready to finish up for the day when I realized I needed to augment my intergenic region tracks. As defined with `intersectBed -v`, the intergenic regions were any part of the genome that did not include genes. However, flanking regions now overlapped with my intergenic regions. It doesn't make sense for something to be considered a flank and an intergenic region, so I decided to include flanking regions in my `intersectBed -v` code. When I tried adding an additional filename to my `bedtools` loops, it wouldn't work! I then realized it would be better to just create intergenic region tracks, so that's what I did.

Back in [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Generating-Genome-Feature-Tracks.ipynb), I created intergenic region tracks by finding the complement of genes and subtracting out flanks. I used the concatenated flanking region tracks instead of the separate tracks since they contained the same information.

```
#Find intergenic regions
#Subtract defined flanks from intergenic regions
!/usr/local/bin/complementBed \
-i Mcap.GFFannotation.gene.gff \
-g Mcap.genome_assembly-sequence-lengths.txt \
| /usr/local/bin/subtractBed \
-a - \
-b Mcap.GFFannotation.flanks.gff \
| awk '{print $1"\t"$2"\t"$3}' \
> Mcap.GFFannotation.intergenic.bed
```

I originally created GFF files, but when I viewed them in IGV they weren't displaying. I remembered I needed to convert them to BEDfiles to be able to view in IGV (I encountered something similar with *C. virginica* tracks), which is why they are BEDfiles. The BEDfile for *M. capitata* is [here](https://github.com/hputnam/Meth_Compare/blob/master/genome-feature-files/Mcap.GFFannotation.intergenic.bed) and the *P. acuta* track is [here](https://github.com/hputnam/Meth_Compare/blob/master/genome-feature-files/Pact.GFFannotation.intergenic.bed).

### Characterizing locations

Alright, now I'll get into characterizing CpG locations. I used the new flank and intergenic region tracks to identify genomic locations for [union data](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union.ipynb), [DMC, and upset plot data](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Identifying-Genomic-Locations.ipynb). I created summary files that I will use for downstream analyses.

***M. capitata***:

- [Union data upstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union_5x-mcFlanksUpstream-counts.txt)
- [Union data downstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union_5x-mcFlanksDownstream-counts.txt)
- [Union data intergenic regions](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union_5x-mcIntergenic-counts.txt)
- [Upset data upstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Mcap/Mcap-upset-data-mcFlanksUpstream-counts.txt)
- [Upset data downstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Mcap/Mcap-upset-data-mcFlanksDownstream-counts.txt)
- [Upset data intergenic regions](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Mcap/Mcap-upset-data-mcIntergenic-counts.txt)

***P. acuta***:

- [Union data upstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Pact/Pact_union_5x-paFlanksUpstream-counts.txt)
- [Union data downstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Pact/Pact_union_5x-paFlanksDownstream-counts.txt)
- [Union data intergenic regions](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Pact/Pact_union_5x-paIntergenic-counts.txt)
- [Upset data upstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-upset-data-paFlanksUpstream-counts.txt)
- [Upset data downstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-upset-data-paFlanksDownstream-counts.txt)
- [Upset data intergenic regions](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-upset-data-paIntergenic-counts.txt)
- [DMC upstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-DMC-data-paFlanksUpstream-counts.txt)
- [DMC downstream flanks](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-DMC-data-paFlanksDownstream-counts.txt)
- [DMC intergenic regions](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Identifying-genomic-locations/Pact/Pact-DMC-data-paIntergenic-counts.txt)

### Going forward

2. Update methods
3. Update results
5. [Locate TE tracks](https://github.com/hputnam/Meth_Compare/issues/56)
6. [Characterize intersections between data and TE, and create summary tables](https://github.com/hputnam/Meth_Compare/issues/59)
2. [Perform statistical tests for all stacked barplot data and correct for multiple comparisons](https://github.com/hputnam/Meth_Compare/issues/68)
5. [Revise stacked barplots](https://github.com/hputnam/Meth_Compare/issues/66)
6. [Replicate Liew et al. Figure 1B](https://github.com/hputnam/Meth_Compare/issues/67)
7. Look into program for mCpG identification

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
