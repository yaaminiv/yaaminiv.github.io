---
layout: post
comments: true
title: Gigas and Virginica Comparison Part 2
tags: manchester gigas-broodstock virginica methylation-islands mox bedtools IGV
---

## *C. gigas* vs. *C. virginica*: general methylation landscapes

Now that I've [tested methylation island analysis](https://yaaminiv.github.io/Gigas-and-Virginica-Comparison/), I want to finalize my *C. virginica* and *C. gigas* MI tracks and look at the location of MI in the genome and in relation to DML.

### *C. virginica* MI

#### Finalizing track parameters

Based on [this issue](https://github.com/RobertsLab/resources/issues/834), I tested out 500 bp windows and a 0.02 mCpG fraction with either a 25 bp or 50 bp step size.

**Table 1**. Testing 500 bp window sizes and different step sizes

| **Initial Window Size (bp)** | **mCpG Fraction** | **Step Size (bp) ** | **Number of MI** | **Max mCpG in MI** | **Min mCpG in MI** |
|:----------------------------:|:-----------------:|:-------------------:|:----------------:|:------------------:|:------------------:|
|              500             |        0.02       |          25         |       64795      |        24777       |         10         |
|              500             |        0.02       |          50         |       63483      |        24777       |         10         |

I then visualized the tracks in IGV! The two 500 bp tracks are pretty comparable, but there are definitely places without MI in the 500 bp tracks that were in the 200 and 300 bp tracks.

<img width="1156" alt="Screen Shot 2020-02-08 at 3 19 06 PM" src="https://user-images.githubusercontent.com/22335838/74093547-19f95600-4a88-11ea-839e-e0d6357f15c3.png">

<img width="1150" alt="Screen Shot 2020-02-08 at 3 19 27 PM" src="https://user-images.githubusercontent.com/22335838/74093548-1bc31980-4a88-11ea-9555-81c12444d79d.png">

<img width="1155" alt="Screen Shot 2020-02-08 at 3 20 08 PM" src="https://user-images.githubusercontent.com/22335838/74093549-1d8cdd00-4a88-11ea-8c90-c7b36dfca5b8.png">

<img width="1150" alt="Screen Shot 2020-02-08 at 3 20 54 PM" src="https://user-images.githubusercontent.com/22335838/74093550-1f56a080-4a88-11ea-8ca6-5ff1048a80fe.png">

<img width="1151" alt="Screen Shot 2020-02-08 at 3 24 32 PM" src="https://user-images.githubusercontent.com/22335838/74093551-21206400-4a88-11ea-9aa2-4e7649185921.png">

**Figures 1-5**. MI tracks with 200, 300, and 500 bp windows and 0.02 mCpG fractions against all 5x CpGs, methylated CpGs, and mRNA.

When I asked for feedback on the tracks in [this issue](https://github.com/RobertsLab/resources/issues/834), Steven pointed out that some of the MI were in fact less than 500 bp.

<img width="1156" alt="Screen Shot 2020-02-08 at 4 16 08 PM" src="https://user-images.githubusercontent.com/22335838/74093960-6b0c4880-4a8e-11ea-8d16-af3c5d3bedc3.png">

**Figure 6**. MI that is at least 500 bp.

<img width="1157" alt="Screen Shot 2020-02-08 at 4 19 57 PM" src="https://user-images.githubusercontent.com/22335838/74094050-c0952500-4a8f-11ea-8627-070d829f7b3c.png">

**Figure 7**. MI that is less than 500 bp.

I don't undertand why the script still created MI that were less than 500 bp! I decided to manually filter by length using this code:

```
!awk '{if ($3-$2 >= 500) { print $1"\t"$2"\t"$3"\t"$4}}' 2020-02-06-Methylation-Islands-500_0.02_25.tab \
> 2020-02-06-Methylation-Islands-500_0.02_25-filtered.tab
```

Almost 30,000 of the MI I identified did not meet this size threshold!

**Tables 2**. MI characteristics after filtering by size. No MI should be less than 500 bp.

| **Initial Window Size (bp)** | **mCpG Fraction** | **Step Size (bp)** | **Number of MI** | **Number of MI after Filtering** | **Max mCpG in MI** | **Min mCpG in MI** |
|:----------------------------:|:-----------------:|:------------------:|:----------------:|:--------------------------------:|:------------------:|:------------------:|
|              500             |        0.02       |         25         |       64795      |               36060              |        24777       |         11         |
|              500             |        0.02       |         50         |       63483      |               37063              |        24777       |         11         |

Based on the feedback, I decided to move forward with the filtered 500 bp windows, 0.02 mCpG fraction, and 50 bp window sizes.

#### Location in the genome

In [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb), I characterized the location of genome feature tracks and DML with respect to MI.

**Table 3**. MI overlaps with genome feature tracks and DML.

|      **Feature**      | **Number of Overlaps with MI** |
|:---------------------:|:------------------------------:|
|         Exons         |             240133             |
|        Introns        |              92472             |
|         Genes         |              15009             |
|          mRNA         |              29483             |
| Transposable Elements |             107926             |
|   Putative Promoters  |               8846             |
|          DML          |               537              |
|  Hypermethylated DML  |               283              |
|   Hypomethylated DML  |               254              |

Most of my DML (89.7%) were in my MI! Additionally, 32.8% of exons, 29.2% of introns, 49.0% of mRNA, and 38.6% of genes overlapped with MI. In contrast, only 15.6% of transposable elements and 14.7% of promoters overlapped with MI.

To visualize the MI in the *C. virginica* genome, I decided to [create boxplots similar to Jeong et al.](https://academic.oup.com/view-large/figure/122844793/evy203f2.tif). I started by importing MI-feature track overlaps in [this R Markdown script](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2020-02-11-Characterizing-Methylation-Islands.Rmd). For each file, I calculated the length of the MI, then divided the base piars that overlapped by the MI length. This gave me an overlap percentage that I used in boxplots! I created a base plot, but decided not to spend too much time on it now and return to it for my reviewer comments in a few days. The code I used for calculations and to make the plot was important than actually making the plot pretty and saving it.

### *C. gigas* MI

Now that I generated *C. virginic* MI, it's time to move onto *C. gigas* MI.

#### General gonad methylation landscape

To identify methylated CpGs in the *C. gigas* genome, I need to characterize the general methylation landscape. Although Claire and Mac did this previously with ctendia, I wanted to go through this process with my gonad samples in the event that somatic and gonad tissue had variations in methylation landscapes. Additionally, Claire and Mac identified mCpGs using 5x data, and all of my analyses were done with 10x data.

##### Rerunning `bismark`

When I went to access my coverage files on `ganent`, I found that I had overwritten these files! the folder with `bismark` output no longer existed...which meant I had to rerun `bismark`.  :sob:

Based on previous lab notebook posts detailing my struggle running `mox` last time, I made a quick change to [this script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/scripts/2019-09-12-Bismark.sh) so my deduplication would run only on my sample files, and not proceed iteratively through a bunch of other files. I used the following code to transfer my files over to `mox`:

```
rsync --archive --progress --verbose yaamini@172.25.149.226:/Users/yaamini/Documents/project-gigas-oa-meth/analyses/2019-08-30-Bismark-Parameter-Testing/*fastq.gz . #Transfer sample files to scrubbed/yaamini/data/2019-09-03-Trimmed-Files
rsync --archive --progress --verbose yaamini@172.25.149.226:/Users/yaamini/Documents/project-gigas-oa-meth/analyses/2019-08-30-Bismark-Parameter-Testing/Crassostrea_gigas.oyster_v9.dna_sm.toplevel . #Transfer bisulfite converted genome to scrubbed/yaamini/data/2019-09-03-Bismark-Inputs
rsync --archive --progress --verbose yaamini@172.25.149.226:/Users/yaamini/Documents/project-gigas-oa-meth/scripts/2019-09-12-Bismark.sh . #Transfer script to srlab/yaaminiv
```

It took me a few failed runs to realize I transfered files to the incorrect directories or didn't transfer the  files at all. I definitely opted for a more complicated file structure in my script but if I would have perused the script more carefully beforehand it wouldn't have been a problem. A good reminder for me to really read the script! Once every file was where I said it would be, I ran my script and hoped for the best. I used the following code to check up on progress:

```
cat slurm-1786754.out
```

Even though I set up the script to run through all of `bismark`, I thankfully only need the coverage files to proceed with my analyses.

One day and six hours later, my `bismark` run completed! I used this code to transfer the files off `mox`:

```
rsync --archive --progress --verbose /gscratch/scrubbed/yaamini/analyses/Gigas-WGBS/2019-09-11-Gigas-Bismark/* yaamini@172.25.149.226:/Volumes/web/spartina/2019-09-03-project-gigas-oa-meth/analyses/2019-09-11-Bismark #Transfer files to gannet
```

##### Characterizing the methylation landscape

While my files were processing, I set up [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2020-02-11-Characterizing-CpG-Methylation.ipynb) to create a master file and characterize methylation. Using the `head` of two *C. virginica* coverage files, I used a series of commands to merge the information in both coverage files. I then used these commands on the coverage files I got from `bismark`. Yay efficiency!

Before I created a master coverage file, I quickly created 5x and 10x sample bedgraphs in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-13-Generating-Coverage-Tracks.ipynb) since the output wasn't on `gannet` or in my repository...RIP.

Alright, BACK TO THE GOOD STUFF. First, I created a new column in each coverage file that combined chromosome and start bp information. I will need to use this information to sort files and join them.

```
for f in ../2019-09-13-IGV-Verification/*cov
do
  awk '{print $1"-"$2"\t"$1"\t"$2"\t"$3"\t"$4"\t"$5"\t"$6}' ${f} \
  | sort -k1,1 \
  > ${f}.sorted
done
```

Next, I joined the two files by the chr-start column. In order to simulate an outer join, I specified that unmatched lines in each file should be included in the output using the `-a` argument. The only time a locus would be in one file and not another is when there are no reads for that locus in one sample. To clean up the file, I replaced any empty fields (methylation percentage, count methylated, count unmethylated) with 0 using `-e 0`. I then specified which columns I wanted printed in the output using `-o`. Before saving the output, I used `tr` to translate the "-" in the first column to a tab delimiter. This way, I did not have to deal with any redundancy in the locus start positions from each files or figure out how to merge the columns while dealing with zeroes.

```
#Join the first column in the first file with the first column in the second file
#The files are tab delimited, and the output should also be tab delimited (-t $'\t')
#Print unpairable lines in file 1 (-a1) and 2 (-a2) to simulate an outer join. Replace empty fields with 0 (-e) and only print the following fields (-o)
#Convert - to \t to uncouple the chromosome and start position

!join -1 1 -2 1 \
-t $'\t' \
-a1 -a2 -e 0 -o '0,1.5,1.6,1.7,2.5,2.6,2.7' \
FILE \
FILE \
| tr '-' "\t" \
> cgigas_gonad-10x_raw.cov
```

Finally, I consolidated the methylated and unmethylated read counts from each file, summed the total number of reads at each locus, and calculated the revised methylation percentage.

```
#Calculate total count methylated and unmethylated for each locus
#Sum number of reads at each locus
#Calculating revised methylation percentage for each locus
#Multiply percentages by 100

!awk '{ print $1"\t"$2"\t"$2"\t"$4+$7"\t"$5+$8}' cgigas_gonad-10x_raw.cov \
| awk '{ print $1"\t"$2"\t"$3"\t"$4+$5"\t"$4"\t"$5}' \
| awk '{ print $1"\t"$2"\t"$3"\t"$5/$4"\t"$5"\t"$6}' \
| awk '{ print $1"\t"$2"\t"$3"\t"$4*100"\t"$5"\t"$6}' \
> cgigas_gonad-10x_concat.cov
```

I have data for 12,728,174 CpGs at 10x read depth. After reducing the dataset to loci with at least 10x coverage, I identified methylated (≥ 50%), sparsely methylated (≥ 10%, < 50%), and unmethylated (< 10%) CpG loci in [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2020-02-11-Characterizing-CpG-Methylation.ipynb). I also characterized the location of these loci in relation to various genome features.

**Table 1**. General *C. gigas* gonad methylation landscape.

|      **Feature**      	| **All 10x Loci** 	| **Methylated Loci** 	| **Sparsely Methylated Loci** 	| **Unmethylated Loci** 	|
|:---------------------:	|:----------------:	|:-------------------:	|:----------------------------:	|:---------------------:	|
|     Number of CpGs    	|     12728174     	|       1677041       	|            2267700           	|        8783433        	|
|         Exons         	|      1803226     	|        559020       	|            199748            	|        1044458        	|
|        Introns        	|      3834047     	|        706876       	|            768464            	|        2358707        	|
|         Genes         	|      5637273     	|       1265896       	|            968212            	|        3403165        	|
| Transposable Elements 	|      612466      	|        41024        	|            161419            	|         410023        	|
|   Putative Promoters  	|      803342      	|        59704        	|            118610            	|         625028        	|
|         Other         	|      5998040     	|        354355       	|            1093226           	|        4550459        	|

#### Creating the MI track

In [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb) I generated the methylation island track using the same parameters I used for *C. virginica*.

**Tables 2**. MI characteristics after filtering by size. No MI should be less than 500 bp.

| **Initial Window Size (bp)** 	| **mCpG Fraction** 	| *Step Size** 	| **Number of MI** 	| **Number of MI after Filtering** 	| **Max mCpG in MI** 	| **Min mCpG in MI** 	|
|:----------------------------:	|:-----------------:	|:------------:	|:----------------:	|:--------------------------------:	|:------------------:	|:-----------------:	|
|              500             	|        0.02       	|      50      	|       38443      	|               23173              	|        3298        	|         11        	|

I then visualized the MI in [this IGV session](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-13-IGV-Verification/2020-02-11-Methylation-Island-Visualization.xml). The MI in *C. gigas* are much more sparse, which is different than *C. virginica*.

<img width="1155" alt="Screen Shot 2020-02-13 at 2 46 36 AM" src="https://user-images.githubusercontent.com/22335838/74427207-7bfce700-4e0b-11ea-8a61-11d401239966.png">

<img width="1156" alt="Screen Shot 2020-02-13 at 2 46 59 AM" src="https://user-images.githubusercontent.com/22335838/74427211-7ef7d780-4e0b-11ea-90cd-31cb6307df4b.png">

<img width="1152" alt="Screen Shot 2020-02-13 at 2 48 38 AM" src="https://user-images.githubusercontent.com/22335838/74427214-80290480-4e0b-11ea-916b-2bc157eb37eb.png">

<img width="1152" alt="Screen Shot 2020-02-13 at 2 49 12 AM" src="https://user-images.githubusercontent.com/22335838/74427218-815a3180-4e0b-11ea-9c78-22adeb742ebd.png">

**Figures 1-4**. Methylation islands in the *C. gigas* gonad

#### Location in the genome

In [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb), I used `bedtools` to characterize the overlaps between *C. gigas* MI and genome feature tracks! I was able to run the code and get counts, but the output format for the Intron-MI and Promoter-MI files looked wonky:

<img width="360" alt="Screen Shot 2020-02-13 at 9 20 36 AM" src="https://user-images.githubusercontent.com/22335838/74460675-1a0ba400-4e42-11ea-94fe-5343939cef11.png">

**Figure 5**. Weird file formatting

I posted [this issue](https://github.com/RobertsLab/resources/issues/840) to figure out why my output looked this way, since I needed proper output to correctly count overlaps and create MI overlap figures.

**Table 3**. Overlaps between methylation islands and various genome features.

|      **Feature**      	| **Number of Overlaps** 	|
|:---------------------:	|:----------------------:	|
|         Exons         	|      49607 (25.2%)     	|
|        Introns        	|      51980 (29.5%)     	|
|         Genes         	|      8761 (31.3%)      	|
| Transposable Elements 	|       5868 (4.9%)      	|
|   Putative Promoters  	|       2746 (9.8%)      	|
|          DML          	|       384 (61.1%)      	|

The sparse MI definitely translates to fewer overlaps between DML, but similar overlaps between MI, exons, introns, and genes in both species. Since I don't trust the intron output, I'm unsure if the actual number here is correct, but in the offchance it is, I included it. Transposable elements and promoters in the *C. virginica* genome had more overlaps with MI than those same features in the *C. gigas* genome.

In [this R Markdown file](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2020-02-11-Characterizing-CpG-Methylation/2020-02-11-Characterizing-Methylation-Islands.Rmd) I created a standalone *C. gigas* MI overlap boxplot as well as a grouped barplot with *C. virginica* and *C. gigas* data. Again, I didn't pay too much attention to the standalone plot since I figured I'd come back and use it again when I had more data. Even though my intron overlap data formatting issue prevented me from creating a boxplot with *C. gigas* intron information, I created a grouped barplot for *C. virginica* and *C. gigas* data. I learned you could use `at` to set where on the x-axis to place individual boxplots to artificially group them!

<img width="820" alt="Screen Shot 2020-02-13 at 9 43 38 AM" src="https://user-images.githubusercontent.com/22335838/74462587-555ba200-4e45-11ea-9e60-07095b4e18bb.png">

**Figure 6**. Exon, intron, and transposable element overlaps. *C. gigas* intron-MI overlap data is missing due to output formatting issues.

I think some of the differences are a little clearer in this boxplot. Time to move on from this analysis!

### Going forward

2. Fix *C. gigas* intron-MI and promoter-MI formatting issues
3. Update *C. gigas* MI overlap counts and grouped barplot
3. Compare *C. gigas* and *C. virginica* DML and GOSlim terms
3. Draft poster for ASLO and get feedback
4. Finalize ASLO poster and send for printing by Thursday to get the ASLO discount!

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
