---
layout: post
comments: true
title: WGBS Analysis Part 26
tags: manchester gigas-broodstock bedtools
---

## Characterizing general methylation landscape

Before looking into DML, I want to characterize the general methylation landscape! This will help me contextualize my DML information. I created [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/09-General-Methylation-Landscape.ipynb) for my analyses. All my output can be found in [this `gannet` folder](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/methylation-landscape/).

### Union BEDgraph

Before I use `intersectBed` to my hearts content and determine where CpG loci are found in the genome, I wanted to create a union BEDgraph that concatenated all of the coverage information from the samples. We did this for the coral methylation comparison paper, and I was able to use this file to create stacked barplots. I still would like to look at CpG methylation categories within individual files, in case I wanted to make a PCA and examine how genome features were driving differences in samples.

First, I downloaded all my sample BEDgraphs:

```
#Download 5x bedgraphs
!wget -r \
--no-check-certificate --no-directories --no-parent --reject "index.html*" \
-P . \
-A "*5x.bedgraph" https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/bismark-roslin/
```

Based on [this notebook](https://github.com/hputnam/Meth_Compare/blob/master/code/01.10-Mcap-Canonical-Coverage-Track.ipynb), I needed to sort my files before creating the union BEDgraph. I used `sortBed` to sort the files:

```
%%bash

for f in *5x.bedgraph
do
/Users/Shared/bioinformatics/bedtools2/bin/sortBed \
-i ${f} \
> $(basename ${f%_5x.bedgraph})_5x.sort.bedgraph
done
```

Then, I used `unionBedGraphs` to concatenate the information:

```
#Create union BEDgraph from sorted files
#Include a header
#Use N/A when there is no data for a CpG in a sample
#Define sample IDs
#Use sorted bedgraphs
#Save output
!{bedtoolsDirectory}unionBedGraphs \
-header \
-filler N/A \
-names 1 2 3 4 5 6 7 8 \
-i \
*5x.sort.bedgraph \
> union_5x.bedgraph
```

Once I had the union BEDgraph, I wanted to average percent methylation across all samples to get a single metric I could use in downstream analyses. I [used `pandas` to do this for the coral methylation paper](https://github.com/hputnam/Meth_Compare/blob/master/code/02.04-Characterizing-CpG-Methylation-5x-Union.ipynb), and did something similar for this dataset!

```
#Import union data into pandas
#Average all samples for total genome methylation information and save as a new column. NA are not included in averages
#Save dataframe in a tabular format and include N/As. Do not include quotes.
df = pd.read_table("union_5x.bedgraph")
df['total'] = df[['1', '2', '3', '4', '5', '6', '7', '8']].mean(axis=1)
df.to_csv("union-averages.bedgraph", sep = "\t", na_rep = "N/A", quoting = 3)
```

```
#Remove header
#Keep chr, start, end, and the average
#Save output
! tail -n+2 union-averages.bedgraph \
| awk -F'\t' -v OFS='\t' '{print $2, $3, $4, $13}' \
> zr3616_union-averages_5x.bedgraph
```

### General methylation landscape

With union BEDgraph and individual sample information, I wanted to quantify the number of methylated, sparsely methylated, and unmethylated loci. I used the same cutoffs I have previously:

- methylated: ≥ 50%
- sparsely methylated: 10-50%
- unmethylated: ≤ 10%

I used `awk` to separate out the different methylation categories and save those loci in a new file. I then counted the number of lines in those files, and saved the information in a different file. I expect I could use the count information to create stacked barplots.

```
%%bash
for f in zr3616*5x.bedgraph
do
    awk '{if ($4 >= 50) { print $1, $2, $3, $4 }}' ${f} \
    > ${f}-Meth
done
```

```
#Get line counts for each fine
#Remove 10th line (total entries)
#Ensure output is tab-delimited
#Save output
!wc -l *-Meth \
| sed '10,$ d' \
| awk '{print $1"\t"$2}' \
> zr3616_5x-Meth-counts.txt
```

### Modifying the intergenic track

When thinking about [the genome feature tracks I created](), I realized I made a mistake with the intergenic track! I subtracted exon information from the intergenic regions I identified initially. That doesn't make sense, because my initial `complementBed` command took the complement of genes, and genes contain exons. I modified my code so I subtracted the flanking regions instead of the exons (this is consistent with the coral methylation paper), and I updated the intergenic region track and copied the revised version to `halfshell`.

I do wonder if I should remove lncRNA and transposable elements from the intergenic track. It's easier to interpret if a locus is only assigned to one genome feature, but it's totally possible to think about lncRNA and transposable elements as being part of the intergenic regions. An existential question for later.

### Genomic locations of CpGs

I decided to look at CpG overlaps with the following genome tracks:

- gene
- exon UTR
- CDS
- intron
- upstream flanks
- downstream flanks
- intergenic regions
- lncRNA
- transposable elements

Since the exon track is the UTR + CDS information, and mRNA is the exon + intron information, I figured I didn't need to use those specific tracks. Thanks math! I created BEDfiles for the methylated, sparsely methylated, and unmethylated files, then used several `intersectBed` for loops to get loci that overlapped with specific genome features, then extracted line counts to save in a separate file:

```
%%bash
for f in *bed
do
    /Users/Shared/bioinformatics/bedtools2/bin/intersectBed \
    -u \
    -a ${f} \
    -b ../../genome-feature-files/cgigas_uk_roslin_v1_gene.gff \
    > ${f}-Gene
done
```

```
#Get line counts for each fine
# Remove 28th line (total entries)
#Ensure output is tab-delimited
#Save output
!wc -l *-Gene \
| sed '28,$ d' \
| awk '{print $1"\t"$2}' \
> zr3616_5x-Gene-counts.txt
```

### Going forward

1. Finish running `methylKit` randomization tests on R Studio server
2. Update `mox` handbook with R information
4. Determine genomic location of DML
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
