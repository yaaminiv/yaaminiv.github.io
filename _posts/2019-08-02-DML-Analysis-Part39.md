---
layout: post
comments: true
title: DML Analysis Part 39
tags: virginica MBDSeq GO-MWU gene-enrichment
---

## Gene enrichment with GO-MWU

[GO-MWU](https://github.com/z0on/GO_MWU) is a rank-based gene enrichment method initially developed for transcriptomics data. GO terms for each gene are matched with parental gene ontology terms. Parental ontology categoresi with the exact same gene list are then combined. Groups are further combined if they share at least 75% of the same genes. After clustering is complete, GO-MWU uses a Mann-Whitney U test to identify genes that are significantly enriched. While DAVID looks at pre-determined significant genes and identifies those that are enriched, GO-MWU identifies gene ontology categories that are significantly enriched by corresponding up- or downregulated genes. After [hearing about GO-MWU from Katie](https://yaaminiv.github.io/DML-Analysis-Part38/), [I used it for the EIMD students' transcriptomics project](https://yaaminiv.github.io/Ecology-of-Infectious-Marine-Diseases-TA-Recap/). Since it worked well, I figured I should try it with my own data!

### Filtering loci in each gene

Differential methylation data is different from transcriptomics data, so I had to make a few tweaks to the input before I formatted it for GO-MWU. First of all, there can be multiple DML in each gene, and in some instances, those DML were not both hypermethylated or hypomethylated. Katie suggested I use the most significant DML in each gene, so I started there.

I created [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-Gene-Enrichment-with-GO-MWU.Rmd) and started by importing a gene background-mRNA overlap file I annotated with Uniprot codes in [a different R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-02-22-Gene-Enrichment-Analysis.Rmd). I then matched the Uniprot codes with GOterms. 

To get the `methylKit` significance measures, I went back to [my `methylKit` R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd) and exported all of the loci tested by `calculatedDiffMeth` (originally I just exported those that had a significant p-value, q-value, and a difference greater than 50%). The list of all loci tested can be found [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-Loci-Analysis/2019-06-30-All-Loci-Methylation-Statistic-Filtered-Destrand-50-Cov5.csv). 

My first goal was to create a master list with all loci tested by `methylKit`, `methylKit` significance measures, Uniprot codes, and GOterms. I thought this would be as easy as merging all loci tested with my annotated gene background file by the DML start position. When I did that, I only had ~4000 loci remaining. That didn't seem right to me, so I looked at the start positions in both files. I needed to add 1 to the start position in the annotated gene background file to account for different formatting of start and stop positions. Normally, I would add 1 to each start position and subtract 1 from each end position (CpG dinucleotide), but the gene background file had start and end positions separated by 1, not 2 (was not a dinucleotide). Once I modified the gene background file, I had ~700,000 overlaps! That made way more sense to me, so I proceeded with the analysis.

Once I merged the files, I wanted to follow Katie's advice and retain one DML per gene. I first sorted my file by Genbank ID (gene) and p-value:

```{r}
attach(allTestedGOterms) #Attach dataframe
allTestedGOterms <- allTestedGOterms[order(Genbank, pvalue),] #Sort by gene ID, then p-value
detach(allTestedGOterms) #Detatch dataframe
head(allTestedGOterms) #Confirm changes
```

I then used Linux `sort` to keep the most significant p-value for each entry:

```{bash}
#Sort a file by the Genbank column (--key = 9,9) and only retain unique lines (--unique). Save output to a new file
sort --unique --key=10,10 2019-07-30-All-Tested-Loci-Uniprot-GOTerms.csv \
> 2019-07-30-All-Tested-Loci-Uniprot-GOTerms-Unique.csv
```

The resultant file can be found [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-All-Tested-Loci-Uniprot-GOTerms-Unique.csv).

### Formatting GO-MWU input files

GO-MWU requires two different input files: 1 tab-delimited file with genes in one column and all associated GOterms separated by semicolons in another, and 1 .csv file with genes in one column and a continuous significance measure in another. In the same R Markdown file I was working in, I divided up the master list to create GO-MWU inputs.

Creating the tab-delimited file was straightforward. I saved the Genbank and GOterm columns from the master list [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-allTested-GO-Annotations-Table.tab). To properly format the inputs, I had to specify no column or row names, and remove quotes from any non-numeric information.

```{r}
write.table(goAnnotations, "2019-07-30-allTested-GO-Annotations-Table.tab", sep = "\t", col.names = FALSE, row.names = FALSE, quote = FALSE) #Save file
```

The table of significance measures, a .csv file, required more finesse. Similar to the GO-MWU example, I used signed negative log p-values modified from `methylKit` output. Hypomethylated loci would have negative p-values, and hypermethylated loci would have positive p-values. I extraced the Genbank, methlyation difference, and p-value columns. While subsetting, I log-transformed the p-values. I then used `subset` to isolate all rows that were hypermethylated, including anything with a methylation difference of 0 (meth.diff >=0). I looked at the range of the log p-values of hypermethylated loci and found they were all negative. I multiplied all log p-values by -1, then saved it as a new column. I repeated this process for hypomethylated loci, but since the log p-values for hypomethylated loci were already negative, I just copied that information into a new column. I used `rbind` to combine the hypermethylated and hypomethylated dataframes, removed extraneous columns, and saved my final file [here](). Similar to the tab-delimited file, I needed to specify no row names or quotes, but I included column names:

```{r}
write.csv(sigMeasuresCorrected, "2019-07-30-allTested-Table-of-Significance-Measures.csv", quote = FALSE, row.names = FALSE) #Save file
```

### GO-MWU enrichment

I downloaded the [README.md](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/README.md), [gene ontology database](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/go.obo), [`perl` script A](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/gomwu_a.pl), [`perl` script B](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/gomwu_b.pl), and [GO-MWU functions R script](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/gomwu.functions.R) from the GO-MWU Github repository. Instead of downloading [their R script](https://github.com/z0on/GO_MWU/blob/master/GO_MWU.R), I copied the commands into the R Markdown file I was already using.

The first part of of the GO-MWU script specifies the path the table of significance measures, GO annotations file, gene ontology database, and R script for pre-written GO-MWU functions. The `goDivision` (biological processes, cellular components, or molecular function) is also set:

```{r}
input="2019-07-30-allTested-Table-of-Significance-Measures.csv" # two columns of comma-separated values: gene id, continuous measure of significance. To perform standard GO enrichment analysis based on Fisher's exact test, use binary measure (0 or 1, i.e., either sgnificant or not).
goAnnotations="2019-07-30-allTested-GO-Annotations-Table.tab" # two-column, tab-delimited, one line per gene, multiple GO terms separated by semicolon.
goDatabase="go.obo" # download from http://www.geneontology.org/GO.downloads.ontology.shtml
goDivision="BP" # BP for biological processes
source("gomwu.functions.R")
```

Then, GO-MWU obtains parent terms, clusters gene ontology categories, and performs a Mann-Whitney U Test:

```{r}
# Calculating stats. It might take ~3 min for MF and BP. Do not rerun it if you just want to replot the data with different cutoffs, go straight to gomwuPlot. If you change any of the numeric values below, delete the files that were generated in previos runs first.
gomwuStats(input, goDatabase, goAnnotations, goDivision,
	perlPath="perl", # replace with full path to perl executable if it is not in your system's PATH already
	largest=0.1,  # a GO category will not be considered if it contains more than this fraction of the total number of genes
	smallest=5,   # a GO category should contain at least this many genes to be considered
	clusterCutHeight=0.25) # threshold for merging similar (gene-sharing) terms. See README for details.
```

I changed the `goDivision` from BP to CC or MF to look at enrichment in other categories. GO-MWU output includes dissimilarity tables for each `goDivision`, parental GOslim and corresponding GOterms from analysis inputs, and parental GOslim terms that match with each gene. For all three divisions, there were no GOterms that passed the 10% FDR, so there are no significantly enriched GOterms for DML. This aligns well with [what I found with DAVID](https://yaaminiv.github.io/DML-Analysis-Part38/). Looks like I'll need to rely on gene description for my discussion section.

### Description of parent GOslim

To describe functions of genes, I counted the frequency of the GOslim terms GO-MWU generated. I did this for all loci tested by `methylKit` for each GO division. I also wanted to look at the functions represented by DML. Since my DML lists have redundant genes, I subsetted DML from my condensed list of all loci tested:

```{r}
condensedDML <- subset(allTestedGOtermsCondensed, subset = abs(meth.diff) > 50 & pvalue < 0.05 & qvalue < 0.01) #Subset DML from condensed list. abs(meth.diff) ensures I get significant hypermethylated and hypomethylated loci.
```

I merged the condensed list of DML with GO-MWU GOslim output to count the number of GOslim terms. I also repeated this process with hypermethylated and hypomethylated loci. My tables can be found here:

**Biological processes**:

- [All loci tested](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-allTested-Parent-GOSlim-Frequency-BP.csv)
- [CondensedDML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDML-Parent-GOSlim-Frequency-BP.csv)
- [Hypermethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHyper-Parent-GOSlim-Frequency-BP.csv)
- [Hypomethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHypo-Parent-GOSlim-Frequency-BP.csv)

**Cellular components**:

- [All loci tested](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-allTested-Parent-GOSlim-Frequency-CC.csv)
- [CondensedDML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDML-Parent-GOSlim-Frequency-CC.csv)
- [Hypermethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHyper-Parent-GOSlim-Frequency-CC.csv)
- [Hypomethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHypo-Parent-GOSlim-Frequency-CC.csv)

**Molecular function**:

- [All loci tested](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-allTested-Parent-GOSlim-Frequency-MF.csv)
- [CondensedDML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDML-Parent-GOSlim-Frequency-MF.csv)
- [Hypermethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHyper-Parent-GOSlim-Frequency-MF.csv)
- [Hypomethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHypo-Parent-GOSlim-Frequency-MF.csv)

I think that's a wrap on the DML analysis! Time to identify differentially methylated *genes*.

### Going forward

1. Work through gene-level analysis
2. Update methods and results
3. Update paper repository
4. Outline the discussion
5. Write the discussion
6. Write the introduction
7. Revise my abstract
8. Share the draft with collaborators and get feedback
9. Post the paper on bioRXiv
10. Prepare the manuscript for publication

{% if page.comments %}

<div id=“disqus_thread”></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page’s canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page’s unique identifier variable
};
*/
(function() { // DON’T EDIT BELOW THIS LINE
var d = document, s = d.createElement(‘script’);
s.src = ‘https://the-responsible-grad-student.disqus.com/embed.js’;
s.setAttribute(‘data-timestamp’, +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href=“https://disqus.com/?ref_noscript”>comments powered by Disqus.</a></noscript>

{% endif %}

<script id=“dsq-count-scr” src=“//the-responsible-grad-student.disqus.com/count.js” async></script>
