---
layout: post
comments: true
title: WGBS Analysis Part 27
tags: manchester gigas-broodstock bedtools DML
---

## Characterizing DML

I have DML and I have genome feature tracks, so it's time to do the thing I've done the most (besides yelling at R Studio server): characterize genomic locations with `bedtools`!

### DML summary information

TL;DR, I have a lot of DML and I'm not sure if I picked the correct thresholds.

**Table 1**. Number of DML obtained for sex-specific contrasts at different percent methylation differences. A q-value of 0.01 was used for each test. After `unite`, I had 5306686 total CpG loci with data.

|  **Contrast** | **25%** | **50%** | **75%** | **100%** |
|:-------------:|:-------:|:-------:|:-------:|:--------:|
|     Female    |  20830  |   3709  |   315   |    N/A   |
| Indeterminate |  129040 |  73414  |  21891  |   2861   |

My guess is that I have several DML because my sample size is lower. I think that using my most restrictive thresholds in downstream analyses is the best course of action until I get my randomization test results, which means a 75% difference for female-DML, and 100% difference for indeterminate-DML.

Another important note: `methylKit` used a Fisher test for the indeterminate samples since there were two of them! I need to go through the manual and confirm what exact test was used:

![Screen Shot 2021-04-30 at 10 35 49 AM](https://user-images.githubusercontent.com/22335838/117893790-f404c780-b26f-11eb-95eb-abab55d40c22.png)

### Looking at DML in IGV

I created [this IGV session](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/10_DML-characterization/dml.xml) to look at my DML and see if my cutoffs made any sense. I quickly created BEDfiles for DML lists in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.ipynb) and saved the output [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/10_DML-characterization/). I also color-coded the samples such that dark blue = female and light blue/cyan = indeterminate.

![Screen Shot 2021-05-11 at 12 44 55 PM](https://user-images.githubusercontent.com/22335838/117893868-18f93a80-b270-11eb-9907-e26717a2afbb.png)

**Figure 1**. Sample 5x bedgraphs and DML tracks in IGV.

I first thumbed through the IGV session and looked at DML identified in female samples. Based on what I saw, I feel comfortable using a 75% difference to identify DML. I looked at some DML that were at least 50% different between samples, and while I was comfortable calling that CpG differentially methylated, the sheer number of 50% DML I have make me think that being more conservative is better for a small sample size. The 25% cutoff was not convincing.

![Screen Shot 2021-05-11 at 12 46 29 PM](https://user-images.githubusercontent.com/22335838/117893876-1b5b9480-b270-11eb-95fe-c39d20ae4565.png)

![Screen Shot 2021-05-11 at 12 48 04 PM](https://user-images.githubusercontent.com/22335838/117893877-1b5b9480-b270-11eb-9ad3-e64a89a0f0c1.png)

![Screen Shot 2021-05-11 at 12 50 58 PM](https://user-images.githubusercontent.com/22335838/117893879-1c8cc180-b270-11eb-9921-b81af49d9b24.png)

![Screen Shot 2021-05-11 at 12 51 30 PM](https://user-images.githubusercontent.com/22335838/117893880-1c8cc180-b270-11eb-92ff-9afc4db34415.png)

![Screen Shot 2021-05-11 at 12 50 11 PM](https://user-images.githubusercontent.com/22335838/117893878-1bf42b00-b270-11eb-95f5-9e1d08251660.png)

**Figures 2-6**. Female DML and associated methylation information for each sample. Figures 2-3 look at DML that are 75% different, Figures 3-5 look at DML that are 50% different, and Figure 6 looks at a DML that is 25% different.

### Common DML between sex-specific contrasts

Once I confirmed I wanted to use the 75% different female-DML and 100% different indeterminate-DML, I wanted to see if there were any DML that were common between my sex-specific lists. I returned to [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.ipynb) and used `intersectBed` to pull out overlapping DML:

```
#Find overlaps between female- and indeterminate-DML
#Check head
#Count number of overlapping DML
!{bedtoolsDirectory}intersectBed \
-u \
-a DML-pH-75-Cov5-Fem.csv.bed \
-b DML-pH-100-Cov5-Ind.csv.bed \
> DML-pH-Cov5-Overlaps.bed
!head DML-pH-Cov5-Overlaps.bed
!wc -l DML-pH-Cov5-Overlaps.bed
```

I saved the output [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/10_DML-characterization/DML-pH-Cov5-Overlaps.bed). Turns out there are only 8 DML that overlap between the female and indeterminate samples!

### Quick tangent: CG motif track

Before I quantified overlaps between genome feature tracks and DML, I realized I needed a better idea of how many CGs were in the Roslin genome! I opened [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/08-Generating-Genome-Feature-Tracks.ipynb) and used `grep -c` to count the number of CGs. The output was suspiciously low!

![Screen Shot 2021-05-11 at 10 52 32 AM](https://user-images.githubusercontent.com/22335838/117862022-fe5e9b80-b246-11eb-81ff-9c8b2c7eb091.png)

Clearly I was counting incorrectly, so I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1208) for feedback. Sam mentioned that I was missing instances with CGs on a new line! I looked through my old *C. virginica* code and found `fgrep -o -i CG {genome-file} | wc -l`. Using this code, I got ~13 million CpGs in the genome (for reference, the *C. virginica* genome has ~14 million). Sam still pointed out that this may not count across new lines, and in fact, he did bring up that point in [an old issue](https://github.com/RobertsLab/resources/issues/685#issuecomment-488439875)! He suggested that instead, I convert the FASTA to a tab-delimited file, and count CGs that way. When I tried his code, I was getting 0 CGs:

![Screen Shot 2021-05-11 at 11 55 52 AM](https://user-images.githubusercontent.com/22335838/117869590-d7589780-b24f-11eb-9b70-b7c9f1db0755.png)

I think the `seqkit fx2tab` wasn't properly converting the FASTA to a tab-delimited format:

![Screen Shot 2021-05-11 at 11 54 25 AM](https://user-images.githubusercontent.com/22335838/117869431-a4160880-b24f-11eb-8939-16e7f503f03c.png)

Steven suggested I use [some code from Jay](https://github.com/jldimond/Coral-CpG/blob/master/ipynb/Ahya_CpG_ratio.ipynb), but I still ended up with the same error:

![Screen Shot 2021-05-11 at 12 04 03 PM](https://user-images.githubusercontent.com/22335838/117870452-015e8980-b251-11eb-952b-8baa6c3c8814.png)

At this point, I realized what I should be doing is creating a CG motif track! Having a separate track would allow me to look at it in IGV and obtain intersections between other BEDfiles of interest (like, all my genome features). I uploaded the Roslin genome and mitochondrial sequence FASTA to Galaxy and used EMBOSS `fuzznuc` to parse out CGs:

![Screen Shot 2021-05-11 at 12 19 30 PM](https://user-images.githubusercontent.com/22335838/117872110-2522cf00-b253-11eb-8766-a5cb7c15515d.png)

<img width="934" alt="Screen Shot 2021-05-11 at 4 08 28 PM" src="https://user-images.githubusercontent.com/22335838/117895399-206e1300-b273-11eb-8924-f5e3b12b5eeb.png">

I uploaded my [CG motif track to `halfshell`](http://owl.fish.washington.edu/halfshell/genomic-databank//cgigas_uk_roslin_v1_fuzznuc_CGmotif.gff), and proceeded to calculate how many CG motifs were present in each genome feature track with `intersectBed`!

**Table 2**. Genome feature overlaps with CG motifs

|   **Genome Feature**  | **Number of CG Motifs** |
|:---------------------:|:-----------------------:|
|         Total         |        13,246,547       |
|         Genes         |        7,425,225        |
|          CDS          |        1,516,641        |
|          Exon         |        2,216,766        |
|         lncRNA        |         454,393         |
|          mRNA         |        7,040,925        |
|        Non-CDS        |        11,040,607       |
|        Introns        |        5,221,932        |
|        Exon UTR       |         700,167         |
|       All flanks      |        1,091,847        |
|    Upstream flanks    |         592,834         |
|   Downstream flanks   |         549,787         |
|   Intergenic regions  |        4,730,728        |
| Transposable elements |        5,424,483        |

### Genomic location of DML

I returned to my [DML analysis notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/10-Genomic-Location-of-DML.ipynb) to keep using `intersectBed` and find overlaps between genome features and female-DML, indeterminate-DML, and overlapping DML.

**Table 3**. DML overlaps with genome features

|   **Genome Feature**  | **Female** | **Indeterminate** | **Overlaps** |
|:---------------------:|:----------:|:-----------------:|:------------:|
|         Total         |     315    |        2861       |       8      |
|          Gene         |     287    |        2519       |       8      |
|        Exon UTR       |     20     |        181        |       1      |
|          CDS          |     78     |        789        |       3      |
|         Intron        |     190    |        1552       |       4      |
|    Upstream flanks    |      2     |         33        |       0      |
|   Downstream flanks   |     16     |        140        |       0      |
|   Intergenic regions  |     12     |        178        |       0      |
|         lncRNA        |      5     |         36        |       0      |
| Transposable elements |     91     |        829        |       0      |

Similar to what I found with *C. virginica*, most of the DML are in genic regions! Hopefully this bodes well for any downstream enrichment analysis.

### Going forward

1. Finish running `methylKit` randomization tests on R Studio server
2. Update `mox` handbook with R information
2. Obtain relatedness matrix and SNPs with [EpiDiverse/snp](https://github.com/EpiDiverse/snp)
3. Filter C/T SNPs
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
