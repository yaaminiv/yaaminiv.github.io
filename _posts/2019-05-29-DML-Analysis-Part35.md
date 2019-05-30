---
layout: post
comments: true
title: DML Analysis Part 35
tags: virginica MBDSeq IGV bedtools
---

## Finalizing *C. virginica* tracks and revisiting analyses

### Generating genome feature tracks

Two weeks ago, I was [struggling to generate a feasible intron track](https://yaaminiv.github.io/DML-Analysis-Part34/). [At FROGER](https://yaaminiv.github.io/FROGER-Recap/), Jon shared the code he used to generate *C. virginica* feature tracks [to the wiki](https://github.com/hputnam/FROGER/wiki/Genome-Sequence-Files-and-Feature-Tracks#genome-features-generated-by-jpuritz). The code he used to generate tracks was similar to [my code](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-05-13-Generating-Genome-Feature-Tracks.ipynb), which was comforting. Looking at his code for intron tracks, he used a combination of `complementBed` and `intersectBed`, which is what [Sam suggested in this issue](https://github.com/RobertsLab/resources/issues/692#issuecomment-494526951). I decided to use that approach with my own tracks.

Instead of just creating an intron track, I decided to create all of the additional tracks Jon created: intergenic regions, non-coding sequences, introns, and mtDNA. Before I could create tracks, I needed to create a `bedtools` friendly genome file. The genome file needed to have chromosome name and lengths. I [previously created this document](https://yaaminiv.github.io/DML-Analysis-Part4/), so I just copied it to my analysis folder (it can be found [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-05-13-Generating-Genome-Feature-Tracks/2018-06-15-bedtools-Chromosome-Lengths.txt)). I also isolated just the chromosome names in [this document](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-05-13-Generating-Genome-Feature-Tracks/2019-05-28-bedtools-Chromosome-Names.txt). 

To isolate intergenic regions, I first needed to sort the gene track. I used `sortBed` to organize the listings by chromosme name:

```
!{bedtoolsDirectory}sortBed \
-faidx {chromNames} \
-i C_virginica-3.0_Gnomon_gene_yrv.gff3 \
> C_virginica-3.0_Gnomon_gene_sorted_yrv.gff3
```

Jon also included a step where he used `mergeBed` on the sorted file to merge any transcript variants. I skipped this step for now and sorted my exon track. I used `complementBed` with the sorted gene track to find all possible locations of intergenic regions, then removed sorted exons with `subtractBed`:

```
!{bedtoolsDirectory}complementBed \
-i {geneList} -sorted \
-g {chromLengths} \
| {bedtoolsDirectory}subtractBed \
-a - \
-b {exonList} \
> C_virginica-3.0_Gnomon_intergenic_yrv.gff3
```

Non-coding sequences were created using the complement of exons:

```
!{bedtoolsDirectory}complementBed \
-i {exonList} \
-g 2018-06-15-bedtools-Chromosome-Lengths.txt \
> C_virginica-3.0_Gnomon_noncoding_yrv.gff3
```

I then used `intersectBed` with non-coding sequences and genes to find introns, since introns are non-coding sequences found in genes:

```
!{bedtoolsDirectory}intersectBed \
-a {nonCDS} \
-b {geneList} -sorted \
> C_virginica-3.0_Gnomon_intron_yrv.gff3
```

Coding sequences are those that are translated into proteins. By subtracting coding sequences from exons, I also derived untranslated regions of exons:

```
!{bedtoolsDirectory}subtractBed \
-a {exonList} \
-b {CDSList} \
-sorted \
-g {chromLengths} \
> C_virginica-3.0_Gnomon_exonUTR_yrv.gff3
```

Generating the mitochondrial DNA track was the easiest. The smallest chromosome, NC_007175.2, is mtDNA, so I just needed to isolate genes from that chromosome:

```
!grep "NC_007175.2" ref_C_virginica-3.0_top_level.gff3 > C_virginica-3.0_Gnomon_mtDNA_yrv.gff3
```

### Visualizing tracks in IGV

Of course, no track generation is complete without looking at the products. I perused the tracks in [this IGV session](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-05-13-Generating-Genome-Feature-Tracks/2019-05-13-Genome-Track-Verification.xml) and was pretty happy with how they turned out! I am more confident in these tracks than the pre-generated ones because I understand how they were made.

![Screen Shot 2019-05-28 at 6 42 36 PM](https://user-images.githubusercontent.com/22335838/58594496-a594c480-8222-11e9-989d-b8155df4b3b8.png)

![Screen Shot 2019-05-28 at 6 42 05 PM](https://user-images.githubusercontent.com/22335838/58594497-a594c480-8222-11e9-9303-9847235ead94.png)

![Screen Shot 2019-05-28 at 6 41 28 PM](https://user-images.githubusercontent.com/22335838/58594498-a594c480-8222-11e9-91ab-8836300f97af.png)

**Figures 1-3**. All tracks in IGV.

Since my tracks looked good, I posted the `.gff3` and `.bed` verisions [here](https://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/2019-05-13-Yaamini-Virginica-Repository/analyses/2019-05-13-Generating-Genome-Feature-Tracks/). 

### Characterizing feature tracks

The reason I started generating new feature tracks is because Steven asked me to do so in [this issue](https://github.com/RobertsLab/resources/issues/685). I obtained the length of each feature track and counted overlaps with the pre-generated CG motif track.

**Table 1**. Length of feature tracks and number of overlaps with CG motifs. `intersectBed` was used to obtain CG motif overlaps with various feature tracks, defining the CG motif track as `-a` and the various feature tracks as `-b`.

|          **Feature**          | **Number of Elements** | **CG Motif Overlaps with Feature** |
|:-----------------------------:|:----------------------:|:----------------------------------:|
|             Genes             |         38,929         |              7,914,842             |
|       Intergenic regions      |         34,557         |              6,545,363             |
|              mRNA             |         60,201         |              7,507,167             |
|             Exons             |         731,279        |              2,330,546             |
|            Introns            |         316,614        |              5,596,808             |
|        Coding sequences       |         645,335        |              1,728,032             |
|      Non-coding sequences     |         336,677        |             12,142,171             |
| Untranslated regions of exons |         182,752        |               602,551              |
|             lncRNA            |          4750          |               281,715              |
|             mtDNA             |           78           |                 431                |

I also posted Table 1 in the issue. I noticed that there are more CG motif overlaps in the introns over exons, and non-coding sequences over coding sequences. However, genes have more overlaps than intergenic regions.

### Characterizing overlaps with DML

Now that I had better feature tracks, I figured I'd recharacterize my DML overlaps. Although I have more tracks I can use, I decided to focus on intersections with genes, exons, introns, transposable elements, and putative promoters. Anything that did not overlap with these categories could be considered "other" or "intergenic." I opened [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb), downloaded the new tracks from `gannet`, and changed the variable paths (again...so. forking. handy). I counted overlaps and wrote out files to [this directory](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-11-01-DML-and-DMR-Analysis). I didn't want to overwrite any of the files I generated with previous versions of my feature tracks, so I tagged everything with the date (2019-05-29). I also created a new directory for my revised flanking analysis, which can be found [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-11-01-DML-and-DMR-Analysis/2019-05-29-Flanking-Analysis). The previous version of my Jupyter notebook has overlap proportion calculations and extraneous chunks of code that were confusing, so I removed them from the notebook. It's not much cleaner and easier to follow (I think).

**Table 2**. Overlaps between DML and various genome feature tracks.

|               **Feature**               | **Hypermethylated DML** | **Hypomethylated DML** | **All DML** |
|:---------------------------------------:|:-----------------------:|:----------------------:|:-----------:|
|                  Genes                  |           289           |           271          |     560     |
|               Unique Genes              |           269           |           241          |     481     |
|                  Exons                  |           190           |           178          |     368     |
|                 Introns                 |            99           |           93           |     192     |
|       Transposable Elements (All)       |            26           |           31           |      57     |
| Transposable Elements (*C. gigas* only) |            16           |           23           |      39     |
|            Putative promoters           |            44           |           23           |      67     |
|                  Other                  |            9            |            6           |      15     |

One good thing from Table 2: the counts for the hypermethylated and hypomethylated DML always sum to the counts for all DML. For the most part, there's an even split between hyper- and hypomethylation. There are twice as many hypermethylated DML in putative promoters than hypomethylated DML. This makes sense since promoters have "housekeeping" functions. There are more DML found in exons than introns, which is also interesting. Exons are the sections of the genes that are most likely retained in coding sequences. There are 481 unique genes with DML, and 15 in other (potentially intragenic) regions.

### Characterizing general methylation trends

Keeping the `bedtools` party going, I decided to recharacterize overlaps of feature tracks with methylated CpGs, sparsely methylated CpGs, and unmethylated CpGs. Again, I generated new files and tagged them with the date (2019-05-29).

**Table 3**. Overlaps between various genome feature tracks and all 5x CpGs, methylated CpGs, sparsely methylated CpGs, and unmethylated CpGs.

|               **Feature**               | **All 5x CpGs** | **Methylated CpGs** | **Sparsely Methylated CpGs** | **Unmethylated CpGs** |
|:---------------------------------------:|:---------------:|:-------------------:|:----------------------------:|:---------------------:|
|                  Genes                  |    3,255,049    |      2,521,653      |            317,249           |        416,147        |
|               Unique Genes              |      33,126     |        25,496       |            26,953            |         27,753        |
|                  Exons                  |    1,366,779    |      1,013,691      |            105,871           |        247,217        |
|                 Introns                 |    1,884,429    |      1,504,791      |            211,143           |        168,495        |
|       Transposable Elements (All)       |    1,011,883    |       755,222       |            155,293           |        101,368        |
| Transposable Elements (*C. gigas* only) |     767,604     |       610,208       |            108,858           |         48,538        |
|            Putative promoters           |     203,330     |       134,468       |            27,429            |         41,433        |
|                  Other                  |     579,159     |       349,064       |            81,168            |        148,927        |

The main concern Mac brought up [in this issue](https://github.com/RobertsLab/resources/issues/685#issuecomment-488016732) that sent me on a saga of feature track generation was that overlap counts for methylated CpGs (`methylatedCpG`) were greater than those for all 5x CpGs (`totalCpG`). That doesn't seem to be the case this time!

### Returning to the chi-squared test

Since I felt better about my overlap counts, I updated [this document](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Overlap-Proportions.csv) so I could them for statistical analyses. Mac said she talked to Loveday Conquest about statistical tests, and that after some discussion chi-squared tests were deemed appropriate. I'm just going to continue with the chi-squared tests [in this R Markdown document](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Proportion-Test.Rmd) and perhaps ask Katie Lotterhos if she has other ideas or knows of any post-hoc tests during our next meeting.

**Table 4**. Results of chi-squared tests between various CpG groupings. All CpGs refer to all CG motifs in the *C. virginica* genome. MBD-Enriched CpGs are those found in *C. virginica* samples with at least 5x sequencing depth. Of those CpG enriched by MBD treatment, methylated CpGs were those with at least 50% methylation. DML were at lest 50% different between treatment with q-values < 0.01. Comparisons were only conducted with a particular CpG grouping and it's associated background (i.e. All CpGs vs. MBD-Enriched, MBD-Enriched vs. Methylated CpGs, and Methylated CpGs vs. DML), with N/A referring to tests that were not conducted. Values above the diagonal are χ² statistics with df = 4, and values below the diagonal are associated p-values.

|   **CpG Category**  | **All CpGs** | **MBD-Enriched** | **Methylated CpGs** | **DML** |
|:-------------------:|:------------:|:----------------:|:-------------------:|:-------:|
|     **All CpGs**    |       -      |      906940      |         N/A         |   N/A   |
|   **MBD-Enriched**  |   < 2.2e-16  |         -        |        15016        |   N/A   |
| **Methylated CpGs** |      N/A     |     < 2.2e-16    |          -          |  356.7  |
|       **DML**       |      N/A     |        N/A       |      < 2.2e-16      |    -    |

<img width="830" alt="Screen Shot 2019-05-30 at 4 18 00 PM" src="https://user-images.githubusercontent.com/22335838/58671266-e78f3a80-82f6-11e9-83ad-ddeca0eb49e1.png">

<img width="825" alt="Screen Shot 2019-05-30 at 4 18 15 PM" src="https://user-images.githubusercontent.com/22335838/58671273-eb22c180-82f6-11e9-855e-bff02f76ad4f.png">

<img width="815" alt="Screen Shot 2019-05-30 at 4 18 48 PM" src="https://user-images.githubusercontent.com/22335838/58671277-ee1db200-82f6-11e9-863b-312a70851028.png">

<img width="815" alt="Screen Shot 2019-05-30 at 4 18 33 PM" src="https://user-images.githubusercontent.com/22335838/58671283-f118a280-82f6-11e9-96fd-52937b07ce9e.png">

**Figures 4-7**. Proportion CpGs located in various genomic features for different CpG group comparisons. The figure comparing MBD-Enriched, methylated, sparsely methylated, and unmethylated proportions may be overkill.

All comparisons were significant, indicating that the distribution of CpGs in various genomic features is likely nonrandom. For All CpGs vs. MBD-Enriched, there was a higher proportion of all CpGs located in other regions (likely intergenic), but there was a higher proportion of enriched CpGs in exons and introns. The MBD-Enriched and methylated CpG groups have similar distributions, even though the test said the distriutions were significantly different. My guess is that this was driven by the slight differences between proportion CpGs in exons, promoters, and other regions. There was a much higher proportion of DML in exons and promoters versus methylated CpGs. Higher proportions of methylated CpGs were in introns, transposable elements, and other regions. It's interesting to see a higher proportion of DML in exons, since those are the regions that could get translated into proteins.

### Going forward

1. Figure out what's going on with DMR
2. Look at overlap proportions for DMR
3. Generate figures for DMR analyses
4. Conduct a gene enrichment for DML and DMR
5. Work through gene-level analysis
6. Update methods and results
7. Update paper repository
8. Write the discussion
9. Write the introduction
10. Revise my abstract
11. Share draft paper at the next Eastern Oyster Project Meeting

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
