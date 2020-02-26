---
layout: post
comments: true
title: DML Analysis Part 45
tags: virginica MBDSeq DML methylation-islands
---

## Addressing reviewer comments

I didn't expect reviewer comments a month after I submitted the [*C. virginica* gonad methylation paper](https://www.biorxiv.org/content/10.1101/2020.01.07.897934v1), but here we are. They looked relatively minor, so I figured it was okay if I couldn't get to them until now.

### Additional genomic feature overlaps

One reviewer asked us to consider the location of DML with respect to 5' UTR, coding sequences, and 3' UTR. Since I had plenty of genome feature tracks, I decided to take this one step further and look at UTR, coding sequence, non-coding sequence, lncRNA, and intergenic region with DML, methylated CpGs, and methylation islands (yup, I'm including this now...see below).

I returned to [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-03-18-Characterizing-CpG-Methylation.ipynb) to look at overlaps between methylated CpGs and these other genome feature tracks. I did similar things in [this notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb) for MI, but I moved the code over to the previous notebook because I think it makes more sense to include this analysis in the same notebook where I generated the MI. [This notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb) is where I characterized DML locations.

**Table 1**. Genome feature overlaps with different CpG categories, methylation islands, and DML

|   **Genome Feature**  	| **All 5x CpGs** 	| **Methylated CpGs** 	| **Sparsely Methylated CpGs** 	| **Unmethylated CpGs** 	| **DML** 	|
|:---------------------:	|:---------------:	|:-------------------:	|:----------------------------:	|:---------------------:	|:-------:	|
|         Total         	|     4304257     	|       3181904       	|            481788            	|         640565        	|   598   	|
|         Exons         	|     1366779     	|       1013691       	|            105871            	|         247217        	|   368   	|
|        Introns        	|     1884429     	|       1504791       	|            211143            	|         168495        	|   192   	|
|        Exon UTR       	|      192907     	|        128585       	|             19280            	|         45042         	|    27   	|
|          mRNA         	|     3140744     	|       2437901       	|            303890            	|         398953        	|   549   	|
|    Coding Sequences   	|     1174256     	|        885327       	|             86624            	|         202305        	|   341   	|
|  Non-Coding Sequences 	|     2933517     	|       2164988       	|            375671            	|         392858        	|   230   	|
|         Genes         	|     3255049     	|       2521653       	|            317249            	|         416147        	|   560   	|
|   Putative Promoters  	|      176156     	|        106111       	|             22870            	|         47175         	|    42   	|
| Transposable Elements 	|     1011883     	|        755222       	|            155293            	|         101368        	|    57   	|
|         lncRNA        	|      82671      	|        63588        	|             9337             	|          9746         	|    5    	|
|   Intergenic Regions  	|     1049088     	|        660197       	|            164528            	|         224363        	|    38   	|
|      No Overlaps      	|      603597     	|        372047       	|             84582            	|         146968        	|    21   	|

### Percent methylation of genome features

The biggest task I got for figures was plotting at overall percent methylation for various genome features instead of plotting overlaps. I think it's important to still plot overlaps to show where methylation occurs — something I don't find immediately apparent from pure percent methylation plots. I decided to calculate methylation for each genome feature globally and break it down by treatment. For global methylation, I could use the concatenated 5x CpG track. For each treatment, I can calculate the median percent methylation using the 5 control or 5 treatment samples.

Conceptually, getting these numbers involved a few steps I based off of [my DMG analysis](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-Differentially-Methylated-Gene-Analysis.Rmd):

1. Interect sample bedgraphs with genome feature tracks (promoters, UTRs, exons, introns, transposable elements, intergenic regions)
2. Add percent methylation information
2. Calculate median percent methylation for the entire matrix

I used [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2019-03-19-Characterizing-CpG-Methylation.Rmd) to carry out my plan. Since I did something similar beforehand, I transferred full sample bedgraphs, sample bedgraphs with only positions, and sample bedgraphs with a sorting key I could use to add percent methylation information to processed files. I reformatted the 5x CpG bedgraph to match these requirements. For each genome feature, I ran `intersectBed` with position-only files to find where loci and various genome feature files overlap:

```{bash}
#For each file that ends in posOnly
#Use intersectBed to find where loci and promoters intersect
#wb: Print all lines in the second file
#a: file that ends in posOnly
#b: promoter track
#Only keep specified columns
#Save output in a new file that has the same base name and ends in -UTROverlap
for f in *posOnly
do
  /Users/Shared/bioinformatics/bedtools2/bin/intersectBed \
  -wb \
  -a ${f} \
  -b ../2019-05-13-Generating-Genome-Feature-Tracks/C_virginica-3.0_Gnomon_exonUTR_yrv.bed \
    | awk '{print $1"\t"$2"\t"$3"\t"$5"\t"$6}' \
  > ${f}-UTROverlap
done
```

Then, I needed to add the percent methylation information to these overlaps! I took the overlap files I created and added a sorting key:

```{bash}
#For each file that ends in UTROverlap
#Print the first three columns (chr, start, end) with dashes in between, then the rest of the columns in the file
#Save the file with the base name + .sorted
for f in *UTROverlap
do
  awk '{print $1"-"$2"-"$3"\t"$0}' ${f} \
  | sort -k1,1 \
  > ${f}.sorted
done
```

I used `join` to add the percent methylation information using the sorting key:

```{bash}
#For each file that ends in bedgraph
#Join 2 files using the first column. The files and tab-delimited and the output should also be tab-delimited
#The first file ends with .sorted
#The second file ends with .posOnly-UTROverlap.sorted
#Add .annotated.percentMeth to the base name of the output file
for f in *bedgraph
do
  join -j 1 -t $'\t' \
  ${f}.sorted \
  ${f}.posOnly-UTROverlap.sorted \
  | awk '{print $2"\t"$3"\t"$4"\t"$9"\t"$10"\t"$5}' \
  > ${f}-UTROverlap-percentMeth
done
```

Finally I calculated median methylation for each sample and the concatenated file:

```{bash}
#For each file that ends in *UTROverlap-percentMeth
#Sort numerically
#Calculate the median of the sixth column (percent methylation) for each file
#Save output in a new file
(for f in *UTROverlap-percentMeth
do
  sort -n ${f} \
  | awk ' { a[i++]=$6; } END { x=int((i+1)/2); if (x < (i+1)/2) print (a[x-1]+a[x])/2; else print a[x-1]; }'
done) > 2020-02-25-UTROverlap-percentMeth-Medians.txt
```

The order of the output is the global methylation file, sample 10, then samples 1-9. The output files can be found here:

- [Promoter median methylation](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2020-02-25-promoterOverlap-percentMeth-Medians.txt)
- [UTR median methylation](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2020-02-25-UTROverlap-percentMeth-Medians.txt)
- [Exon median methylation](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2020-02-25-exonOverlap-percentMeth-Medians.txt)
- [Intron median methylation](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2020-02-25-intronOverlap-percentMeth-Medians.txt)
- [Transposable element median methylation](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2020-02-25-TEOverlap-percentMeth-Medians.txt)
- [Intergenic median methylation](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2020-02-25-intergenicOverlap-percentMeth-Medians.txt)

Now that I had this information, I needed to create a dataframe that I could use to create plots. I imported the data into R, then created some barplots (see below).

### Adding methylation island analysis

Since I [generated MI](https://yaaminiv.github.io/Gigas-and-Virginica-Comparison-Part2/) for my poster, Steven suggested I add the analysis to the revised paper. First, I characterzied any remaining overlaps bewteen genome feature tracks and MI in [this notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-03-18-Characterizing-CpG-Methylation.ipynb).

**Table 2**. MI and genome feature overlaps

|   **Genome Feature**  	| **MI Overlaps with Features** 	| **Feature Overlaps with MI** 	|
|:---------------------:	|:-----------------------------:	|:----------------------------:	|
|         Exons         	|             22705             	|            240133            	|
|        Introns        	|             28730             	|             92472            	|
|        Exon UTR       	|              8649             	|             30827            	|
|          mRNA         	|             29805             	|             29483            	|
|    Coding Sequences   	|             20872             	|            226237            	|
|  Non-Coding Sequences 	|             35932             	|             98103            	|
|         Genes         	|             30773             	|             15009            	|
|   Putative Promoters  	|              4217             	|             8846             	|
| Transposable Elements 	|             25085             	|            107926            	|
|         lncRNA        	|              949              	|             1108             	|
|   Intergenic Regions  	|             10302             	|             8526             	|
|      No Overlaps      	|              1154             	|              N/A             	|

In [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Proportion-Test.Rmd) I recolored my methylation island overlap plot and added it to one with locations of all CpGs and methylated CpGs. I also revised [previously-made boxplots](https://yaaminiv.github.io/Gigas-and-Virginica-Comparison-Part4/) in [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/2020-02-11-Characterizing-Methylation-Islands.Rmd). For the poster, I looked at how much each individual feature overlapped with respect to the length of the methylation island. For the revised boxplots, I thought it made more sense to look at the overlap length versus the length of the individual feature. For each genome feature — putative promoters, UTRs, exons, introns, transposable elements, and intergenic regions — I calculated the length of an individual feature (each line). I then divided the overlap length by the length of the individual feature.

### Figure changes

#### Figure 2

##### Figure 2a: Location of all CpGs vs. methylated CpGs vs. methlyation islands

Now that I had additional overlap information, I could update my figures. I pulled additional CG motif overlap information from [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-05-13-Generating-Genome-Feature-Tracks.ipynb) and updated [this document](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Overlap-Proportions.csv) with overlap counts. In [this R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-01-15-Proportion-Test.Rmd), I updated figures to include UTR and intergenic region overlaps in addition to putative promoters, exons, introns, transposable elements, and other (no overlap between exons, introns, transposable elements or putative promoters...which may no longer make sense but I'll wait for some feedback on that). I also included methylation island information!

<img width="824" alt="Screen Shot 2020-02-24 at 1 20 05 PM" src="https://user-images.githubusercontent.com/22335838/75193703-94260d80-570b-11ea-9096-c1cfd36b64a8.png">

##### Figure 2b: Individual feature overlap with methylation islands

After recalculating feature overlaps with methylation islands, I created a boxplot. It's interesting to see that the entirety of exons and transposable elements overlap entirely with methylation islands, while introns, promoters, UTRs, and intergenic regions are more variable.

<img width="827" alt="Screen Shot 2020-02-24 at 1 42 16 PM" src="https://user-images.githubusercontent.com/22335838/75193697-90928680-570b-11ea-8bc1-8b1cb95054f5.png">

##### Figure 2c: Percent methylation across genome features

I first plotted global and sample methylation across genome features in one barplot...

<img width="826" alt="Screen Shot 2020-02-25 at 11 51 44 PM" src="https://user-images.githubusercontent.com/22335838/75323548-ff660180-5829-11ea-866f-338017de4d8a.png">

...which was extremly busy. I then tried breaking it up by global vs. sample methylation.

<img width="824" alt="Screen Shot 2020-02-25 at 11 52 09 PM" src="https://user-images.githubusercontent.com/22335838/75323562-03921f00-582a-11ea-8654-6892ee4efb68.png">

<img width="826" alt="Screen Shot 2020-02-25 at 11 52 22 PM" src="https://user-images.githubusercontent.com/22335838/75323564-04c34c00-582a-11ea-8843-a2533e3f96f6.png">

While the global methylation plot was cleaner, the sample methylation plot was still pretty busy. Additionally, I think it's important to have the global methylation information in the same plot since it's the "background" for the sample methylation.

The plot version I settled on is a multipanel plot that compares different samples to global methylation for each genome feature:

<img width="822" alt="Screen Shot 2020-02-25 at 11 51 56 PM" src="https://user-images.githubusercontent.com/22335838/75323614-199fdf80-582a-11ea-8af3-7c9f53b35a63.png">

##### Full revised figure

#### Figure 5

I was asked to normalize the number of DML in Figure 5A by the total number of nucleotides in each chromosome. I went to [NCBI](https://www.ncbi.nlm.nih.gov/genome/398) and pulled out the chromosome length (Mb). I then divided the number of DML in each chromosome by length (Mb) in [this R Markdown file. I considered using the full length instead, but then my y-axis was comprised of really small numbers. I figured I could just indicated in the y-axis text that I normalized by length (Mb) instead of length (bp).

<img width="820" alt="Screen Shot 2020-02-19 at 2 40 40 PM" src="https://user-images.githubusercontent.com/22335838/74894572-3b89f580-5344-11ea-9df6-110863d7b226.png">

#### Figure 6: Location of DML vs. enriched CpGs

Similar to Figure 2a, I added additional information for where DML were found!

<img width="820" alt="Screen Shot 2020-02-24 at 11 37 31 AM" src="https://user-images.githubusercontent.com/22335838/75184946-19a0c200-56fa-11ea-8543-0f85582ecccd.png">

#### Figure 8

No changes...I just didn't include the attachment when I submitted the original paper *RIP*. It'll be included this time.

### Going forward

1. Address remaining comments about discussion text
1. Get feedback from Epigenetics Reading Group
2. Update manuscript text
2. Update response to reviewers
1. Consolidate any co-author feedback
3. Submit comment responses and reviesed manuscript
9. Post revised paper on bioRXiv
10. Update [paper repository](https://github.com/epigeneticstoocean/paper-gonad-meth) with new code and figures

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
