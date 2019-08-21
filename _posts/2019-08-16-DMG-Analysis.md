---
layout: post
comments: true
title: DMG Analysis
tags: virginica MBDSeq GO-MWU DMG
---

## Starting the differentially methylated gene analysis

After weeks of thinking about it, I'm finally looking at differentially methylated genes! I'm replicating the differentially methylated gene analysis from [this paper](https://www.biorxiv.org/content/early/2018/02/21/269076). The authors used `python` scripts for the analysis, so I started by reviewing their steps:

1. [Annotate coverage files from `bismark` with gene and exon information](https://github.com/lyijin/working_with_dna_meth/blob/master/annotate_bismark_cov.py)
2. [Calculte median % methylation for each gene](https://github.com/lyijin/pdae_dna_meth/blob/master/diff_meth_genes/bias_density_medians/calc_bias_density_medians.py)
3. [Check normality and variance assumptions](https://github.com/lyijin/pdae_dna_meth/blob/master/diff_meth_genes/normality_testing/test_normality.py)
4. [Perform t-tests to see if median % methylation differs by treatment, and correct p-values for multiple comparisons](https://github.com/lyijin/pdae_dna_meth/blob/master/diff_meth_genes/fujairah_vs_abu_dhabi/t-test.f_vs_ad.py)

For my analysis, I created [this repository folder]() and downloaded initial python scripts from [this repository](https://github.com/lyijin/working_with_dna_meth). I figured I could do the rest of the analysis in R, so I didn't download any other `python` scripts.

### Choosing a workflow

The first step is to annotate coverage files from `bismark` with gene information. I am using [this python script](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-annotate_bismark_cov.py), which I got from ). The majority of the script imports other functions and scripts.

I started running the script but encountered this error:

![Screen Shot 2019-08-14 at 2 38 21 PM](https://user-images.githubusercontent.com/22335838/63058281-2a49c300-bea1-11e9-94ed-916ded2fecb8.png)

I think the issue came from the [.gff3 I downloaded from NCBI](https://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/2019-05-13-Yaamini-Virginica-Repository/analyses/2019-05-13-Generating-Genome-Feature-Tracks/ref_C_virginica-3.0_top_level.gff3).

![Screen Shot 2019-08-14 at 2 42 10 PM](https://user-images.githubusercontent.com/22335838/63058498-b65bea80-bea1-11e9-8abd-ac13df203bc1.png)

I posted [this issue](https://github.com/RobertsLab/resources/issues/727), and Sam confirmed that the [parse.gff3 script](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/parse_gff3.py) is the reason why I got an error. The script I downloaded isn't a general GFF3 parser, so I would need to modify it for my own use. He asked me what I wanted to do, and I realized I don't need to use the exact script. Talking with Shelly on Friday, I also found out that Hollie was working on something similar. In the issue's comments, she shared [this Markdown document](https://github.com/hputnam/Geoduck_Meth/blob/master/RAnalysis/Scripts/Juv_Geoduck_Meth_notebook.md) and [this R script](https://github.com/hputnam/Geoduck_Meth/blob/master/RAnalysis/Scripts/methylation_analysis.R) with me. Since I'm more proficient in R, I decided to modify Hollie's workflow for my use.

### Annotating coverage files

#### Master gene annotation list

I created [this R Markdown script](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-Differentially-Methylated-Gene-Analysis.Rmd) for my analysis. The first thing I did was create a list of annotated genes. This was relatively straightforward for me because [I've done it before](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-Gene-Enrichment-Analysis.Rmd) — [twice](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-Gene-Enrichment-with-GO-MWU.Rmd)!. I matched genes with mRNA, and ended up with a preliminary list of 60,201 entries — the exact number of mRNA. The file can be found [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-All-Annotated-Gene-and-mRNA.csv).

Then came the first tricky part: selecting the longest mRNA for each gene. When looking at differentially methylated genes, Liew et al. assigned a function to the gene by using information from the longest mRNA. I wanted to do the same thing. I initially used `sort --unique` to try and do this, but that didn't work. I posted [this issue](https://github.com/RobertsLab/resources/issues/731), and Sam found a solution.

```{bash}
#For the 6th field [$6] (gene ID), remove all duplicate entries in a csv file (-F","). These entries are saved and can be extracted as a different output if needed. 
#Save output to a new file
awk -F"," '!x[$6]++' 2019-08-14-All-Annotated-Gene-and-mRNA.csv \
> 2019-08-14-All-Annotated-Gene-and-mRNA-Unique.csv
```

I was left with ~34,000 lines, so under half of all mRNA were from duplicate genes. I appended Uniprot Accession codes and GOterms to each line and saved the file [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-All-Annotated-Gene-and-mRNA-Unique.csv).

#### Issues with `full_join`

To match loci in coverage files with the genes they're in, Hollie used `full_join` from `dplyr`, then `%>% filter` to retain loci that were within the gene's start and end positions. I tried doing the same thing and got an error:

```
temp <- full_join(sample1, geneAnnotationsCondensedUniprotGOterms, by="gene.chr") %>% filter(start >= gene.start & start <= gene.end)
Error in full_join_impl(x, y, by_x, by_y, aux_x, aux_y, na_matches, environment()) : 
  negative length vectors are not allowed
```

Baffled, I went to Stack Overflow and found this could happen because the resultant table has too many lines. I posted [this issue](https://github.com/RobertsLab/resources/issues/732), in which Sam suggested I try the same code with subsets of my input files, and try the code on `ostrich` since it has more RAM. On `genefish`, I tried running the `full_join` using data from one chromosome in a sample, but it didn't work. I tried running full samples on `ostrich` but that didn't work either. Finally, I got code to work on `ostrich` using one chromosome from a sample and the annotated gene list with Uniprot Accession codes and GOterms. I had to use `merge` instead of `full_join` because `dplyr` is not compatible with that version of R, which also meant I couldn't use `%>%` to pipe the table into `filter` and needed a separate `filter` step. Two hours after I started running the `merge` code, it still wasn't done! Fed up, I was reminded of my best friend: `bedtools`.

#### Using `intersectBed`

Why did I spend time trying to figure out an R workaround when I could use `intersectBed` — a tool made for this *exact* purpose! I quickly found there was one caveat with `intersectBed`. I couldn't use the coverage file as is, since it contained numerical values that were not positions on the chromosome. I needed to remove the percent methylation column from each file, run `intersectBed`, then add the percent methylation information back in.

I used the following code to isolate the first three columns (chromosome, start, end) from each coverage file. I included `\t` to ensure the file will be tab-delimited (requirement for `bedtools`):

``{bash}
#For each file that ends in bedgraph
#Print the first three columns, tab-delimited, and save the columns in a new file that has the same base name and ends in posOnly
for f in *bedgraph
do
  awk '{print $1"\t"$2"\t"$3}' ${f} > ${f}.posOnly
done
```

With my newly created files, I was able to run `intersectBed`:

```{bash}
#For each file that ends in posOnly
#Use intersectBed to find where loci and genes intersect, allowing loci to be mapped to annotated genes
#wb: Print all lines in the second file
#a: file that ends in posOnly
#b: annotated gene list
#Save output in a new file that has the same base name and ends in -Annotated.txt
for f in *posOnly
do
  /Users/Shared/bioinformatics/bedtools2/bin/intersectBed \
  -wb \
  -a ${f} \
  -b 2019-08-14-Annotated-Gene-Longest-mRNA-Uniprot-GOterms.txt \
  > ${f}-Annotated.txt
done
```

#### Adding back percent methylation information

After annotating coverage files with `intersectBed`, I need to add percent methylation information back to the annotated files to calculate median percent methylation by gene. I could import 20 files into R, or I could use a series of `bash` commands. There are two steps, since Linux `join` does not allow you to `join` by multiple fields. The first step involves creating a new column with the information from the multiple columns you want to match in both files. The second step is to `join`.

Both my original coverage files and annotated coverage files have the same first three columns: chromosome, start, and end for each CpG locus. For all 20 files, I created a new column with information formatted as chromosome-start-end:

Then, I used `join` specifiying the columns I just created:

```{bash}
for f in *bedgraph
do
  join -j 1 -t $'\t' \
  ${f}.sorted \
  ${f}.posOnly-Annotated.txt.sorted \
  > ${f}.annotated-percentMeth
done
```

I used `wc -l` to check that the number of lines in the annotated files are the same as the number of lines in the files I just generated. There were no lines lost between the file pairs, so this part of my DMG analysis is done!

### Going forward

1. Calculate median percent methylation
2. Conduct t-tests by gene to identify DMG
3. Gene enrichment for DMG with GO-MWU
2. Update methods and results
2. Update paper repository
3. Outline the discussion
4. Write the discussion
5. Write the introduction
6. Revise my abstract
7. Share the draft with collaborators and get feedback
8. Post the paper on bioRXiv
9. Prepare the manuscript for publication

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
