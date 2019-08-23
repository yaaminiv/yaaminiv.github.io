---
layout: post
comments: true
title: DMG Analysis Part 2
tags: virginica MBDSeq DMG
---

## Identifying differentially methylated genes

After [messing around with code](https://yaaminiv.github.io/DMG-Analysis/) to annotate coverage files with gene information, I finally have files I can work with to do some number crunching in [this R Markdown script](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2019-08-14-Differentially-Methylated-Genes).

### Formatting files

I wanted to [use a shortcut](https://genefish.wordpress.com/2019/08/20/importing-multiple-files-to-r/) to import multiple files at once, but I quickly found out that doing so makes it impossible to include additional arguments with `read.delim` or `read.table`. Additionally, the original coverage files were not tab-delimited, so I had tab and space delimiters in my files! I decided that the best thing to do would be to format, then import.

The first formatting issue I tackled was the fact that the original coverage files had space delimiters instead of tab-delimiters. Importing files with multiple delimiters into R, or trying to separate columns once imported, seemed hellish. I went to my code to create the `join` key. Instead of printing all columns in the file as-is after creating the new column, I specified tab delimiters between columns:

```{bash}
#For each file that ends in bedgraph (the original coverage files)
#Print the first three columns (chr, start, end) with dashes ("-") in between. Then, print the rest of the columns (chr, start, end, percentMeth) in the file with tabs ("\t") inbetween.
#Save the file with the base name + .sorted
for f in *bedgraph
do
  awk '{print $1"-"$2"-"$3"\t"$1"\t"$2"\t"$3"\t"$4}' ${f} \
  | sort -k1,1 \
  > ${f}.sorted
done
```

I re-joined my files and was ready to tackle the second issue: column headers. When I tried importing all 10 files at once with `list2env` (more on that below), I couldn't modify arguments of `read.delim`. Instead of messing with R code, I decided to add headers to each file. The minds at Stack Overflow suggested [a few workarounds](https://stackoverflow.com/questions/12902497/add-a-header-to-a-tab-delimited-file). I tried to keep column names consisent between previous versions of the files. Between each column name, I typed `\t` to include tab delimiters.


```{bash}
#For each file that ends in percentMeth
#List tab-delimited column headers
#Add the file after the headers
#Save output as a new file with the base name + ".header"
for f in *percentMeth
do
  echo -e "key\tchr\tstart\tend\tpercentMeth\tchr2\tstart2\tend2\tgene-chr\tgene-start\tgene-end\tmRNA-start\tmRNA-end\tgeneID\tGenbank\tmRNALength\tProduct\tUniprot\tGO" \
  | cat - ${f} \
  > ${f}.header
done
```

Finally, I was ready to import files into R! Instead of importing 10 files separately, I wanted to import them all together. Once again, Stack Overflow helped.

```{r}
#Create a file list for all 10 files to import
#Import files to the .GlobalEnv with list2env. Use lapply to setNames of the files by taking all the common parts of their names out. Read files with read.delim (can only use defaults = header = TRUE, sep = "\t"). Files will be named zr2096_N

filesToImport <- list.files(pattern = "*header")
list2env(lapply(setNames(filesToImport,
                         make.names(gsub("_s1_R1_val_1_bismark_bt2_pe.deduplicated.bismark.cov_5x.bedgraph.annotated-percentMeth.header", "", filesToImport))),
                read.delim),
         envir = .GlobalEnv)
```

The code allowed me to import 10 files at once and condense the file names (let's be real they were informative but super long and annoying). I removed extraneous columns for each sample.

### Calculating median percent methylation

This step was pretty straightforward! I looked at [Hollie's code](https://github.com/hputnam/Geoduck_Meth/blob/master/RAnalysis/Scripts/methylation_analysis.R) and found she used `aggregate`. I did too:

```{r}
#Calculate median percent methylation by gene for each sample
zr2096_1_PercentMeth <- aggregate(percentMeth ~ geneID, data = zr2096_1, FUN = median)
zr2096_2_PercentMeth <- aggregate(percentMeth ~ geneID, data = zr2096_2, FUN = median)
zr2096_3_PercentMeth <- aggregate(percentMeth ~ geneID, data = zr2096_3, FUN = median)
zr2096_4_PercentMeth <- aggregate(percentMeth ~ geneID, data = zr2096_4, FUN = median)
zr2096_5_PercentMeth <- aggregate(percentMeth ~ geneID, data = zr2096_5, FUN = median)
zr2096_6_PercentMeth <- aggregate(percentMeth ~ geneID, data = zr2096_6, FUN = median)
zr2096_7_PercentMeth <- aggregate(percentMeth ~ geneID, data = zr2096_7, FUN = median)
zr2096_8_PercentMeth <- aggregate(percentMeth ~ geneID, data = zr2096_8, FUN = median)
zr2096_9_PercentMeth <- aggregate(percentMeth ~ geneID, data = zr2096_9, FUN = median)
zr2096_10_PercentMeth <- aggregate(percentMeth ~ geneID, data = zr2096_10, FUN = median)
head(zr2096_1_PercentMeth) #Confirm percent methylation calculation
```

The resultant data frames had `geneID` and `percentMeth`. It was annoying that 1) I can't figure out a way to loop through this command for all 10 samples and 2( all of my gene annotations were gone, but oh well. I guess I can add that in later.

The not-so-straightforward part: making sure every gene is listed in both dataframes. I realized that each dataframe had a different number of lines, or gene IDs. I used a series of `merge` commands to ensure that I had all gene IDs common between all 10 samples. Again, frustrating that I can't do this in a loop:

```{r}
#Merge sample percent methylation information so that all geneIDs have data
fullPercentMeth <- merge(x = zr2096_1_PercentMeth, y = zr2096_2_PercentMeth, by = "geneID")
fullPercentMeth <- merge(x = fullPercentMeth, y = zr2096_3_PercentMeth, by = "geneID")
fullPercentMeth <- merge(x = fullPercentMeth, y = zr2096_4_PercentMeth, by = "geneID")
fullPercentMeth <- merge(x = fullPercentMeth, y = zr2096_5_PercentMeth, by = "geneID")
fullPercentMeth <- merge(x = fullPercentMeth, y = zr2096_6_PercentMeth, by = "geneID")
fullPercentMeth <- merge(x = fullPercentMeth, y = zr2096_7_PercentMeth, by = "geneID")
fullPercentMeth <- merge(x = fullPercentMeth, y = zr2096_8_PercentMeth, by = "geneID")
fullPercentMeth <- merge(x = fullPercentMeth, y = zr2096_9_PercentMeth, by = "geneID")
fullPercentMeth <- merge(x = fullPercentMeth, y = zr2096_10_PercentMeth, by = "geneID")
```

There are 11,622 genes with data for all 10 samples, meaning I shouldn't have any missing data for my statistical tests. I subsetted the gene ID and each sample's percent methylation columns to create individual dataframes for each sample. After changing column names, I added the sample number and treatment in different columns. Finally, I used `rbind` to create a dataframe with `geneID`, `percentMeth`, `sampleNumber`, and `treatment` information for all 10 samples. I saved the file [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-22-Gene-Median-Percent-Methylation-By-Sample.csv).

### Statistical testing

Liew et al. checked normality and variance assumptions before performing a t-test. Since I did MBD-BSseq, which biases towards highly methylated regions, I'm pretty confident my data will not meet a normality assumption.

#### Choosing a method

My raw data is most definitely not normal if I looka t a histogram:

![Screen Shot 2019-08-23 at 12 09 20 PM](https://user-images.githubusercontent.com/22335838/63617643-d759c500-c59e-11e9-8798-4647aabab23c.png)

To deal with the left-skewness, I applied separate log, square root, and cube root transformations to see if any of them look better:

![Screen Shot 2019-08-23 at 12 10 14 PM](https://user-images.githubusercontent.com/22335838/63617693-f7898400-c59e-11e9-83c0-e519c57299d6.png)

![Screen Shot 2019-08-23 at 12 10 23 PM](https://user-images.githubusercontent.com/22335838/63617708-fce6ce80-c59e-11e9-956a-e21da5190575.png)

![Screen Shot 2019-08-23 at 12 11 27 PM](https://user-images.githubusercontent.com/22335838/63617758-230c6e80-c59f-11e9-8150-2f90d00f3319.png)

While the log transformation looks the best, it is still left-skewed. I decided to use the non-parametric Kruskall-Wallis test instead of a t-test. I chose Kruskall-Wallis over Mann-Whitney because I do not have paired data.

#### Conducting the statistical test

I decided to use `kruskal.test` in R to conduct the Kruskal-Wallis test. I took a quick peek at the example:

```
## Hollander & Wolfe (1973), 116.
## Mucociliary efficiency from the rate of removal of dust in normal
##  subjects, subjects with obstructive airway disease, and subjects
##  with asbestosis.
x <- c(2.9, 3.0, 2.5, 2.6, 3.2) # normal subjects
y <- c(3.8, 2.7, 4.0, 2.4)      # with obstructive airway disease
z <- c(2.8, 3.4, 3.7, 2.2, 2.0) # with asbestosis
kruskal.test(list(x, y, z))
```

I needed to isolate my ambient and treatment samples in different dataframes to follow this method. After sorting the dataframe by `geneID` then `treatment`, I used `subset` to create dataframes for each treatment.

For `kruskal.test`, I thought the best approach would be to loop through the dataset by `geneID`, extract the chi-squared statistic, df, and p-value from each test, and save the output in a new dataframe:

```{r}
#For each unique geneID
#Conduct a KW test for a set of 5 rows from each dataframe (5 ambient and 5 treatment median % methylation values for the same gene)
#Store the chi-squared statistic in the ith row
#Store the df in the ith row
#Store the p.value in the ith row
for (i in 1:length(KWTestResults$geneID)) {
  temp <- kruskal.test(list(ambientPercentMethFull$percentMeth[((5*i)-4):(5*i)], treatmentPercentMethFull$percentMeth[((5*i)-4):(5*i)]))
  KWTestResults$chi.squared[i] <- temp$statistic
  KWTestResults$df[i] <- temp$parameter
  KWTestResults$p.value[i] <- temp$p.value
}
```

I then used `adjust.p` to apply a Benjamini-Hochberg correction to my p-values to control for a false discovery rate. When I looked at the range of my adjusted p-values, none of them were significant! I'm a little bummed, but it's not surprising seeing how I did over 10,000 comparisons. I saved the output of my Kruskal-Wallis tests [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-DMG-KW-Test-Results.csv).

#### mRNA positions with `intersectBed`

After I looked at my statistical test results, I had a mild panic that the mRNA start and end positions I used to annotate coverage files biased what loci were included. I thought there could be a chance `intersectBed` matched CpG loci with gene and mRNA positions. I removed the mRNA information from my annotations and reran `intersectBed`. I counted the lines of the resultant sorted and joined files. They did not differ from the counts I previously had, so I put that mild panic to rest and reverted to including mRNA positions in my files.

### Filtering genes before statistical testing

Liew et al. filtered data before they conducted their statistical tests by ony retaining genes with at least five methylated positions. Even though keeping genes with no missing data pared down my list by ~10,000 genes, I figured it couldn't hurt to replicate their efforts. I don't really understand why they chose genes with at least five methylated positions, sice it seems like that would bias the analysis towards heavily methylated genes.

I thought an easier, and perhaps more meaningful way to pare down my list and see if it had an effect was to only look at genes with DML. I extracted unique `geneID` from my DML list and merged that with my `kruskal.test` results. I then applied a Benjamini-Hochberg correction to the p-values from the 481 genes. None of the adjusted p-values for genes with DML were significant. My paper will focus solely on the DML.

Speaking of which...time to WRITE!

### Going forward

1. Update methods and results
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
