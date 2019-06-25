---
layout: post
comments: true
title: DML Analysis Part 37
tags: virginica MBDSeq
---

## Gene annotation tables

 With my [finalized DML](https://yaaminiv.github.io/DML-Analysis-Part35/), I decided to describe the genes they are in. Steven suggested I start by making tables annotating the genes DML overlap with. Once I realized this would just involve some (hopefully) quick R manipulation, I created a grand plan:

![IMG_0378](https://user-images.githubusercontent.com/22335838/59392874-10b7be00-8d2e-11e9-918b-ff6147f68d13.jpg)

**Figure 1**. The grand plan.

You'll notice that the grand plan includes DMR...but I'm dropping the [DMR](https://yaaminiv.github.io/DML-Analysis-Part36/). After thinking about it more and discussing the DMR at the Eastern Oyster Project Meeting, I think they're just artificial and don't make sense when I have loci-level resolution.

To do this, I first wanted to create a master DML annotation list. Then, I could subset the master list based on overlaps with putative promoters and gene overlaps. I could further subset the annotated DML that overlap with genes into exon and intron overlaps. For all gene, exon, and intron overlaps, I can separate hypermethylated and hypomethylated loci. Separately, I can create a list of DML annotations with transposable elements. Since there are only 57 transposable element overlaps, I don't think I need to create subsets. Once I found some time to actually work in the midst of summer TAing at Friday Harbor, [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-Gene-Enrichment-Analysis.Rmd) and got cranking.

### Master DML-gene annotation

I wanted to create a master list with the following columns:

- chr
- start
- end
- p-value
- q-value
- meth.diff
- dist-to-closest-gene
- gene-chr
- gene-start
- gene-end
- gene-ID
- mRNA-start
- mRNA-end
- genbank
- uniprot-ID
- e-value
- product

#### DML-closestBed

I started with my [DML csv file](https://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/2019-06-11-Yaamini-Virginica-Repository/analyses/2018-10-25-MethylKit/2019-04-05-DML-Destrand-5x-Locations-Hypermethylated.bed) and merged that with the [information from `closestBed`](2019-05-29-Genes-Closest-NoOverlap-DMLs.txt). To get this merge to work, I needed to switch the positions of my DML and genes so `closestBed` would report the closest gene to each DML, and not the closest DML to each gene [in this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb):

```
! {bedtoolsDirectory}closestBed \
-a {DMLlist} \
-b {geneList} \
-t all \
-D ref \
> 2019-05-29-Flanking-Analysis/2019-05-29-Genes-Closest-NoOverlap-DMLs.txt #Although the file name says "NoOverlaps," overlaps are represented by "0" in distance column.
```

When I imported the dataset into R, I had to subtract 1 from the start position and add 1 to the end position so it lined up with the DML list. I merged the two files by DML start and end positions.

#### DML-closestBed-geneList

The next file I wanted to add to my master dataset was [the gene list](https://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/2019-05-13-Yaamini-Virginica-Repository/analyses/2019-05-13-Generating-Genome-Feature-Tracks/C_virginica-3.0_Gnomon_gene_sorted_yrv.gff3). I easily imported the `gff3` with `read.delim`, but my challenge came from sorting through the last column with gene description information. I usually use Excel Text-to-Columns for this, but I decided I needed a reproducible bash-wizardry method.

First, I `cut` the column with the gene descriptions and replaced `;` with `\tab` using `awk` and `gsub`:

```{bash}
cut -f9 ../2019-05-13-Generating-Genome-Feature-Tracks/C_virginica-3.0_Gnomon_gene_sorted_yrv.gff3 \
| awk  '{gsub(";","\t",$0); print;}' \
> 2019-06-20-Gene-Descriptions.txt #Isolate gene description column and convert ";" to "\t". Save the output as a new file
```

This gave me [a tab-delimited gene description output file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-Gene-Descriptions.csv). I tried importing this into R, but found that if a row had more columns than the first row, the information from those columns wrapped around to create an additional row. The hivemind of Stack Overflow told me to cound the number of columns in each row, then set `col.names` in `read.delim` with enough column names for the maxiumum number of columns. I used `awk` to count the number of columns in each row, then saved that in [this file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-Gene-Description-Column-Numbers.txt).

```{bash}
awk '{print NF}' 2019-06-20-Gene-Descriptions.txt > 2019-06-20-Gene-Description-Column-Numbers.txt
```

I imported the column number file into R and found the maximum number of columns was 9 with `max`. I set `col.names` in `read.delim` to accomodate 9 columns:

```{r}
geneDescriptions <- read.delim2("2019-06-20-Gene-Descriptions.txt", header = FALSE, col.names = c("gene-number", "gene-ID", "V3", "V4", "V5", "V6", "V7", "V8", "V9")) #Import document with gene descrptions. Name 9 columns so all rows are imported with 9 columns
```

I wasn't done yet! I ended up with gene IDs in a column, but there were extraneous characters before the actual ID that I did not need. I removed these with `gsub`:

```{r}
geneID <- geneDescriptions$gene.ID #Isolate gene ID column
geneID <- gsub("Dbxref=GeneID:", "", geneID) #Remove extraneous characters
```

Now that the `geneID` column was clean, I added the column to my gene list (I just created a dataframe, but I now realize I could have used `cbind`). Finally, I merged the revised gene list with isolated gene IDs with my master list.

#### DML-closestBed-geneList-mRNAList

The gene list has gene IDs, but the [Uniprot codes I have](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2018-09-11-Transcript-Uniprot-blastx-codeIsolated.txt) are matched with Genbank IDs from an mRNA file. The next intermediate step is to merge then mRNA list with my master list. Once again, I started by isolataing the mRNA description column with `cut` and using `awk` and `gsub` to replace ";" and "," delimiters with "\t".

```{bash}
cut -f9 ../2019-05-13-Generating-Genome-Feature-Tracks/C_virginica-3.0_Gnomon_mRNA_yrv.gff3 \
| awk  '{gsub(";","\t",$0); print;}' \
| awk  '{gsub(",","\t",$0); print;}' \
> 2019-06-20-mRNA-Descriptions.txt #Isolate mRNA description column and convert ";" and "," to "\t". Save the output as a new file
```

I counted the number of columns with `awk` and imported the file into R, setting a maximum number of columns with `col.names` in `read.delim`. From the mRNA descriptions, I isolated the gene ID that matched with the gene track, and the Genbank ID to match with Uniprot codes:

```{r}
geneIDmRNAList <- mRNADescriptions$V3 #Isolate gene ID column
geneIDmRNAList <- gsub("Dbxref=GeneID:", "", geneIDmRNAList) #Remove extraneous characters
```

```{r}
genbankIDmRNAList <- mRNADescriptions$V4 #Isolate Genbank ID
genbankIDmRNAList <- gsub("Genbank:", "", genbankIDmRNAList) #Remove extraneous characters
```

In addition to getting gene and Genbank IDs, I wanted to get mRNA product descriptions from the amalgam of information in that description column. Since each row can have a different number of delimiters, I decided to use `cut` and `awk` with `gsub` to essentially set a custom delimiter. The products are denoted after "product=", so I set this as my new delimiter. This created a bunch of garbage in one column, and then product information and garbage in a second column. I then piped the that output into another `cut` command, where I isolated the second column with the product information. I used another `awk` and `gsub` to change ";" delimiters to "\t". My final file, found [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-mRNA-Products.txt), has products in the first column, then garbage in any remaining columns.

```{bash}
cut -f9 ../2019-05-13-Generating-Genome-Feature-Tracks/C_virginica-3.0_Gnomon_mRNA_yrv.gff3 \
| awk  '{gsub("product=","\t",$0); print;}' \
| cut -f2 \
| awk  '{gsub(";","\t",$0); print;}' \
> 2019-06-20-mRNA-Products.txt #Isolate mRNA description column and convert "product=" to "\t". This separates the descriptions into 2 columns. The beginning of second column has the product description. Take the second column of the output and convert ";" to "\tab" to better isolate mRNA products. Save the output as a new file.
```

Just like I did previously, I counted the number of columns and imported the file with `read.delim` and the appropriate number of `col.names`. I then isolated the first column with product descriptions, and appended that to the mRNA list with gene IDs and Genbank IDs. Finally, I merged the massive mRNA list with the master list.

#### DML-closestBed-geneList-mRNAList-Uniprot

Finally, I'll merged the working master with Uniprot codes by matching Genbank IDs. This was the easiest step. I reorganized the columns and ensured I didn't have any duplicate entries by using `distinct` in the `dplyr` package. I exported the file as [this .csv](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-Master-DML-Annotation.csv).

#### Gene subsets

Now that I had the master list, the easier part was subsetting files using `subset`. I started by subsetting DML-gene overlaps by defining `dist-to-closest-gene == 0` as my subset. I wrote out the file [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Gene-Annotation.csv). From the DML-gene overlaps, I used a `meth.diff > 0` subset to get [hypermethylated DML-gene overlaps](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Gene-Annotation-Hypermethylated.csv) and `meth.diff < 0` to get [hypomethylated DML-gene overlaps](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Gene-Annotation-Hypomethylated.csv).

### Master DML-exon and DML-intron annotations

Even though I had DML-gene overlaps annotated, I wanted to annotate DML-exon overlaps. I figured the most painless way to do this would be to obtain exon-gene overlaps in `bedtools`, then merge those overlaps with the DML-gene overlap annotations by gene start and end positions. In [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb), I used `intersectBed` to identify exon-gene overlaps.

When I imported this into R, I realized it was not necessary! My gene subset has DML start and end positions, which I could match with [DML-exon overlaps](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2019-05-29-DML-Exon.txt). *Womp womp*. Since the DML-exon overlap file was a BEDtools output, I needed to add 1 to the start position and subtract 1 from the end position. I could then merge the DML-exon overlaps with the DML-gene annotation by DML start position. With `merge`, I set `sort = FALSE` so my output would not be sorted by the `start` column. I also included `all.x = TRUE` so even if an exon did not match with a gene, it was still retained as a row in the final table. This would be important for including exons from coding sequences that did not overlap with genes. I saved the output [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Exon-Annotation.csv). I also subsetted annotation tables for [hypermethylated DML-exon overlaps](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Exon-Annotation-Hypermethylated.csv) and [hypomethylated DML-exon overlaps](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Exon-Annotation-Hypomethylated.csv). I repeated this process with DML-intron overlaps. I obtained a [DML-intron annotation table](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Intron-Annotation.csv) for all DML, [hypermethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Intron-Annotation-Hypermethylated.csv), and [hypomethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Intron-Annotation-Hypomethylated.csv).

### Master DML-promoter annotation

The `dist-to-closest-gene` column has base pair distances from DML to the closest genes, but I defined my putative promoter regions as 1 kb regions upstream of *transcription* start sites. Even though my `dist-to-closest-gene` column has important information, I need to look at distances between DML and mRNA. I quickly detoured to [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb) to rerun `intersectBed`. When I originally created my overlap files, I wrote out information from only the flanks without matching DML. I changed the `-wb` argument to `-wo` to get information from both files in the output. I then imported the [DML-promoter overlaps](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2019-05-29-Flanking-Analysis/2019-05-29-mRNA-1000bp-UpstreamFlanks-DML.txt) and merged that with the master annotation list by duplicating the column with the end of the promoter and naming it the start of the mRNA. I saved the annotated DML-promoter table [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-Promoter-Annotation.csv). 

### Master DML-TE annotation

I went through a similar process for the DML-TE annotation. I imported the [DML-TE overlap file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-07-DML-TE-all.txt). I also revised `intersectBed` by swapping `-wb` for `-wo` and producing this [Gene-TE overlap file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-07-Genes-TE-all.txt). I merged the documents by TE start and end positions, then merged the resultant output with the master table. Similar to the other tables I created, I retained all rows from the original overlap file I was interested in and removed any duplicate rows (merging artifacts) with `distinct`. The DML-TE annotations can be found [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-06-20-DML-TE-Annotation.csv).

And that's how you end up with an R Markdown file that's close to 600 lines long just to subset some files. Join me tomorrow for "how the heck to I adequately describe this" and "will a gene enrichment yield interesting results."

### Going forward

1. Finish gene description and enrichment
2. Work through gene-level analysis
3. Update methods and results
4. Update paper repository
5. Outline the discussion
6. Write the discussion
7. Write the introduction
8. Revise my abstract
9. Share the draft with collaborators and get feedback
10. Post the paper on bioRXiv
11. Prepare the manuscript for publication

{% if page.comments %}

<div id=“disqus_thread”></div>
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
