---
layout: post
comments: true
title: MethCompare Part 5
tags: MethCompare bedtools
---

## Characterizing full samples

We got the all-clear from Shelly and Steven, so I started working on the CpG characterization pipeline with full samples in [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.ipynb).

### Running the full pipeline

#### *M. capitata*

After downloading the 10x bedgraphs from [this link](https://gannet.fish.washington.edu/seashell/bu-mox/scrubbed/031520-TG-bs/Mcap_tg/dedup/), I classified CpG in each sample as either methylated (> 50%), sparsely methylated (10-50%), or unmethylated (< 10%). I created a summary table that included counts and percentages for us to understand the data.

**Table 1**. Number and proportion of  methylated (> 50%), sparsely methylated (10-50%), and unmethylated (< 10%) CpG dinucleotides in *M. capitata* samples.

| **Sample** 	| **Method** 	| **CpG with Data** 	| **Methylated CpG** 	| **Sparsely Methylated CpG** 	| **Unmethylated CpG** 	|
|:----------:	|:----------:	|:-----------------:	|:------------------:	|:---------------------------:	|:--------------------:	|
|     10     	|    WGBS    	|        470893       	|         35488 (7.5%)        	|             43405 (9.2%)            	|         392000 (83.2%)        	|
|     11     	|    WGBS    	|       479520       	|         49640 (10.3%)       	|             42459 (8.9%)           	|         387421 (80.8%)        	|
|     12     	|    WGBS    	|        1756997       	|         173085 (9.9%)       	|             127160 (7.2%)           	|         1456752 (82.9%)         	|
|     13     	|    RRBS    	|      2945967      	|       204797 (7.0%)       	|            129206 (4.4%)           	|        2611964 (88.7%)      	|
|     14     	|    RRBS    	|      2310457      	|       136505 (5.9%)       	|            107375 (4.6%)          	|        2066577 (89.4%)       	|
|     15     	|    RRBS    	|      2874355      	|       172957 (6.0%)       	|            145976 (5.1%)          	|        2555422 (88.9%)       	|
|     16     	|  MBD-BSSeq 	|        44091       	|        16168 (36.7%)        	|             5283 (12.0%)             	|         22640 (51.3%)        	|
|     17     	|  MBD-BSSeq 	|        21797       	|         6080 (27.9%)        	|             2282 (10.5%)            	|         13435 (61.6%)        	|
|     18     	|  MBD-BSSeq 	|        14818       	|         4837 (32.6%)       	|             1433 (9.7%)            	|         8548 (57.7%)        	|

Looking at each method, it seems like MBD-BSSeq has more interindividual variation than WGBS and RRBS. I think this may be a factor of the sybiodinium contamination, but it would be interesting to actually investigate that variation statistically.

I then looked at the distribution of CpG in genes, CDS, introns, and intergenic regions. I used the [genome tracks I created](https://yaaminiv.github.io/MethCompare-Part3/), and used `intersectBed -v` with the gene track to look at intergenic regions. When I checked my output for intersections with genes, I noticed that the gene track still had CDS and intron information in it even though I fixed this error! I'm not sure what happened with Github commits and potential branch merging, but I went back to [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Generating-Genome-Feature-Tracks.ipynb) to recreate my gene track using only AUGUSTUS information. I also went through and looked at overlaps between CG motifs and each track. Most of the CG motifs for *M. capitata* are found in intergenic regions.

**Table 2**. CG motif overlaps with *M. capitata* genome tracks.

| ***M. capitata* Genome Feature** 	| **Number individual features** 	| **Overlaps with CG Motifs** 	| **% Total CG Motifs** 	|
|:----------------------------------:	|:------------------------------:	|:---------------------------:	|:--------------------:	|
|                Genes               	|             458273             	|           9450564          	|         32.9         	|
|          Coding Sequences          	|             283926             	|           1953206           	|          6.8         	|
|               Introns              	|             221428             	|           7503314          	|         26.2         	|
|         Intergenic Regions         	|               N/A              	|           19224826          	|         67.0         	|

Once I fixed my tracks, I used `intersectBED` to look at intersections between samples and variosu genome features. Again, I made tables that had count and percentage information. Similar to waht I saw in Table 1, there was more variation between MBD-BSSeq samples than the other methods. However, MBD-BSSeq still performed better at capturing methylated regions.

**Table 3**. Number and percent of CpGs that overlap with genes in *M. capitata*.

| **Sample** 	| **Method** 	| **CpG with Data** 	| **Methylated CpG** 	| **Sparsely Methylated CpG** 	| **Unmethylated CpG** 	|
|------------	|------------	|-------------------	|--------------------	|-----------------------------	|----------------------	|
|     10     	|    WGBS    	|        160102       	|         17007 (10.6%)        	|             15647 (9.8%)             	|         127448 (79.6%)         	|
|     11     	|    WGBS    	|        166510       	|         23163 (13.9%)        	|             15042 (9.0%)             	|         128305 (77.1%)         	|
|     12     	|    WGBS    	|        634252        	|         82744 (13.0%)       	|             44107 (7.0%)            	|         507401 (80.0%)        	|
|     13     	|    RRBS    	|      988858      	|       99753 (10.1%)      	|            46167 (4.7%)          	|        842938 (85.2%)       	|
|     14     	|    RRBS    	|       780718      	|       67014 (8.6%)      	|            39102 (5.0%)          	|        674602 (86.4%)       	|
|     15     	|    RRBS    	|       964930      	|       86291 (8.9%)      	|             53429 (5.5%)          	|        825210 (85.5%)       	|
|     16     	|  MBD-BSSeq 	|        11499       	|        6390 (55.6%)       	|             1215 (10.6%)            	|          3894 (33.9%)        	|
|     17     	|  MBD-BSSeq 	|        5127       	|         2373 (46.3%)       	|              511 (10.0%)            	|          2243 (43.7%)        	|
|     18     	|  MBD-BSSeq 	|        3278       	|         2018 (61.6%)       	|              165 (5.0%)            	|          1095 (33.4%)        	|

**Table 4**. Number and percent of CpGs that overlap with CDS in *M. capitata*.

| **Sample** 	| **Method** 	| **CpG with Data** 	| **Methylated CpG** 	| **Sparsely Methylated CpG** 	| **Unmethylated CpG** 	|
|------------	|------------	|-------------------	|--------------------	|-----------------------------	|----------------------	|
|     10     	|    WGBS    	|        63618       	|         5918 (9.3%)        	|             7652 (12.0%)             	|         50048 (78.7%)         	|
|     11     	|    WGBS    	|        67079       	|         8085 (12.1%)        	|             7257 (10.8%)             	|         51737 (77.1%)         	|
|     12     	|    WGBS    	|        209101        	|         25530 (12.2%)       	|             16812 (8.0%)            	|         166759 (79.8%)        	|
|     13     	|    RRBS    	|      190252      	|       14708 (7.7%)      	|            10382 (5.5%)          	|        165162 (86.8%)       	|
|     14     	|    RRBS    	|       151699      	|       9279 (6.1%)      	|            8654 (5.7%)          	|        133766 (8.8%)       	|
|     15     	|    RRBS    	|       186877      	|       11132 (6.0%)      	|             12318 (6.6%)          	|        163427 (87.5%)       	|
|     16     	|  MBD-BSSeq 	|        4664       	|        2046 (43.9%)       	|             623 (13.4%)            	|          1995 (42.8%)        	|
|     17     	|  MBD-BSSeq 	|        2307       	|         758 (32.9%)       	|              220 (9.5%)            	|          1329 (57.6%)        	|
|     18     	|  MBD-BSSeq 	|        1282       	|         750 (58.5)       	|              93 (7.3%)            	|          439 (34.2%)        	|

**Table 5**. Number and percent of CpGs that overlap with introns in *M. capitata*.

| **Sample** 	| **Method** 	| **CpG with Data** 	| **Methylated CpG** 	| **Sparsely Methylated CpG** 	| **Unmethylated CpG** 	|
|------------	|------------	|-------------------	|--------------------	|-----------------------------	|----------------------	|
|     10     	|    WGBS    	|        96590       	|         11106 (11.5%)        	|             8003 (8.3%)             	|         77481 (80.2%)         	|
|     11     	|    WGBS    	|        99522       	|         15088 (15.2%)        	|             7793 (7.8%)             	|         76641 (77.0%)         	|
|     12     	|    WGBS    	|        425558        	|         57278 (13.5%)       	|             27323 (6.4%)            	|         340957 (80.1%)        	|
|     13     	|    RRBS    	|      798984      	|       85069 (10.6%)      	|            35795 (4.5%)          	|        678120 (84.9%)       	|
|     14     	|    RRBS    	|       629356      	|       57748 (9.2%)      	|            30463 (4.8%)          	|        541145 (86.0%)       	|
|     15     	|    RRBS    	|       778492      	|       75177 (9.7%)      	|             41135 (5.3%)          	|        662180 (85.1%)       	|
|     16     	|  MBD-BSSeq 	|        6841       	|        4347 (63.5%)       	|             595 (8.7%)            	|          1899 (27.8%)        	|
|     17     	|  MBD-BSSeq 	|        2824       	|         1618 (57.3%)       	|              291 (10.3%)            	|          915 (32.4%)        	|
|     18     	|  MBD-BSSeq 	|        2000       	|         1270 (63.5%)       	|              73 (3.7%)            	|          657 (32.9%)        	|

**Table 6**. Number and percent of CpGs that overlap with intergenic regions in *M. capitata*.

| **Sample** 	| **Method** 	| **CpG with Data** 	| **Methylated CpG** 	| **Sparsely Methylated CpG** 	| **Unmethylated CpG** 	|
|------------	|------------	|-------------------	|--------------------	|-----------------------------	|----------------------	|
|     10     	|    WGBS    	|        310791       	|         18481 (5.9%)        	|             27758 (8.9%)             	|         264552 (85.1%)         	|
|     11     	|    WGBS    	|        313010       	|         26477 (8.5%)        	|             27417 (8.8%)             	|         259116 (82.8%)         	|
|     12     	|    WGBS    	|        1122745        	|         90341 (8.0%)       	|             83053 (7.4%)            	|         949351 (84.6%)        	|
|     13     	|    RRBS    	|      1957109      	|       105044 (5.4%)      	|            83039 (4.2%)          	|        1769026 (90.4%)       	|
|     14     	|    RRBS    	|       1529739      	|       69491 (4.5%)      	|            68273 (4.5%)          	|        1391975 (91.0%)       	|
|     15     	|    RRBS    	|       1909425      	|       86666 (4.5%)      	|             92547 (4.5%)          	|        1730212 (90.6%)       	|
|     16     	|  MBD-BSSeq 	|        32592       	|        9778 (30.0%)       	|             4068 (12.5%)            	|          18746 (57.5%)        	|
|     17     	|  MBD-BSSeq 	|        16670       	|         3707 (22.2%)       	|              1771 (10.6%)            	|          11192 (67.1%)        	|
|     18     	|  MBD-BSSeq 	|        11540       	|         2819 (24.2%)       	|              1268 (11.0%)            	|          7453 (64.6%)        	|

I generated these tables manually, but I really need a way to do this programmatically. That's a problem for later.

#### *P. acuta*

I did the same thing for all *P. acuta* samples! Just to be safe, I double-checked my code to generate the genome feature tracks and look at overlaps with CG motifs. Similar to *M. capitata*, most of the CG motifs were present in intergenic regions.

**Table 7**. *P. acuta* genome feature overlaps with CG motifs.

| ***P. acuta* Genome Feature** 	| **Number individual features** 	| **Overlaps with CG Motifs** 	| **% Total CG Motifs** 	|
|:-------------------------------:	|:------------------------------:	|:---------------------------:	|:---------------------:	|
|              Genes              	|              64558             	|           3434720           	|          35.6         	|
|         Coding Sequences        	|             318484             	|           1455630           	|          15.1         	|
|             Introns             	|             241534             	|           1999490           	|          20.7         	|
|        Intergenic Regions       	|               N/A              	|           5720900           	|          59.3         	|

I then looked at the number of methylated, sparsely methylated, and unmethylated CpGs in each sample. Similar to *M. capitata*, there was more interindividual variation for MBD-BSSeq samples. This variation was more pronounced for *P. acuta*.

**Table 8**. Number and proportion of  methylated (> 50%), sparsely methylated (10-50%), and unmethylated (< 10%) CpG dinucleotides in *P. acuta* samples.

| **Sample** 	| **Method** 	| **CpG with Data** 	| **Methylated CpG** 	| **Sparsely Methylated CpG** 	| **Unmethylated CpG** 	|
|:----------:	|:----------:	|:-----------------:	|:------------------:	|:---------------------------:	|:--------------------:	|
|     1     	|    WGBS    	|        2518069       	|         37201 (1.5%)       	|             83999 (3.3%)           	|         2396869 (95.2%)        	|
|     2     	|    WGBS    	|       3926923       	|         66524 (1.7%)       	|             109999 (2.8%)            	|         3750400 (95.5%)         	|
|     3     	|    WGBS    	|         3028012      	|         51081 (1.7%)       	|             104860 (3.5%)            	|         2872071 (94.9%)         	|
|     4     	|    RRBS    	|      1184293      	|       12021 (1.0%)       	|            65647 (5.5%)           	|        1106625 (93.4%)       	|
|     5     	|    RRBS    	|       992337     	|       14557 (1.5%)       	|            27831 (2.8%)          	|        949949 (95.7%)       	|
|     6     	|    RRBS    	|       1014588     	|       10621 (1.0%)      	|            38383 (3.8%)          	|        965584 (95.2%)        	|
|     7     	|  MBD-BSSeq 	|        744052       	|        195284 (26.2%)       	|             106836 (14.3%)             	|         441932 (59.4%)        	|
|     8     	|  MBD-BSSeq 	|        250032       	|         156098 (62.4%)       	|             39889 (16.0%)            	|         54045 (21.6%)        	|
|     9     	|  MBD-BSSeq 	|        725079       	|         187956 (25.9%)       	|             112592 (15.5%)             	|         424531  (58.5%)       	|

Finally, I looked at the overlaps between CpGs and genes, CDS, introns, and intergenic regions with `bedtools`. Making these tables really drove home the importance of generating these programatically instead of manually! I divided so many numbers in succession with Google that I had to cofirm I wasn't a bot many times.

**Table 9**. Number and percent of CpGs that overlap with genes in *P. acuta*.

| **Sample** 	| **Method** 	| **CpG with Data** 	| **Methylated CpG** 	| **Sparsely Methylated CpG** 	| **Unmethylated CpG** 	|
|------------	|------------	|-------------------	|--------------------	|-----------------------------	|----------------------	|
|     1     	|    WGBS    	|        1179434       	|         25899 (2.2%)        	|             34626 (2.9%)             	|         1118909 (94.9%)         	|
|     2     	|    WGBS    	|        1821034       	|         47716 (2.6%)        	|             49984 (2.7%)             	|         1723334 (94.6%)         	|
|     3     	|    WGBS    	|        1426087        	|         35357 (2.5%)       	|             44108 (3.1%)            	|         1346622 (94.4%)        	|
|     4     	|    RRBS    	|      502813      	|       4988 (1.0%)      	|            26205 (5.2%)          	|        471620 (93.8%)       	|
|     5     	|    RRBS    	|       416016      	|       5815 (1.4%)      	|            10664 (2.6%)          	|        399537 (96.0%)       	|
|     6     	|    RRBS    	|       428818      	|       4293 (1.0%)      	|             14614 (3.4%)          	|        409911 (95.6%)       	|
|     7     	|  MBD-BSSeq 	|        325489       	|        87468 (26.9%)       	|             31803 (9.8%)            	|          206218 (63.4%)        	|
|     8     	|  MBD-BSSeq 	|        88675       	|         60278 (68.0%)       	|              11894 (13.4%)            	|          16503 (18.6%)        	|
|     9     	|  MBD-BSSeq 	|        314739       	|         92131 (29.3%)       	|              34146 (10.8%)            	|          188462 (59.9%)        	|

**Table 10**. Number and percent of CpGs that overlap with CDS in *P. acuta*.

| **Sample** 	| **Method** 	| **CpG with Data** 	| **Methylated CpG** 	| **Sparsely Methylated CpG** 	| **Unmethylated CpG** 	|
|------------	|------------	|-------------------	|--------------------	|-----------------------------	|----------------------	|
|     1     	|    WGBS    	|        850489       	|         23872 (2.8%)        	|             26408 (3.1%)             	|         800209 (94.1%)         	|
|     2     	|    WGBS    	|        1219826       	|         41839 (3.4%)        	|             34947 (2.9%)             	|         1143040 (93.7%)         	|
|     3     	|    WGBS    	|        1010637        	|         32069 (3.2%)       	|             32550 (3.2%)            	|         946018 (93.6%)        	|
|     4     	|    RRBS    	|      334839      	|       3840 (1.1%)      	|            18125 (5.4%)          	|        312874 (93.4%)       	|
|     5     	|    RRBS    	|       275416      	|       4217 (1.5%)      	|            7617 (2.8%)          	|        263582 (95.7%)       	|
|     6     	|    RRBS    	|       283255      	|       3230 (1.1%)      	|             10420 (3.7%)          	|        269605 (95.2%)       	|
|     7     	|  MBD-BSSeq 	|        284856       	|        72744 (25.5%)       	|             24809 (8.7%)            	|          187303 (65.8%)        	|
|     8     	|  MBD-BSSeq 	|        73240       	|         51129 (69.8%)       	|              9109 (12.4%)            	|          13002 (17.8%)        	|
|     9     	|  MBD-BSSeq 	|        265810       	|         75705 (28.5%)       	|              26175 (9.8%)            	|          163930 (61.7%)        	|

**Table 11**. Number and percent of CpGs that overlap with introns in *P. acuta*.

| **Sample** 	| **Method** 	| **CpG with Data** 	| **Methylated CpG** 	| **Sparsely Methylated CpG** 	| **Unmethylated CpG** 	|
|------------	|------------	|-------------------	|--------------------	|-----------------------------	|----------------------	|
|     1     	|    WGBS    	|        733890       	|         11164 (1.5%)        	|             19112 (2.6%)             	|         703614 (95.9%)         	|
|     2     	|    WGBS    	|        1237263       	|         23483 (1.9%)        	|             32016 (2.6%)             	|         1181764 (95.5%)         	|
|     3     	|    WGBS    	|        908646        	|         15720 (1.7%)       	|             25760 (2.8%)            	|         867166 (95.4%)        	|
|     4     	|    RRBS    	|      336436      	|       2726 (0.8%)      	|            16685 (5.0%)          	|        317025 (94.2%)       	|
|     5     	|    RRBS    	|       277680      	|       3514 (1.3%)      	|            6409 (2.3%)          	|        267757 (96.4%)       	|
|     6     	|    RRBS    	|       287803      	|       2348 (0.8%)      	|             8824 (3.1%)          	|        276631 (96.1%)       	|
|     7     	|  MBD-BSSeq 	|        140343       	|        42347 (30.2%)       	|             15003 (10.7%)            	|          82993 (59.1%)        	|
|     8     	|  MBD-BSSeq 	|        40384       	|         27138 (67.2%)       	|              5541 (13.7%)            	|          7705 (19.1%)        	|
|     9     	|  MBD-BSSeq 	|        146914       	|         46592 (31.7%)       	|              16913 (11.5%)            	|          83409 (56.8%)        	|

**Table 12**. Number and percent of CpGs that overlap with intergenic regions in *P. acuta*.

| **Sample** 	| **Method** 	| **CpG with Data** 	| **Methylated CpG** 	| **Sparsely Methylated CpG** 	| **Unmethylated CpG** 	|
|------------	|------------	|-------------------	|--------------------	|-----------------------------	|----------------------	|
|     1     	|    WGBS    	|        1339417       	|         11319 (0.8%)        	|             49388 (3.7%)             	|         1278710 (95.5%)         	|
|     2     	|    WGBS    	|        2107121       	|         18848 (0.9%)        	|             60047 (2.8%)             	|         2028226 (96.3%)         	|
|     3     	|    WGBS    	|        1602822        	|         15747 (1.0%)       	|             60783 (3.8%)            	|         1526292 (95.2%)        	|
|     4     	|    RRBS    	|      681870      	|       7040 (1.0%)      	|            39459 (5.8%)          	|        635371 (93.2%)       	|
|     5     	|    RRBS    	|       576682      	|       8749 (1.5%)      	|            17176 (3.0%)          	|        550757 (95.5%)       	|
|     6     	|    RRBS    	|       586105      	|       6335 (1.1%)      	|             23780 (4.1%)          	|        555990 (94.9%)       	|
|     7     	|  MBD-BSSeq 	|        418826       	|        107907 (25.8%)       	|             75067 (17.9%)            	|          235852 (56.3%)        	|
|     8     	|  MBD-BSSeq 	|        161464       	|         95906 (59.4%)       	|              28006 (17.3%)            	|          37552 (23.3%)        	|
|     9     	|  MBD-BSSeq 	|        410614       	|         95913 (23.4%)       	|              78494 (19.1%)            	|          236207 (57.5%)        	|

Lookinga t these tables, it seems like sample 8 is the outlier for the MBD-BSSeq data. That file has less CpGs than the other two MBD-BSSeq files, and performs better at capturing methyalted regions. This could mean that there are less mapped reads for the sample becuse of C1 contamination, but I'm not sure. It's defienitely something to look into later.

### Going forward

3. Create figures for CpG characterization in various genome features
6. [Update code for methylation frequency distribution figure](https://github.com/hputnam/Meth_Compare/issues/39)
5. [Look into exon annotations in Liew and Li papers](https://github.com/hputnam/Meth_Compare/issues/40)
5. [Generate repeat tracks for each species](https://github.com/hputnam/Meth_Compare/issues/23)
7. Figure out how to meaningfully concatenate data for each method
3. Figure out methylation island analysis

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
