---
layout: post
comments: true
title: MethCompare Part 8
tags: MethCompare bedtools
---

## Characterizing CpGs in 5x data

After confirming last week that mapping to a pan-genome wasn't changing our output, I went ahead with the CpG characterization pipeline. We decided to [move ahead with 5x data](https://github.com/hputnam/Meth_Compare/issues/42) with the option to return to 10x data later. I decided to create a [new Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x.ipynb) for running 5x data through the pipeline.

### Repo fiasco

Turns out we haven't mastered how to have five people working on a repo simultaneously without having pull-and-push issues. When Sam [reverted to an old repo version](https://github.com/hputnam/Meth_Compare/issues/52), I lost the [checksum code](https://yaaminiv.github.io/MethCompare-Part6/) I worked on earlier. The first thing I did was add the code back in to [my 10x Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-10x.ipynb) and to the new notebook I created for 5x data. This isn't the first time a repo commit from someone else lost some of my code, so hopefully we figure out!

### *M. capitata*

Using 5x files provided more CpG data to characterize, but the percentages of methylated CpGs total and in various genome features were mostly consistent between 5x and 10x data.

**Table 1**. Methylated, sparsely methylated, and unmethylated CpGs in *M. capitata*

| **Sample** 	| **Method** 	| **CpG with Data** 	| **Methylated CpG** 	| **Sparsely Methylated CpG** 	| **Unmethylated CpG** 	|
|:----------:	|:----------:	|:-----------------:	|:------------------:	|:---------------------------:	|:--------------------:	|
|     10     	|    WGBS    	|        4571288       	|         450582 (9.9%)        	|             547868 (12.0%)            	|         3572838 (78.2%)        	|
|     11     	|    WGBS    	|       4661716       	|         528902 (11.3%)       	|             517805 (11.1%)           	|         3615009 (77.5%)        	|
|     12     	|    WGBS    	|        8791700       	|         1059904 (12.1%)       	|             1000337 (11.4%)           	|         6731459 (76.6%)         	|
|     13     	|    RRBS    	|      3173254      	|       257741 (8.1%)       	|            152042 (4.8%)           	|        2763471 (87.1%)      	|
|     14     	|    RRBS    	|      2648697      	|       184742 (7.0%)       	|            135052 (5.1%)          	|        2328903 (87.9%)       	|
|     15     	|    RRBS    	|      3176517      	|       231347 (7.3%)       	|            179454 (5.6%)          	|        2765716 (87.1%)       	|
|     16     	|  MBD-BSSeq 	|        583599       	|        106695 (18.3%)        	|             74839 (12.8%)             	|         402065 (68.9%)        	|
|     17     	|  MBD-BSSeq 	|        242390       	|         45506 (18.8%)        	|             28850 (11.9%)            	|         168034 (69.3%)        	|
|     18     	|  MBD-BSSeq 	|        153392       	|         29468 (19.2%)       	|             16793 (10.9%)            	|         107131 (69.8%)        	|

**Table 2**. Number and percent of CpGs that overlap with genes in *M. capitata*.

| **Sample** 	| **Method** 	| **CpGs with Data** 	| **Methylated CpGs** 	| **Sparsely Methylated CpGs** 	| **Unmethylated CpGs** 	|
|:----------:	|:----------:	|:------------------:	|:-------------------:	|:----------------------------:	|:---------------------:	|
|     10     	|    WGBS    	|       1683069      	|    230343 (13.7%)   	|        196827 (11.7%)        	|    1255899 (74.6%)    	|
|     11     	|    WGBS    	|       1740149      	|    267181 (15.3%)   	|        188348 (10.8%)        	|    1284620 (73.8%)    	|
|     12     	|    WGBS    	|       3204885      	|    533230 (16.6%)   	|        348967 (10.9%)        	|    2322688 (72.5%)    	|
|     13     	|    RRBS    	|       1062577      	|    123271 (11.6%)   	|         53235 (5.0%)         	|     886071 (83.4%)    	|
|     14     	|    RRBS    	|       895852       	|    89865 (10.0%)    	|         48499 (5.4%)         	|     757488 (84.6%)    	|
|     15     	|    RRBS    	|       1064769      	|    113217 (10.6%)   	|         64106 (6.0%)         	|     887446 (83.3%)    	|
|     16     	|  MBD-BSSeq 	|       208503       	|    48436 (23.2%)    	|         25316 (12.1%)        	|     134751 (64.6%)    	|
|     17     	|  MBD-BSSeq 	|        83519       	|    20458 (24.5%)    	|         9441 (11.3%)         	|     53620 (64.2%)     	|
|     18     	|  MBD-BSSeq 	|        50302       	|    12960 (25.8%)    	|          4843 (9.6%)         	|     32499 (64.6%)     	|

**Table 3**. Number and percent of CpGs that overlap with CDS in *M. capitata*.

| **Sample** 	| **Method** 	| **CpGs with Data** 	| **Methylated CpGs** 	| **Sparsely Methylated CpGs** 	| **Unmethylated CpGs** 	|
|:----------:	|:----------:	|:------------------:	|:-------------------:	|:----------------------------:	|:---------------------:	|
|     10     	|    WGBS    	|       476579       	|    54412 (11.4%)    	|         60266 (12.6%)        	|     361901 (75.9%)    	|
|     11     	|    WGBS    	|       496887       	|    64070 (12.9%)    	|         58258 (11.7%)        	|     374559 (75.4%)    	|
|     12     	|    WGBS    	|       791965       	|    113396 (14.3%)   	|         89455 (11.3%)        	|     589114 (74.4%)    	|
|     13     	|    RRBS    	|       200808       	|     18158 (9.0%)    	|         11383 (5.7%)         	|     171267 (85.3%)    	|
|     14     	|    RRBS    	|       172313       	|     12833 (7.4%)    	|         10486 (6.1%)         	|     148994 (86.5%)    	|
|     15     	|    RRBS    	|       203147       	|     15130 (7.4%)    	|         14015 (6.9%)         	|     174002 (85.7%)    	|
|     16     	|  MBD-BSSeq 	|        72490       	|    13535 (18.7%)    	|         9666 (13.3%)         	|     49289 (68.0%)     	|
|     17     	|  MBD-BSSeq 	|        29305       	|     5649 (19.3%)    	|         3690 (12.6%)         	|     19966 (68.1%)     	|
|     18     	|  MBD-BSSeq 	|        17619       	|     3992 (22.7%)    	|         1907 (10.8%)         	|     11720 (66.5%)     	|

**Table 4**. Number and percent of CpGs that overlap with introns in *M. capitata*.

| **Sample** 	| **Method** 	| **CpGs with Data** 	| **Methylated CpGs** 	| **Sparsely Methylated CpGs** 	| **Unmethylated CpGs** 	|
|:----------:	|:----------:	|:------------------:	|:-------------------:	|:----------------------------:	|:---------------------:	|
|     10     	|    WGBS    	|       1207609      	|    176102 (14.6%)   	|        136671 (11.3%)        	|     894836 (74.1%)    	|
|     11     	|    WGBS    	|       1244403      	|    203311 (16.3%)   	|        130195 (10.5%)        	|     910897 (73.2%)    	|
|     12     	|    WGBS    	|       2415036      	|    420249 (17.4%)   	|        259702 (10.8%)        	|    1735085 (71.8%)    	|
|     13     	|    RRBS    	|       862191       	|    105145 (12.2%)   	|         41866 (4.9%)         	|     715180 (82.9%)    	|
|     14     	|    RRBS    	|       723933       	|    77053 (10.6%)    	|         38032 (5.3%)         	|     608848 (84.1%)    	|
|     15     	|    RRBS    	|       862091       	|    98111 (11.4%)    	|         50116 (5.8%)         	|     713864 (82.8%)    	|
|     16     	|  MBD-BSSeq 	|       136153       	|    34929 (25.7%)    	|         15666 (11.5%)        	|     85558 (62.8%)     	|
|     17     	|  MBD-BSSeq 	|        54275       	|    14821 (27.3%)    	|         5755 (10.6%)         	|     33699 (62.1%)     	|
|     18     	|  MBD-BSSeq 	|        32717       	|     8973 (27.4%)    	|          2942 (9.0%)         	|     20802 (63.6%)     	|

**Table 5**. Number and percent of CpGs that overlap with intergenic regions in *M. capitata*.

| **Sample** 	| **Method** 	| **CpGs with Data** 	| **Methylated CpGs** 	| **Sparsely Methylated CpGs** 	| **Unmethylated CpGs** 	|
|:----------:	|:----------:	|:------------------:	|:-------------------:	|:----------------------------:	|:---------------------:	|
|     10     	|    WGBS    	|       2888219      	|    220239 (7.6%)    	|        351041 (12.2%)        	|    2316939 (80.2%)    	|
|     11     	|    WGBS    	|       2921567      	|    261721 (9.0%)    	|        329457 (11.3%)        	|    2330389 (79.8%)    	|
|     12     	|    WGBS    	|       5586815      	|    526674 (9.4%)    	|        651370 (11.7%)        	|    4408771 (78.9%)    	|
|     13     	|    RRBS    	|       2110677      	|    134470 (6.4%)    	|         98807 (4.7%)         	|    1877400 (88.9%)    	|
|     14     	|    RRBS    	|       1752845      	|     94877 (5.4%)    	|         86553 (4.9%)         	|    1571415 (89.6%)    	|
|     15     	|    RRBS    	|       2111748      	|    118130 (5.6%)    	|         115348 (5.5%)        	|    1878270 (88.9%)    	|
|     16     	|  MBD-BSSeq 	|       375096       	|    58259 (15.5%)    	|         49523 (13.2%)        	|     267314 (71.3%)    	|
|     17     	|  MBD-BSSeq 	|       158871       	|    25048 (15.8%)    	|         19409 (12.2%)        	|     114414 (72.0%)    	|
|     18     	|  MBD-BSSeq 	|       103090       	|    16508 (16.0%)    	|         11950 (11.6%)        	|     74632 (72.4%)     	|

### *P. acuta*

**Table 6**. Methylated, sparsely methylated, and unmethylated CpGs in *P. acuta*

| **Sample** | **Method** | **CpG with Data** | **Methylated CpG** | **Sparsely Methylated CpG** | **Unmethylated CpG** |
|:----------:|:----------:|:-----------------:|:------------------:|:---------------------------:|:--------------------:|
|      1     |    WGBS    |      5546051      |    110364 (2.0%)   |        367019 (6.6%)        |    5068668 (91.4%)   |
|      2     |    WGBS    |      6358722      |    126440 (2.0%)   |        345887 (5.4%)        |    5886395 (92.6%)   |
|      3     |    WGBS    |      5866786      |    124819 (2.1%)   |        385346 (6.6%)        |    5356621 (91.3%)   |
|      4     |    RRBS    |      1835561      |    31047 (1.7%)    |        137700 (7.5%)        |    1666814 (90.8%)   |
|      5     |    RRBS    |      1451229      |    30345 (2.1%)    |         64837 (4.5%)        |    1356047 (93.4%)   |
|      6     |    RRBS    |      1517358      |    26617 (1.8%)    |         89246 (5.9%)        |    1401495 (92.4%)   |
|      7     |  MBD-BSSeq |      2640625      |    258222 (9.8%)   |        296059 (11.2%)       |    2086344 (79.0%)   |
|      8     |  MBD-BSSeq |       539008      |   213342 (39.6%)   |        80086 (14.9%)        |    245580 (45.6%)    |
|      9     |  MBD-BSSeq |      2732607      |    255370 (9.3%)   |        337855 (12.4%)       |    2139382 (78.3%)   |

**Table 7**. Number and percent of CpGs that overlap with genes in *P. acuta*.

| **Sample** | **Method** | **CpG with Data** | **Methylated CpG** | **Sparsely Methylated CpG** | **Unmethylated CpG** |
|:----------:|:----------:|:-----------------:|:------------------:|:---------------------------:|:--------------------:|
|      1     |    WGBS    |      2466992      |    73959 (3.0%)    |        157337 (6.4%)        |    2235696 (90.6%)   |
|      2     |    WGBS    |      2761956      |    85861 (3.1%)    |        144292 (5.2%)        |    2531803 (91.7%)   |
|      3     |    WGBS    |      2588278      |    82377 (3.2%)    |        161791 (6.3%)        |    2344110 (90.6%)   |
|      4     |    RRBS    |       776123      |    13588 (1.8%)    |         56290 (7.3%)        |    706245 (91.0%)    |
|      5     |    RRBS    |       607225      |    12789 (2.1%)    |         25954 (4.3%)        |    568482 (93.6%)    |
|      6     |    RRBS    |       639570      |    11396 (1.8%)    |         35633 (5.6%)        |    592541 (92.6%)    |
|      7     |  MBD-BSSeq |      1253805      |    118291 (9.4%)   |        120485 (9.6%)        |    1015029 (81.0%)   |
|      8     |  MBD-BSSeq |       220096      |    86074 (39.1%)   |        27902 (12.7%)        |    106120 (48.2%)    |
|      9     |  MBD-BSSeq |      1281644      |    125536 (9.8%)   |        139034 (10.8%)       |    1017074 (79.4%)   |

**Table 8**. Number and percent of CpGs that overlap with CDS in *P. acuta*.

| **Sample** | **Method** | **CpG with Data** | **Methylated CpG** | **Sparsely Methylated CpG** | **Unmethylated CpG** |
|:----------:|:----------:|:-----------------:|:------------------:|:---------------------------:|:--------------------:|
|      1     |    WGBS    |      1494340      |    59188 (4.0%)    |         89863 (6.0%)        |    1345289 (90.0%)   |
|      2     |    WGBS    |      1620632      |    66365 (4.1%)    |         76868 (4.7%)        |    1477399 (91.2%)   |
|      3     |    WGBS    |      1552715      |    65245 (4.2%)    |         89654 (5.8%)        |    1397816 (90.0%)   |
|      4     |    RRBS    |       500779      |     9644 (1.9%)    |         36616 (7.3%)        |    454519 (90.8%)    |
|      5     |    RRBS    |       395869      |     8808 (2.2%)    |         17345 (4.4%)        |    369716 (93.4%)    |
|      6     |    RRBS    |       415529      |     7954 (1.9%)    |         23679 (5.7%)        |    383896 (92.4%)    |
|      7     |  MBD-BSSeq |       931250      |    92559 (9.9%)    |         87369 (9.4%)        |    751322 (80.7%)    |
|      8     |  MBD-BSSeq |       184606      |    70696 (38.3%)   |        21973 (11.9%)        |     91937 (49.8%)    |
|      9     |  MBD-BSSeq |       924825      |    95614 (10.3%)   |        98699 (10.7%)        |    730512 (79.0%)    |

**Table 9**. Number and percent of CpGs that overlap with introns in *P. acuta*.

| **Sample** | **Method** | **CpG with Data** | **Methylated CpG** | **Sparsely Methylated CpG** | **Unmethylated CpG** |
|:----------:|:----------:|:-----------------:|:------------------:|:---------------------------:|:--------------------:|
|      1     |    WGBS    |      1840138      |    41787 (2.3%)    |        122271 (6.6%)        |    1676080 (91.1%)   |
|      2     |    WGBS    |      2113295      |    51567 (2.4%)    |        117738 (5.6%)        |    1943990 (92.0%)   |
|      3     |    WGBS    |      1946150      |    47352 (2.4%)    |        128122 (6.6%)        |    1770676 (91.0%)   |
|      4     |    RRBS    |       539524      |     8446 (1.6%)    |         38589 (7.2%)        |    492489 (91.3%)    |
|      5     |    RRBS    |       414147      |     8375 (2.0%)    |         17239 (4.2%)        |    388533 (93.8%)    |
|      6     |    RRBS    |       439844      |     7162 (1.6%)    |         23825 (5.4%)        |    408857 (93.0%)    |
|      7     |  MBD-BSSeq |       746710      |    64572 (8.6%)    |         71046 (9.5%)        |    611092 (81.8%)    |
|      8     |  MBD-BSSeq |       101087      |    42068 (41.6%)   |        13365 (13.2%)        |     45654 (45.2%)    |
|      9     |  MBD-BSSeq |       794295      |    72530 (9.1%)    |        85117 (10.7%)        |    636648 (80.2%)    |

**Table 10**. Number and percent of CpGs that overlap with intergenic regions in *P. acuta*.

| **Sample** | **Method** | **CpG with Data** | **Methylated CpG** | **Sparsely Methylated CpG** | **Unmethylated CpG** |
|:----------:|:----------:|:-----------------:|:------------------:|:---------------------------:|:--------------------:|
|      1     |    WGBS    |      3080835      |   3080835 (1.2%)   |        209781 (6.8%)        |    2834593 (92.0%)   |
|      2     |    WGBS    |      3598848      |    40642 (1.1%)    |        201712 (5.6%)        |    3356494 (93.3%)   |
|      3     |    WGBS    |      3280357      |    42507 (1.3%)    |        223666 (6.8%)        |    3014184 (91.9%)   |
|      4     |    RRBS    |      1060062      |    17473 (1.6%)    |         81473 (7.7%)        |    961116 (90.7%)    |
|      5     |    RRBS    |       844509      |    17581 (2.1%)    |         38900 (4.6%)        |    788028 (93.3%)    |
|      6     |    RRBS    |       878266      |    15232 (1.7%)    |         53637 (6.1%)        |    809397 (92.2%)    |
|      7     |  MBD-BSSeq |      1387623      |   140057 (10.1%)   |        175668 (12.7%)       |    1071898 (77.2%)   |
|      8     |  MBD-BSSeq |       319125      |   127376 (39.9%)   |        52215 (16.4%)        |    139534 (43.7%)    |
|      9     |  MBD-BSSeq |      1451853      |    129949 (9.0%)   |        198940 (13.7%)       |    1122964 (77.3%)   |

### Going forward

3. Create figures for CpG characterization in various genome features
6. [Update code for methylation frequency distribution figure](https://github.com/hputnam/Meth_Compare/issues/39)
5. [Generate repeat tracks for each species](https://github.com/hputnam/Meth_Compare/issues/23)
6. Find a way to generate tables programmatically
4. Figure out how to meaningfully concatenate data for each method
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
