---
layout: post
comments: true
title: MethCompare Part 23
tags: MethCompare bedtools DMG
---

## Calculating median percent methylation

We're wrapping up our analyses. Hollie is looking at gene methylation by method and asked me to calculate median methylation by gene for each sample, since I [did that before for *C. virginica*](https://yaaminiv.github.io/DMG-Analysis-Part2/). I set out to do just that in [this script](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Median-Methylation-Calculations.Rmd). I started by testing code on *M. capitata* samples, then used the finalized code for *P. acuta* as well. 

Looking at my files on `gannet`, I realized I never transferred the intermediate genonme feature overlap files! I started rerunning [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x.ipynb) to get the gene-sample overlap files. Even after I generated those files, I was missing percent methylation information. I decided to include percent methylation information in the intermediate BEDfiles before using `intersectBED`. After using `intersectBed`, I had chromosome, start, stop, and percent methylation information for each sample.

I returned to my R Markdown script to import the overlap files. Again, I realized I was missing something! I needed to associate CpG loci with genes. I used `intersectBed` to match CpGs with genes:

```{bash}
#For each sample-gene overlap file (ends in *bedgraph.bed-mcGenes)
#Use intersectBed to find where loci and genes intersect, allowing loci to be mapped to genes
#wb: Print all lines in the second file
#a: sample-gene overlap file
#b: gene track
#Save output in a new file that has the same base name and ends in -geneID
for f in ../analyses/Characterizing-CpG-Methylation-5x/Mcap/*bedgraph.bed-mcGenes
do
  /usr/local/bin/intersectBed \
  -wb \
  -a ${f} \
  -b ../genome-feature-files/Mcap.GFFannotation.gene.gff \
  > ${f}-geneID
done
```

I then took the files and moved them into a Median-Methylation-Calculations subdirectory. Next, I needed to import the nine files for each species into R. I used code I learned about when working with *C. virginica* to import all the files at once:

```{r}
setwd("../analyses/Characterizing-CpG-Methylation-5x/Mcap/Median-Methylation-Calculations") #Set working directory within the notebook chunk for list.files to find the necessary files
filesToImport <- list.files(pattern = "*geneID") #Create a file list for all 9 files to import. Only import overlaps for full samples (not divided by methylation status)
list2env(lapply(setNames(filesToImport,
                         make.names(gsub("_R1_001_val_1_bismark_bt2_pe._5x.bedgraph.bed-mcGenes-geneID", "", filesToImport))),
                read.delim, header = FALSE),
         envir = .GlobalEnv) #Import files to the .GlobalEnv with list2env. Use lapply to setNames of the files by taking all the common parts of their names out. Read files with read.delim and include header = FALSE. Files will be named Meth#
head(Meth10) #Confirm import
```

My next steps were simple: remove redundant columns, add column names, and use `aggregate` to calculate median percent methylation for each gene ID. That would amount to three lines of code for each sample, but I didn't want to write an individual block of code for each sample! Based on [this Stack Overflow post](https://stackoverflow.com/questions/18375969/rename-columns-in-multiple-dataframes-r), I created a vector of dataframes, then pulled the dataframes out of the vector in a loop to format the files:

```{r}
samplesMcap <- c("Meth10",
                 "Meth11",
                 "Meth12",
                 "Meth13",
                 "Meth14",
                 "Meth15",
                 "Meth16",
                 "Meth17",
                 "Meth18") #Create a vector of sample names
```

```{r}
for(sample in samplesMcap) { #For each sample listed in samplesMcap
  sample.tmp <- get(sample) #Extract sample based on vector contents
  sample.tmp <- sample.tmp[,-c(5:12)] #Remove extraneous columns
  colnames(sample.tmp) <- c("chr", "start", "stop", "percentMeth", "geneID") #Rename columns
  assign(sample, sample.tmp) #Replace sample with edited sample.tmp contents
}
head(Meth17) #Confirm formatting changes
```

Finally, I used similar loops to calculate median percent methylation by gene ID and write out tab-delimited files for each sample:

```{r}
for(sample in samplesMcap) { #For each sample listed in samplesMcap
  sample.tmp <- get(sample) #Extract sample based on vector contents
  sample.tmp <- aggregate(percentMeth ~ geneID, data = sample.tmp, FUN = median) #Use aggregate to group geneID and calculate median percent methylation
  assign(sample, sample.tmp) #Replace sample with edited sample.tmp contents
}
head(Meth17) #Confirm median methylation calculation
```

```{r}
for (i in 1:length(samplesMcap)) { #For each sample listed in samplesMcap
  sample <- get(samplesMcap[i]) #Extract sample based on vector contents
  fileName <- paste("../analyses/Characterizing-CpG-Methylation-5x/Mcap/Median-Methylation-Calculations/", samplesMcap[i], "-Median-Methylation", ".txt", sep = "") #Assign filename for each sample
  write.table(sample, file = fileName, sep = "\t", row.names = FALSE, col.names = TRUE) #Write out files into the Median-Methylation-Calculations subdirectory
}
```

The trick of putting dataframes in a vector to run the same commands on them is super useful. Definitely using that in the future. I posted the links to the *M. capitata* and *P. acuta* subdirectories in [this issue](https://github.com/hputnam/Meth_Compare/issues/86) for Hollie to use.

Aside from calculating methylation, I also modified figures based on suggestions from Hollie and updated the methods section. Not much left to work on (hopefully)!

### Going forward

1. Update results
2. Look into program for mCpG identification

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
  
