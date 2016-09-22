---
layout: post
title: Jupyter Notebook
date: '2016-09-01'
categories: 
tags: jupyter
---


```python
cd /Volumes/web-1/btea/data/
```

    /Volumes/web-1/btea/data



```python
ls
```

    [31mGeoduck-transcriptome-v2.fasta[m[m*




```python
!head Geoduck-transcriptome-v2.fasta
```

    >comp7_c0_seq1 len=210 path=[5082:0-45 293:46-209]

    TTAACCAAGGTGTGACGCCGACGCAAGGGTGAGTAGAATAGCTCTGTTTATTATCCGAAT

    AGTCGAGCTAAAAACACAAAGAATAAAGGTTTAACAGTTCTATCTGAAATATATATTTGG

    ATATCTATTGGTAAGGATACGTTTTATATTAAAAACAAACAATTTATAAAGCGCTCTCGC

    ACCTTGTTTTTGCATTATGAGCATATACAT

    >comp30_c0_seq1 len=201 path=[6331:0-200]

    AAGAAAATTGATTTGAAATTGACTCTGCTTGAATAGAAAAAAATGTTTTGTTCTTTTTTT

    CGAAGTGTAAATTGTAAATTACTTTATTAAAAAATTCATAGTTTCCGGGCAAGTTATTTT

    TAATATATTGTAAATGTTGTCATTCAGAGGTTTGTTACGAATATATTGTTTGACAGACAT

    GCTACTGTTGTACTACTATTG




```python
!fgrep -c ">" Geoduck-transcriptome-v2.fasta
```

    154407




```python
!perl /Users/sr320/git-repos/course-btea/script-box/count_fasta.pl \
-i 1000 \
Geoduck-transcriptome-v2.fasta
```

    

    0:999 	127881

    1000:1999 	18040

    2000:2999 	5312

    3000:3999 	1808

    4000:4999 	773

    5000:5999 	284

    6000:6999 	139

    7000:7999 	86

    8000:8999 	21

    9000:9999 	33

    10000:10999 	7

    11000:11999 	6

    12000:12999 	3

    13000:13999 	4

    14000:14999 	4

    15000:15999 	3

    16000:16999 	0

    17000:17999 	2

    18000:18999 	1

    

    Total length of sequence:	101836734 bp

    Total number of sequences:	154407

    N25 stats:			25% of total sequence length is contained in the 8036 sequences >= 2055 bp

    N50 stats:			50% of total sequence length is contained in the 26074 sequences >= 1014 bp

    N75 stats:			75% of total sequence length is contained in the 64502 sequences >= 445 bp

    Total GC count:			37647852 bp

    GC %:				36.97 %

    




```python

```
