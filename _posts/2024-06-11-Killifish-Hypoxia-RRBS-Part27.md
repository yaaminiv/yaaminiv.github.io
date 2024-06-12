---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 27
tags: killifish RRBS
---

## Characterizing DMR locations

There are several interesting questions I can tackle now that I have a more finalized DMR list! Where are the DMR in the genome, and are they close to any genes of interest?

### Generating genome feature tracks

Before I could launch into anything, I needed to create genome feature tracks for the new genome. To do this, I opened [this Jupyter notebook](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/07-generating-genome-feature-tracks.ipynb). I downloaded the annotation GFF from NCBI and pulled out the gene, CDS, exon, lncRNA, mRNA, and transcript tracks that were already annotated. I then used a series of `bedtools` commands to create nonCDS, intron, and intergenic tracks.

### Genomic locations of DMR

Once I had my genome feature tracks, I opened [this Jupyter notebook](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/08-annotate-DMR.ipynb) to understand where the DMR were in the genome. Instead of just using `intersectBed`, I used `closestBed` to look for overlapping features and closest features:

```
%%bash

for f in *DMR.sort.bed
do
   /opt/homebrew/bin/closestBed \
   -D "ref" \
   -a ${f} \
   -b ../../../genome-features/GCF_011125445.2_MU-UCD_Fhet_4.1_genomic-gene.gff \
   -g ../../../genome-features/mummichog.chrom.length \
   > $(basename ${f%_DMR.sort.bed})_DMR.closestGene.bed
done
```

I then extracted the features that were overlapping, since `closestBed` prints a distance of 0 if the features overlap!

```
%%bash

for f in *DMR.closestlncRNA.bed
do
   awk -F '\t' '$15 == 0' ${f} > $(basename ${f%_DMR.closestlncRNA.bed})_DMR.overlappinglncRNA.bed
done
```

```
%%bash

for f in *DMR.closestIntergenic.bed
do
   awk -F '\t' '$9 == 0' ${f} > $(basename ${f%_DMR.closestIntergenic.bed})_DMR.overlappingIntergenic.bed
done
```

The output can be found in [this folder](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/08-annotate-DMR/new-genome).

**Table 1**. Number of closest and overlapping features for various DMR categories

| **Comparison** | **Genes** |  **CDS** | **Exons** | **Introns** | **lncRNA** | **Intergenic** |
|:--------------:|:---------:|:--------:|:---------:|:-----------:|:----------:|:--------------:|
|  20 vs. 5 NBH  |   24 / 9  |  55 / 0  |   52 / 0  |    24 / 9   |   26 / 0   |     23 / 15    |
|  20 vs. OC NBH |   10 / 3  |  33 / 0  |   11 / 0  |    10 / 3   |   13 / 0   |     10 / 7     |
|  5 vs. 20 NBH  |  62 / 29  | 146 / 14 |  105 / 15 |   61 / 19   |   64 / 2   |     59 / 25    |
|  NBH vs. SC 5  |   13 / 7  |  36 / 0  |   34 / 0  |    13 / 7   |   15 / 1   |     13 / 6     |
|  NBH vs. SC OC |   1 / 1   |   1 / 0  |   1 / 0   |    1 / 1    |    1 / 0   |      1 / 0     |
|   NBH vs. SC   |   11 / 6  |  29 / 0  |   23 / 0  |    11 / 6   |   14 / 2   |     11 / 5     |

While it seems like there are several DMR close to CDS or exons, there are a higher proportion of overlapping DMR in introns. As expected with RRBS, there are a decent amount of intergenic DMR as well.

### DMR vs. genes of interest

I looked at DMR that overlapped with specific genes of interest: AHR, HIF, AIP, AHRR, and EGLN3. These are genes we know are involved in toxicant response. To do this, I took the files with DMR-gene overlaps and isolated the gene name. I then perused these gene names for genes of interest:

```
#Extract the 14th column
#Trim first 8 characters
#Convert ; to \t
#Extract the first column
#Sort and print unique entries
!cut -f14 20_5_N_DMR.overlappingGene.bed \
| cut -c 9- \
| tr ";" "\t" \
| cut -f1 \
| sort | uniq
```

None of the DMR overlapped with those specific genes. I asked Neel for the list of DEG, but when I looked through them I noticed that the gene naming convention from the `edgeR` output differs greatly from the gene naming convention in the NCBI annotations I'm using! I'm waiting for some additional information before I move forward with that.

### Common DMR within various categories

The last thing Neel wanted me to look at were commonalities between DMR lists. There were several comparisons within NBH that had DMR, and looking at commonalities would allow us to understand what methylation responses may be conserved in response to any disturbance in oxygen level. To do this, I used `intersectBed` to look for DMR that overlapped between various comparisons. I opted for intersecting DMR instead of the exact start and stop positions because of differences in CpGs with data between samples. I figured if they overlap at least a little bit, they probably have a similar regulatory role. Neel was also interested in DMR associated with population differences that were common with any NBH oxygen DMR. I used similar methods to identify these common DMR.

**Table 2**. Common DMR between various comparisons

|   **Comparison**   | **20 vs. 5** | **20 vs. OC** | **5 vs. OC** | **All NBH vs. SC** | **OC NBH vs. SC** | **5 NBH vs. SC** |
|:------------------:|:------------:|:-------------:|:------------:|:------------------:|:-----------------:|:----------------:|
|    **20 vs. 5**    |       -      |       -       |       -      |          -         |         -         |         -        |
|    **20 vs. OC**   |       0      |       -       |       -      |          -         |         -         |         -        |
|    **5 vs. OC**    |       8      |       2       |       -      |          -         |         -         |         -        |
| **All NBH vs. SC** |       0      |       0       |       0      |          -         |         -         |         -        |
|  **OC NBH vs. SC** |       0      |       0       |       0      |          0         |         -         |         -        |
|  **5 NBH vs. SC**  |       6      |       0       |       1      |          1         |         0         |         -        |

When I looked at the overlapping DMR within NBH, they were also methylated in the same direction! So that's also a pretty good indication that my `intersectBed` code was good enough. There were very few population-level DMR that overlapped with treatment DMR, except for DMR comparing NBH and SC samples exposed to hypoxia. For the common DMR between hypoxia samples and hypoxia vs. outside control in NBH, both DMR were hypomethylated. This means there was lower methylation in hypoxia and lower methylation for NBH fish. When comparing NBH fish exposed to normoxia or hypoxia with NBH and SC fish exposed to hypoxia, all DMR from the population comparison had higher methylation in NBH while all DMR from the treatment comparison had higher methylation in hypoxia. Based on these commonalities, it seems like NBH fish have levels of methylation that are similar to fish exposed to low oxygen. There is potentially something here to think about with respect to the higher pollutant and hypoxia tolerance in these fish.

### Going forward

1. Update methods and results
2. Map `edgeR` transcript names to IDs from genome
2. Examine DMR-DEG overlaps
3. Create DMR figure
6. Update OSF repository for all intermediate files
2. Conduct pathway analysis for RNA-Seq data by population
5. Identify known SNP/DMR overlaps

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
