---
layout: post
comments: true
title: DML Analysis Part 29 
tags: virginica MBDSeq
---

## Describing general methylation trends

A.K.A. that time I used a lot of bash for loops and `awk` commands while understanding what I was doing.

### Total DNA recovered

I noticed Mac's 2013 paper included how much of the original input DNA was recovered from MBD procedures. I figured it was a good thing to include in my paper too! In [this spreadsheet](), I documented the µg of DNA input I used from each sample. In total, I used 37.28 µg. After MBD, [the samples](https://docs.google.com/spreadsheets/d/1p13VsqTMrynvWIfokPVAwqaL78_GA6QtTZ0NAgLVfSI/edit#gid=236614896) were resuspended in 25 µL of buffer. The total yield was 1.42515 µg. This meant that only 3.82% of DNA was recovered after MBD.

### Counting reads

One important part of characterizing general methylation trends is to explain how many reads were used for analysis. In [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-03-17-Counting-Reads.ipynb), I downloaded FastQC and `bismark` reports. I originally thought I should download the full trimmed and untrimmed files, but in [this issue](https://github.com/RobertsLab/resources/issues/616) Steven pointed out that all of that information would be stored in summary reports.

I used a similar series of commands for each report to obtain read counts:

1. Determine the for loop selects the files I want

`````
%%bash
for f in *zip
do
    echo ${f}
done
`````

2. Unzip files if needed

`````
%%bash
for f in *zip
do
    unzip ${f}
done
`````

3. Isolate the number of reads from each report and concatenate in a new file

`````
%%bash
for f in *fastqc
do
    grep "Total Sequences *" ${f}/fastqc_data.txt \
    >> 2019-03-17-Untrimmed-Read-Counts.txt
done
`````

I counted [untrimmed reads](https://github.com/fish546-2018/yaamini-virginica/blob/master/data/2019-03-17-Counting-Reads/2019-03-17-Untrimmed-Reads/2019-03-17-Untrimmed-Read-Counts.txt), [trimmed reads](https://github.com/fish546-2018/yaamini-virginica/blob/master/data/2019-03-17-Counting-Reads/2019-03-17-FastQC-Reports/2019-03-17-Trimmed-Read-Counts.txt), and [reads that were not mapped to the genome](https://github.com/fish546-2018/yaamini-virginica/blob/master/data/2019-03-17-Counting-Reads/2019-03-17-Mapped-Reads/2019-03-17-Unmapped-Read-Counts.txt). For the unmapped reads, I subtracted the number of unmapped reads from the number of trimmed reads to obtain the number of reads mapped to the genome. There are 279,681,264 untrimmed reads sequence reads. Of 275,914,272 trimmed paired-end reads, 190,675,298 reads were mapped.

### Identify methylated CpG loci

I want to describe general methylation trends, irrespective of pCO<sub>2</sub> treatment in my *C. virginica* gonad data. Claire and Mac both had sections in their papers where determined if a CpG locus was methylated or not. From Mac's 2013 PeerJ paper:

> A CpG locus was considered methylated if at least half of the reads remained unconverted after bisulfite treatment.

They characterized this using `methratio.py` in `BSMAP`, but Steven pointed out I could use `.cov` files [in this issue](https://github.com/RobertsLab/resources/issues/615). In [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-03-18-Characterizing-CpG-Methylation.ipynb), I first identified loci with 5x coverage for each sample by using this `awk` command in a loop:

`````
awk '{print $1, $2-1, $2, $4, $5+$6}' ${f} | awk '{if ($5 >= 5) { print $1, $2-1, $2, $4 }}'
`````

The coverage file has the following columns: chromosome, start, end, methylation percentage, count methylated, and count unmethylated. In the above command, I correct the start and stop positions (`$2-1, $2`), and add the count methylated and unmethylated (`$5+$6`) in each file `${f}`. If the new fifth column, total count methylated and unmethylated was greater than 5 (`$5 >= 5`), it meant that I had 5x coverage for that locus. I could then redirect that information to a new file.

The next step was to concatenate all loci with 5x coverage across my five control samples. In this step, I'm essentially mimicing `methylKit unite`. I used a series of `join` commands for this task, then used `awk` to clean up the columns.

![Screen Shot 2019-03-18 at 9 15 05 PM](https://user-images.githubusercontent.com/22335838/54579892-d656b300-49c2-11e9-947b-30967f1b7aae.png)

![Screen Shot 2019-03-18 at 9 15 20 PM](https://user-images.githubusercontent.com/22335838/54579893-d656b300-49c2-11e9-926d-788bbfc0ea29.png)

![Screen Shot 2019-03-18 at 9 15 31 PM](https://user-images.githubusercontent.com/22335838/54579894-d656b300-49c2-11e9-9566-f2bb113a6ca5.png)

The final file with 5x loci across all samples can be found [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2019-03-18-Control-5x-CpG-Loci.bedgraph). By summing the percent methylation information from each sample for each locus, I could characterize its methylation status and separate loci and save these loci in a new file:

- [Methylated (50% methylation and above; sum ≥ 250)](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2019-03-18-Control-5x-CpG-Loci-Methylated.bedgraph)
- [Sparsely methylated (10-50% methylated; 50 < sum < 250)](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2019-03-18-Control-5x-CpG-Loci-Sparsely-Methylated.bedgraph)
- [Ummethylated (0% methylation; sum = 0)](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2019-03-18-Control-5x-CpG-Loci-Unmethylated.bedgraph)

Using `wc -l` to count the number of loci in each file, I had 63,827 total loci across all samples with at least 5x coverage. This is very different from the 14,458,703 CpG motifs across the *C. virginica* genome. Of the loci with at least 5x coverage, 60,552 were methylated, 2,796 were sparsely methylated, and 479 were unmethylated.

### Characterize location of methylated CpG

The last step to describe methylated CpG is to figure out where the 60,552 loci are in the genome! I set up `intersectBed` in [my Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-03-18-Characterizing-CpG-Methylation.ipynb) to find overlaps between my methylated loci and exons, introns, mRNA coding regions, transposable elements, and putative promoter regions. When creating my BEDfile, I needed to specify tab delimiters (`\t`) to get `intersectBed` to recognize the file:

`````
awk '{print $1"\t"$2"\t"$3}' 2019-03-18-Control-5x-CpG-Loci-Methylated.bedgraph \
> 2019-03-18-Control-5x-CpG-Loci-Methylated.bed
`````

Similar to the DML, methylated CpG were primarily located in genic regions (56,055 CpG; 92.6%), with 3,083 unique genes represented. More CpG (40,127; 66.27%) overlaped with exons than with introns (17,510; 28.92%). I found 4,687 CpG (7.74%) in transposable elements and 1,221 CpG (2.02%) in putative promoter regions 1 kb upstream of transcription start sites. There were 2278 methylated loci (3.76%) that did not overlap with exons, introns, transposable elements, or putative promoters. All lists of overlapping loci can be found in [this folder](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2019-03-18-Characterizing-CpG-Methylation).

### TL;DR

I had lots of `awk` and bash coding issues today BUT I FIGURED MOST OF THEM OUT MYSELF! Just in time to go on vacation and lose all of my hard-earned coding prowess.

### Going forward

1. Conduct a proportion test for DML locations
2. Update paper methods and results
3. Work through gene-level analysis
4. Figure out what's going on with DMR
5. Figure out what's going on with the gene background

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
