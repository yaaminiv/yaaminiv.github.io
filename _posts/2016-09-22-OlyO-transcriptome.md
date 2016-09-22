---
layout: post
title: Jupyter Notebook 3
date: '2016-09-01'
categories: 
tags: jupyter
---


# Olympia Oyster transcriptome

See Geoduck page for details on how to download and check integretity of raw files..


```python
cd /Users/sr320/data-genomic/tentacle/OlyOv5
```

    /Users/sr320/data-genomic/tentacle/OlyOv5



```python
cd PE
```

    /Users/sr320/data-genomic/tentacle/OlyOv5/PE



```python
!cat \
filtered_106A_Female_Mix_GATCAG_L004_R1.fastq \
filtered_108A_Female_Mix_GGCTAC_L004_R1.fastq \
filtered_106A_Male_Mix_TAGCTT_L004_R1.fastq \
filtered_108A_Male_Mix_AGTCAA_L004_R1.fastq >> /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq
!cat \
filtered_106A_Female_Mix_GATCAG_L004_R2.fastq \
filtered_108A_Female_Mix_GGCTAC_L004_R2.fastq \
filtered_106A_Male_Mix_TAGCTT_L004_R2.fastq \
filtered_108A_Male_Mix_AGTCAA_L004_R2.fastq >> /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R2.fastq
```


```python
!head /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq

```

    @HWI-ST700693:263:C0K6DACXX:4:1101:1464:1969
    NTTCATGAAGGGGTCAACCAGAATGATCTCGTAAAACTTGTAAGATGAATCCTGTGCCACCCAGTAAGAGCTTAGC
    +
    #4=DDDDDHFFHHGGABHIICBGH<E@FHIIGEGIIIIIIIDGGIIGEIIIIIIIIIIGIGGI9=D@CECEEF@CC
    @HWI-ST700693:263:C0K6DACXX:4:1101:1506:1985
    NCCTGATTGAAAACAAAATGTCTTTGTTTTTTCTACTTTGTATGGATCAATCAATCTTATTTGTTCCTGTAGAGAT
    +
    #1=DDFFFHHHHHJJJJJIGFHHJIIIIJJJJIIJIJJJJIJJFEGJIIIIHIGIJJEHIJJJGAGHJJIIIHGD;
    @HWI-ST700693:263:C0K6DACXX:4:1101:1882:1979
    NGGGACTCTAACGAGAACCTTCCGTCATCTGGCTCTGGACATAACCATGGTTAACAAGAACACTGTCATGGTGGAG



```python
cd /Users/sr320/data-genomic/tentacle/OlyOv6
```

    /Users/sr320/data-genomic/tentacle/OlyOv6



```python
!/Applications/bioinfo/FastQC/fastqc \
/Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq \
/Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R2.fastq
```

    Started analysis of OlyOv6_combined_R1.fastq
    Approx 5% complete for OlyOv6_combined_R1.fastq
    Approx 10% complete for OlyOv6_combined_R1.fastq
    Approx 15% complete for OlyOv6_combined_R1.fastq
    Approx 20% complete for OlyOv6_combined_R1.fastq
    Approx 25% complete for OlyOv6_combined_R1.fastq
    Approx 30% complete for OlyOv6_combined_R1.fastq
    Approx 35% complete for OlyOv6_combined_R1.fastq
    Approx 40% complete for OlyOv6_combined_R1.fastq
    Approx 45% complete for OlyOv6_combined_R1.fastq
    Approx 50% complete for OlyOv6_combined_R1.fastq
    Approx 55% complete for OlyOv6_combined_R1.fastq
    Approx 60% complete for OlyOv6_combined_R1.fastq
    Approx 65% complete for OlyOv6_combined_R1.fastq
    Approx 70% complete for OlyOv6_combined_R1.fastq
    Approx 75% complete for OlyOv6_combined_R1.fastq
    Approx 80% complete for OlyOv6_combined_R1.fastq
    Approx 85% complete for OlyOv6_combined_R1.fastq
    Approx 90% complete for OlyOv6_combined_R1.fastq
    Approx 95% complete for OlyOv6_combined_R1.fastq
    Analysis complete for OlyOv6_combined_R1.fastq
    Started analysis of OlyOv6_combined_R2.fastq
    Approx 5% complete for OlyOv6_combined_R2.fastq
    Approx 10% complete for OlyOv6_combined_R2.fastq
    Approx 15% complete for OlyOv6_combined_R2.fastq
    Approx 20% complete for OlyOv6_combined_R2.fastq
    Approx 25% complete for OlyOv6_combined_R2.fastq
    Approx 30% complete for OlyOv6_combined_R2.fastq
    Approx 35% complete for OlyOv6_combined_R2.fastq
    Approx 40% complete for OlyOv6_combined_R2.fastq
    Approx 45% complete for OlyOv6_combined_R2.fastq
    Approx 50% complete for OlyOv6_combined_R2.fastq
    Approx 55% complete for OlyOv6_combined_R2.fastq
    Approx 60% complete for OlyOv6_combined_R2.fastq
    Approx 65% complete for OlyOv6_combined_R2.fastq
    Approx 70% complete for OlyOv6_combined_R2.fastq
    Approx 75% complete for OlyOv6_combined_R2.fastq
    Approx 80% complete for OlyOv6_combined_R2.fastq
    Approx 85% complete for OlyOv6_combined_R2.fastq
    Approx 90% complete for OlyOv6_combined_R2.fastq
    Approx 95% complete for OlyOv6_combined_R2.fastq
    Analysis complete for OlyOv6_combined_R2.fastq


These files are created locally

<img src="http://eagle.fish.washington.edu/cnidarian/skitch/OlyOv6_1BBEE01A.png" alt="OlyOv6_1BBEE01A.png"/>

<img src="http://eagle.fish.washington.edu/cnidarian/skitch/OlyOv6_combined_R1_fastq_FastQC_Report_1BBEE058.png" alt="OlyOv6_combined_R1_fastq_FastQC_Report_1BBEE058.png"/>

<img src="http://eagle.fish.washington.edu/cnidarian/skitch/OlyOv6_combined_R1_fastq_FastQC_Report_1BBEE088.png" alt="OlyOv6_combined_R1_fastq_FastQC_Report_1BBEE088.png"/>

<img src="http://eagle.fish.washington.edu/cnidarian/skitch/OlyOv6_combined_R1_fastq_FastQC_Report_1BBEE0AB.png" alt="OlyOv6_combined_R1_fastq_FastQC_Report_1BBEE0AB.png"/>

Similar results for R2

<img src="http://eagle.fish.washington.edu/cnidarian/skitch/OlyOv6_combined_R2_fastq_FastQC_Report_1BBEE46A.png" alt="OlyOv6_combined_R2_fastq_FastQC_Report_1BBEE46A.png"/>


```python
pwd
```




    u'/Users/sr320/data-genomic/tentacle/OlyOv6'




```python
!/Applications/bioinfo/trim_galore_zip/trim_galore \
--paired \
--clip_R1 14 \
--clip_R2 14 \
--retain_unpaired \
--path_to_cutadapt /Users/sr320/.local/bin/cutadapt \
/Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq \
/Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R2.fastq
```

    No quality encoding type selected. Assuming that the data provided uses Sanger encoded Phred scores (default)
    
    Path to Cutadapt set as: '/Users/sr320/.local/bin/cutadapt' (user defined)
    1.8.1
    Cutadapt seems to be working fine (tested command '/Users/sr320/.local/bin/cutadapt --version')
    
    
    AUTO-DETECTING ADAPTER TYPE
    ===========================
    Attempting to auto-detect adapter type from the first 1 million sequences of the first file (>> /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq <<)
    
    Found perfect matches for the following adapter sequences:
    Adapter type	Count	Sequence	Sequences analysed	Percentage
    Illumina	5712	AGATCGGAAGAGC	1000000	0.57
    Nextera	3	CTGTCTCTTATA	1000000	0.00
    smallRNA	1	ATGGAATTCTCG	1000000	0.00
    Using Illumina adapter for trimming (count: 5712). Second best hit was Nextera (count: 3)
    
    Writing report to 'OlyOv6_combined_R1.fastq_trimming_report.txt'
    
    SUMMARISING RUN PARAMETERS
    ==========================
    Input filename: /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq
    Trimming mode: paired-end
    Trim Galore version: 0.4.0
    Cutadapt version: 1.8.1
    Quality Phred score cutoff: 20
    Quality encoding type selected: ASCII+33
    Adapter sequence: 'AGATCGGAAGAGC' (Illumina TruSeq, Sanger iPCR; auto-detected)
    Maximum trimming error rate: 0.1 (default)
    Minimum required adapter overlap (stringency): 1 bp
    Minimum required sequence length for both reads before a sequence pair gets removed: 20 bp
    Length cut-off for read 1: 35 bp (default)
    Length cut-off for read 2: 35 bb (default)
    All Read 1 sequences will be trimmed by 14 bp from their 5' end to avoid poor qualities or biases
    All Read 2 sequences will be trimmed by 14 bp from their 5' end to avoid poor qualities or biases (e.g. M-bias for BS-Seq applications)
    
    Writing final adapter and quality trimmed output to OlyOv6_combined_R1_trimmed.fq
    
    
      >>> Now performing quality (cutoff 20) and adapter trimming in a single pass for the adapter sequence: 'AGATCGGAAGAGC' from file /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq <<< 
    10000000 sequences processed
    20000000 sequences processed
    30000000 sequences processed
    40000000 sequences processed
    50000000 sequences processed
    60000000 sequences processed
    70000000 sequences processed
    80000000 sequences processed
    90000000 sequences processed
    100000000 sequences processed
    110000000 sequences processed
    120000000 sequences processed
    130000000 sequences processed
    140000000 sequences processed
    150000000 sequences processed
    160000000 sequences processed
    170000000 sequences processed
    180000000 sequences processed
    190000000 sequences processed
    200000000 sequences processed
    This is cutadapt 1.8.1 with Python 2.7.10
    Command line parameters: -f fastq -e 0.1 -q 20 -O 1 -a AGATCGGAAGAGC /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq
    Trimming 1 adapter with at most 10.0% errors in single-end mode ...
    Finished in 2144.94 s (11 us/read; 5.62 M reads/minute).
    
    === Summary ===
    
    Total reads processed:             200,998,380
    Reads with adapters:                73,774,681 (36.7%)
    Reads written (passing filters):   200,998,380 (100.0%)
    
    Total basepairs processed: 15,275,876,880 bp
    Quality-trimmed:             416,684,162 bp (2.7%)
    Total written (filtered):  14,564,248,250 bp (95.3%)
    
    === Adapter 1 ===
    
    Sequence: AGATCGGAAGAGC; Type: regular 3'; Length: 13; Trimmed: 73774681 times.
    
    No. of allowed errors:
    0-9 bp: 0; 10-13 bp: 1
    
    Bases preceding removed adapters:
      A: 33.0%
      C: 25.2%
      G: 17.2%
      T: 20.3%
      none/other: 4.3%
    
    Overview of removed sequences
    length	count	expect	max.err	error counts
    1	52089917	50249595.0	0	52089917
    2	13122915	12562398.8	0	13122915
    3	3984745	3140599.7	0	3984745
    4	876041	785149.9	0	876041
    5	192459	196287.5	0	192459
    6	42209	49071.9	0	42209
    7	21249	12268.0	0	21249
    8	16482	3067.0	0	16482
    9	17084	766.7	0	12797 4287
    10	17718	191.7	1	10499 7219
    11	12818	47.9	1	9549 3269
    12	12839	12.0	1	11800 1039
    13	8626	3.0	1	8184 442
    14	8526	3.0	1	8067 459
    15	7023	3.0	1	6596 427
    16	7179	3.0	1	6596 583
    17	7496	3.0	1	6726 770
    18	7079	3.0	1	6428 651
    19	4954	3.0	1	4399 555
    20	5021	3.0	1	4372 649
    21	4882	3.0	1	4090 792
    22	5126	3.0	1	4458 668
    23	6242	3.0	1	4749 1493
    24	6742	3.0	1	4742 2000
    25	7701	3.0	1	5052 2649
    26	7470	3.0	1	4266 3204
    27	11441	3.0	1	4746 6695
    28	10960	3.0	1	5263 5697
    29	12888	3.0	1	5044 7844
    30	10274	3.0	1	6240 4034
    31	8909	3.0	1	3674 5235
    32	9062	3.0	1	4115 4947
    33	19175	3.0	1	3926 15249
    34	20247	3.0	1	6814 13433
    35	20215	3.0	1	6308 13907
    36	18977	3.0	1	6897 12080
    37	24248	3.0	1	5779 18469
    38	29759	3.0	1	7077 22682
    39	38372	3.0	1	7309 31063
    40	49968	3.0	1	10540 39428
    41	34797	3.0	1	13808 20989
    42	38698	3.0	1	9281 29417
    43	27563	3.0	1	13132 14431
    44	38729	3.0	1	6563 32166
    45	42853	3.0	1	12030 30823
    46	30309	3.0	1	10213 20096
    47	27693	3.0	1	7937 19756
    48	30221	3.0	1	7209 23012
    49	29430	3.0	1	7957 21473
    50	28966	3.0	1	7287 21679
    51	69734	3.0	1	7286 62448
    52	92152	3.0	1	17407 74745
    53	41276	3.0	1	19721 21555
    54	36134	3.0	1	7428 28706
    55	31643	3.0	1	9177 22466
    56	48430	3.0	1	7663 40767
    57	48431	3.0	1	10921 37510
    58	78974	3.0	1	10519 68455
    59	52248	3.0	1	17284 34964
    60	55257	3.0	1	9701 45556
    61	76855	3.0	1	12860 63995
    62	175186	3.0	1	17609 157577
    63	197887	3.0	1	39008 158879
    64	122848	3.0	1	38522 84326
    65	85413	3.0	1	22646 62767
    66	116361	3.0	1	19925 96436
    67	163900	3.0	1	25021 138879
    68	271961	3.0	1	37785 234176
    69	365011	3.0	1	58318 306693
    70	241610	3.0	1	76299 165311
    71	145508	3.0	1	38669 106839
    72	74901	3.0	1	27598 47303
    73	19037	3.0	1	7591 11446
    74	13889	3.0	1	4672 9217
    75	5707	3.0	1	1628 4079
    76	30031	3.0	1	10128 19903
    
    
    RUN STATISTICS FOR INPUT FILE: /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq
    =============================================
    200998380 sequences processed in total
    The length threshold of paired-end sequences gets evaluated later on (in the validation step)
    
    Writing report to 'OlyOv6_combined_R2.fastq_trimming_report.txt'
    
    SUMMARISING RUN PARAMETERS
    ==========================
    Input filename: /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R2.fastq
    Trimming mode: paired-end
    Trim Galore version: 0.4.0
    Cutadapt version: 1.8.1
    Quality Phred score cutoff: 20
    Quality encoding type selected: ASCII+33
    Adapter sequence: 'AGATCGGAAGAGC' (Illumina TruSeq, Sanger iPCR; auto-detected)
    Maximum trimming error rate: 0.1 (default)
    Minimum required adapter overlap (stringency): 1 bp
    Minimum required sequence length for both reads before a sequence pair gets removed: 20 bp
    Length cut-off for read 1: 35 bp (default)
    Length cut-off for read 2: 35 bb (default)
    All Read 1 sequences will be trimmed by 14 bp from their 5' end to avoid poor qualities or biases
    All Read 2 sequences will be trimmed by 14 bp from their 5' end to avoid poor qualities or biases (e.g. M-bias for BS-Seq applications)
    
    Writing final adapter and quality trimmed output to OlyOv6_combined_R2_trimmed.fq
    
    
      >>> Now performing quality (cutoff 20) and adapter trimming in a single pass for the adapter sequence: 'AGATCGGAAGAGC' from file /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R2.fastq <<< 
    10000000 sequences processed
    20000000 sequences processed
    30000000 sequences processed
    40000000 sequences processed
    50000000 sequences processed
    60000000 sequences processed
    70000000 sequences processed
    80000000 sequences processed
    90000000 sequences processed
    100000000 sequences processed
    110000000 sequences processed
    120000000 sequences processed
    130000000 sequences processed
    140000000 sequences processed
    150000000 sequences processed
    160000000 sequences processed
    170000000 sequences processed
    180000000 sequences processed
    190000000 sequences processed
    200000000 sequences processed
    This is cutadapt 1.8.1 with Python 2.7.10
    Command line parameters: -f fastq -e 0.1 -q 20 -O 1 -a AGATCGGAAGAGC /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R2.fastq
    Trimming 1 adapter with at most 10.0% errors in single-end mode ...
    Finished in 2050.25 s (10 us/read; 5.88 M reads/minute).
    
    === Summary ===
    
    Total reads processed:             200,998,380
    Reads with adapters:                72,221,243 (35.9%)
    Reads written (passing filters):   200,998,380 (100.0%)
    
    Total basepairs processed: 15,275,876,880 bp
    Quality-trimmed:             582,620,358 bp (3.8%)
    Total written (filtered):  14,547,993,528 bp (95.2%)
    
    === Adapter 1 ===
    
    Sequence: AGATCGGAAGAGC; Type: regular 3'; Length: 13; Trimmed: 72221243 times.
    
    No. of allowed errors:
    0-9 bp: 0; 10-13 bp: 1
    
    Bases preceding removed adapters:
      A: 34.2%
      C: 26.2%
      G: 17.6%
      T: 21.1%
      none/other: 0.9%
    
    Overview of removed sequences
    length	count	expect	max.err	error counts
    1	53392369	50249595.0	0	53392369
    2	12692989	12562398.8	0	12692989
    3	3996044	3140599.7	0	3996044
    4	898465	785149.9	0	898465
    5	199957	196287.5	0	199957
    6	41733	49071.9	0	41733
    7	21420	12268.0	0	21420
    8	16937	3067.0	0	16937
    9	21594	766.7	0	15418 6176
    10	21338	191.7	1	12772 8566
    11	16835	47.9	1	11422 5413
    12	12411	12.0	1	11193 1218
    13	12316	3.0	1	9998 2318
    14	13123	3.0	1	12314 809
    15	7466	3.0	1	6846 620
    16	8648	3.0	1	7523 1125
    17	9515	3.0	1	9033 482
    18	5625	3.0	1	5083 542
    19	6811	3.0	1	6422 389
    20	5214	3.0	1	4965 249
    21	4196	3.0	1	3988 208
    22	5056	3.0	1	4787 269
    23	8323	3.0	1	5755 2568
    24	7914	3.0	1	7268 646
    25	6468	3.0	1	5960 508
    26	10420	3.0	1	5889 4531
    27	7100	3.0	1	6000 1100
    28	7244	3.0	1	6611 633
    29	5672	3.0	1	4854 818
    30	12359	3.0	1	11824 535
    31	2079	3.0	1	1623 456
    32	3282	3.0	1	3072 210
    33	2203	3.0	1	2014 189
    34	2939	3.0	1	2686 253
    35	3399	3.0	1	3149 250
    36	3761	3.0	1	3478 283
    37	3774	3.0	1	3480 294
    38	3903	3.0	1	3569 334
    39	4639	3.0	1	4278 361
    40	5311	3.0	1	4954 357
    41	6074	3.0	1	5422 652
    42	11033	3.0	1	10481 552
    43	3527	3.0	1	3210 317
    44	5255	3.0	1	4529 726
    45	7410	3.0	1	6983 427
    46	2498	3.0	1	2316 182
    47	3537	3.0	1	3265 272
    48	2722	3.0	1	2518 204
    49	3561	3.0	1	3277 284
    50	7011	3.0	1	6634 377
    51	5356	3.0	1	5029 327
    52	2795	3.0	1	2571 224
    53	2345	3.0	1	2117 228
    54	4249	3.0	1	4023 226
    55	5601	3.0	1	5243 358
    56	4440	3.0	1	3896 544
    57	5862	3.0	1	5432 430
    58	12815	3.0	1	12283 532
    59	9503	3.0	1	9079 424
    60	11281	3.0	1	10794 487
    61	16326	3.0	1	15494 832
    62	30462	3.0	1	28919 1543
    63	80723	3.0	1	77509 3214
    64	147721	3.0	1	137737 9984
    65	225595	3.0	1	207184 18411
    66	68475	3.0	1	62667 5808
    67	10044	3.0	1	8962 1082
    68	3200	3.0	1	2861 339
    69	1568	3.0	1	1362 206
    70	912	3.0	1	808 104
    71	836	3.0	1	580 256
    72	571	3.0	1	458 113
    73	787	3.0	1	415 372
    74	613	3.0	1	495 118
    75	1057	3.0	1	938 119
    76	4626	3.0	1	4181 445
    
    
    RUN STATISTICS FOR INPUT FILE: /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R2.fastq
    =============================================
    200998380 sequences processed in total
    The length threshold of paired-end sequences gets evaluated later on (in the validation step)
    
    Validate paired-end files OlyOv6_combined_R1_trimmed.fq and OlyOv6_combined_R2_trimmed.fq
    file_1: OlyOv6_combined_R1_trimmed.fq, file_2: OlyOv6_combined_R2_trimmed.fq
    
    
    >>>>> Now validing the length of the 2 paired-end infiles: OlyOv6_combined_R1_trimmed.fq and OlyOv6_combined_R2_trimmed.fq <<<<<
    Writing validated paired-end read 1 reads to OlyOv6_combined_R1_val_1.fq
    Writing validated paired-end read 2 reads to OlyOv6_combined_R2_val_2.fq
    
    Writing unpaired read 1 reads to OlyOv6_combined_R1_unpaired_1.fq
    Writing unpaired read 2 reads to OlyOv6_combined_R2_unpaired_2.fq
    
    Total number of sequences analysed: 200998380
    
    Number of sequence pairs removed because at least one read was shorter than the length cutoff (20 bp): 7305383 (3.63%)
    
    Deleting both intermediate output files OlyOv6_combined_R1_trimmed.fq and OlyOv6_combined_R2_trimmed.fq
    
    ====================================================================================================
    


files generated
<img src="http://eagle.fish.washington.edu/cnidarian/skitch/OlyOv6_1BBF222F.png" alt="OlyOv6_1BBF222F.png"/>


```python
!cat OlyOv6_combined_R1.fastq_trimming_report.txt
```

    
    SUMMARISING RUN PARAMETERS
    ==========================
    Input filename: /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq
    Trimming mode: paired-end
    Trim Galore version: 0.4.0
    Cutadapt version: 1.8.1
    Quality Phred score cutoff: 20
    Quality encoding type selected: ASCII+33
    Adapter sequence: 'AGATCGGAAGAGC' (Illumina TruSeq, Sanger iPCR; auto-detected)
    Maximum trimming error rate: 0.1 (default)
    Minimum required adapter overlap (stringency): 1 bp
    Minimum required sequence length for both reads before a sequence pair gets removed: 20 bp
    Length cut-off for read 1: 35 bp (default)
    Length cut-off for read 2: 35 bp (default)
    All Read 1 sequences will be trimmed by 14 bp from their 5' end to avoid poor qualities or biases
    All Read 2 sequences will be trimmed by 14 bp from their 5' end to avoid poor qualities or biases (e.g. M-bias for BS-Seq applications)
    
    
    This is cutadapt 1.8.1 with Python 2.7.10
    Command line parameters: -f fastq -e 0.1 -q 20 -O 1 -a AGATCGGAAGAGC /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq
    Trimming 1 adapter with at most 10.0% errors in single-end mode ...
    Finished in 2144.94 s (11 us/read; 5.62 M reads/minute).
    
    === Summary ===
    
    Total reads processed:             200,998,380
    Reads with adapters:                73,774,681 (36.7%)
    Reads written (passing filters):   200,998,380 (100.0%)
    
    Total basepairs processed: 15,275,876,880 bp
    Quality-trimmed:             416,684,162 bp (2.7%)
    Total written (filtered):  14,564,248,250 bp (95.3%)
    
    === Adapter 1 ===
    
    Sequence: AGATCGGAAGAGC; Type: regular 3'; Length: 13; Trimmed: 73774681 times.
    
    No. of allowed errors:
    0-9 bp: 0; 10-13 bp: 1
    
    Bases preceding removed adapters:
      A: 33.0%
      C: 25.2%
      G: 17.2%
      T: 20.3%
      none/other: 4.3%
    
    Overview of removed sequences
    length	count	expect	max.err	error counts
    1	52089917	50249595.0	0	52089917
    2	13122915	12562398.8	0	13122915
    3	3984745	3140599.7	0	3984745
    4	876041	785149.9	0	876041
    5	192459	196287.5	0	192459
    6	42209	49071.9	0	42209
    7	21249	12268.0	0	21249
    8	16482	3067.0	0	16482
    9	17084	766.7	0	12797 4287
    10	17718	191.7	1	10499 7219
    11	12818	47.9	1	9549 3269
    12	12839	12.0	1	11800 1039
    13	8626	3.0	1	8184 442
    14	8526	3.0	1	8067 459
    15	7023	3.0	1	6596 427
    16	7179	3.0	1	6596 583
    17	7496	3.0	1	6726 770
    18	7079	3.0	1	6428 651
    19	4954	3.0	1	4399 555
    20	5021	3.0	1	4372 649
    21	4882	3.0	1	4090 792
    22	5126	3.0	1	4458 668
    23	6242	3.0	1	4749 1493
    24	6742	3.0	1	4742 2000
    25	7701	3.0	1	5052 2649
    26	7470	3.0	1	4266 3204
    27	11441	3.0	1	4746 6695
    28	10960	3.0	1	5263 5697
    29	12888	3.0	1	5044 7844
    30	10274	3.0	1	6240 4034
    31	8909	3.0	1	3674 5235
    32	9062	3.0	1	4115 4947
    33	19175	3.0	1	3926 15249
    34	20247	3.0	1	6814 13433
    35	20215	3.0	1	6308 13907
    36	18977	3.0	1	6897 12080
    37	24248	3.0	1	5779 18469
    38	29759	3.0	1	7077 22682
    39	38372	3.0	1	7309 31063
    40	49968	3.0	1	10540 39428
    41	34797	3.0	1	13808 20989
    42	38698	3.0	1	9281 29417
    43	27563	3.0	1	13132 14431
    44	38729	3.0	1	6563 32166
    45	42853	3.0	1	12030 30823
    46	30309	3.0	1	10213 20096
    47	27693	3.0	1	7937 19756
    48	30221	3.0	1	7209 23012
    49	29430	3.0	1	7957 21473
    50	28966	3.0	1	7287 21679
    51	69734	3.0	1	7286 62448
    52	92152	3.0	1	17407 74745
    53	41276	3.0	1	19721 21555
    54	36134	3.0	1	7428 28706
    55	31643	3.0	1	9177 22466
    56	48430	3.0	1	7663 40767
    57	48431	3.0	1	10921 37510
    58	78974	3.0	1	10519 68455
    59	52248	3.0	1	17284 34964
    60	55257	3.0	1	9701 45556
    61	76855	3.0	1	12860 63995
    62	175186	3.0	1	17609 157577
    63	197887	3.0	1	39008 158879
    64	122848	3.0	1	38522 84326
    65	85413	3.0	1	22646 62767
    66	116361	3.0	1	19925 96436
    67	163900	3.0	1	25021 138879
    68	271961	3.0	1	37785 234176
    69	365011	3.0	1	58318 306693
    70	241610	3.0	1	76299 165311
    71	145508	3.0	1	38669 106839
    72	74901	3.0	1	27598 47303
    73	19037	3.0	1	7591 11446
    74	13889	3.0	1	4672 9217
    75	5707	3.0	1	1628 4079
    76	30031	3.0	1	10128 19903
    
    
    RUN STATISTICS FOR INPUT FILE: /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R1.fastq
    =============================================
    200998380 sequences processed in total
    



```python
!cat OlyOv6_combined_R2.fastq_trimming_report.txt
```

    
    SUMMARISING RUN PARAMETERS
    ==========================
    Input filename: /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R2.fastq
    Trimming mode: paired-end
    Trim Galore version: 0.4.0
    Cutadapt version: 1.8.1
    Quality Phred score cutoff: 20
    Quality encoding type selected: ASCII+33
    Adapter sequence: 'AGATCGGAAGAGC' (Illumina TruSeq, Sanger iPCR; auto-detected)
    Maximum trimming error rate: 0.1 (default)
    Minimum required adapter overlap (stringency): 1 bp
    Minimum required sequence length for both reads before a sequence pair gets removed: 20 bp
    Length cut-off for read 1: 35 bp (default)
    Length cut-off for read 2: 35 bp (default)
    All Read 1 sequences will be trimmed by 14 bp from their 5' end to avoid poor qualities or biases
    All Read 2 sequences will be trimmed by 14 bp from their 5' end to avoid poor qualities or biases (e.g. M-bias for BS-Seq applications)
    
    
    This is cutadapt 1.8.1 with Python 2.7.10
    Command line parameters: -f fastq -e 0.1 -q 20 -O 1 -a AGATCGGAAGAGC /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R2.fastq
    Trimming 1 adapter with at most 10.0% errors in single-end mode ...
    Finished in 2050.25 s (10 us/read; 5.88 M reads/minute).
    
    === Summary ===
    
    Total reads processed:             200,998,380
    Reads with adapters:                72,221,243 (35.9%)
    Reads written (passing filters):   200,998,380 (100.0%)
    
    Total basepairs processed: 15,275,876,880 bp
    Quality-trimmed:             582,620,358 bp (3.8%)
    Total written (filtered):  14,547,993,528 bp (95.2%)
    
    === Adapter 1 ===
    
    Sequence: AGATCGGAAGAGC; Type: regular 3'; Length: 13; Trimmed: 72221243 times.
    
    No. of allowed errors:
    0-9 bp: 0; 10-13 bp: 1
    
    Bases preceding removed adapters:
      A: 34.2%
      C: 26.2%
      G: 17.6%
      T: 21.1%
      none/other: 0.9%
    
    Overview of removed sequences
    length	count	expect	max.err	error counts
    1	53392369	50249595.0	0	53392369
    2	12692989	12562398.8	0	12692989
    3	3996044	3140599.7	0	3996044
    4	898465	785149.9	0	898465
    5	199957	196287.5	0	199957
    6	41733	49071.9	0	41733
    7	21420	12268.0	0	21420
    8	16937	3067.0	0	16937
    9	21594	766.7	0	15418 6176
    10	21338	191.7	1	12772 8566
    11	16835	47.9	1	11422 5413
    12	12411	12.0	1	11193 1218
    13	12316	3.0	1	9998 2318
    14	13123	3.0	1	12314 809
    15	7466	3.0	1	6846 620
    16	8648	3.0	1	7523 1125
    17	9515	3.0	1	9033 482
    18	5625	3.0	1	5083 542
    19	6811	3.0	1	6422 389
    20	5214	3.0	1	4965 249
    21	4196	3.0	1	3988 208
    22	5056	3.0	1	4787 269
    23	8323	3.0	1	5755 2568
    24	7914	3.0	1	7268 646
    25	6468	3.0	1	5960 508
    26	10420	3.0	1	5889 4531
    27	7100	3.0	1	6000 1100
    28	7244	3.0	1	6611 633
    29	5672	3.0	1	4854 818
    30	12359	3.0	1	11824 535
    31	2079	3.0	1	1623 456
    32	3282	3.0	1	3072 210
    33	2203	3.0	1	2014 189
    34	2939	3.0	1	2686 253
    35	3399	3.0	1	3149 250
    36	3761	3.0	1	3478 283
    37	3774	3.0	1	3480 294
    38	3903	3.0	1	3569 334
    39	4639	3.0	1	4278 361
    40	5311	3.0	1	4954 357
    41	6074	3.0	1	5422 652
    42	11033	3.0	1	10481 552
    43	3527	3.0	1	3210 317
    44	5255	3.0	1	4529 726
    45	7410	3.0	1	6983 427
    46	2498	3.0	1	2316 182
    47	3537	3.0	1	3265 272
    48	2722	3.0	1	2518 204
    49	3561	3.0	1	3277 284
    50	7011	3.0	1	6634 377
    51	5356	3.0	1	5029 327
    52	2795	3.0	1	2571 224
    53	2345	3.0	1	2117 228
    54	4249	3.0	1	4023 226
    55	5601	3.0	1	5243 358
    56	4440	3.0	1	3896 544
    57	5862	3.0	1	5432 430
    58	12815	3.0	1	12283 532
    59	9503	3.0	1	9079 424
    60	11281	3.0	1	10794 487
    61	16326	3.0	1	15494 832
    62	30462	3.0	1	28919 1543
    63	80723	3.0	1	77509 3214
    64	147721	3.0	1	137737 9984
    65	225595	3.0	1	207184 18411
    66	68475	3.0	1	62667 5808
    67	10044	3.0	1	8962 1082
    68	3200	3.0	1	2861 339
    69	1568	3.0	1	1362 206
    70	912	3.0	1	808 104
    71	836	3.0	1	580 256
    72	571	3.0	1	458 113
    73	787	3.0	1	415 372
    74	613	3.0	1	495 118
    75	1057	3.0	1	938 119
    76	4626	3.0	1	4181 445
    
    
    RUN STATISTICS FOR INPUT FILE: /Users/sr320/data-genomic/tentacle/OlyOv6/OlyOv6_combined_R2.fastq
    =============================================
    200998380 sequences processed in total
    
    Total number of sequences analysed for the sequence pair length validation: 200998380
    
    Number of sequence pairs removed because at least one read was shorter than the length cutoff (20 bp): 7305383 (3.63%)



```python
!/Applications/bioinfo/FastQC/fastqc \
OlyOv6_combined_R1_val_1.fq \
OlyOv6_combined_R2_val_2.fq
```

    Started analysis of OlyOv6_combined_R1_val_1.fq
    Approx 5% complete for OlyOv6_combined_R1_val_1.fq
    Approx 10% complete for OlyOv6_combined_R1_val_1.fq
    Approx 15% complete for OlyOv6_combined_R1_val_1.fq
    Approx 20% complete for OlyOv6_combined_R1_val_1.fq
    Approx 25% complete for OlyOv6_combined_R1_val_1.fq
    Approx 30% complete for OlyOv6_combined_R1_val_1.fq
    Approx 35% complete for OlyOv6_combined_R1_val_1.fq
    Approx 40% complete for OlyOv6_combined_R1_val_1.fq
    Approx 45% complete for OlyOv6_combined_R1_val_1.fq
    Approx 50% complete for OlyOv6_combined_R1_val_1.fq
    Approx 55% complete for OlyOv6_combined_R1_val_1.fq
    Approx 60% complete for OlyOv6_combined_R1_val_1.fq
    Approx 65% complete for OlyOv6_combined_R1_val_1.fq
    Approx 70% complete for OlyOv6_combined_R1_val_1.fq
    Approx 75% complete for OlyOv6_combined_R1_val_1.fq
    Approx 80% complete for OlyOv6_combined_R1_val_1.fq
    Approx 85% complete for OlyOv6_combined_R1_val_1.fq
    Approx 90% complete for OlyOv6_combined_R1_val_1.fq
    Approx 95% complete for OlyOv6_combined_R1_val_1.fq
    Analysis complete for OlyOv6_combined_R1_val_1.fq
    Started analysis of OlyOv6_combined_R2_val_2.fq
    Approx 5% complete for OlyOv6_combined_R2_val_2.fq
    Approx 10% complete for OlyOv6_combined_R2_val_2.fq
    Approx 15% complete for OlyOv6_combined_R2_val_2.fq
    Approx 20% complete for OlyOv6_combined_R2_val_2.fq
    Approx 25% complete for OlyOv6_combined_R2_val_2.fq
    Approx 30% complete for OlyOv6_combined_R2_val_2.fq
    Approx 35% complete for OlyOv6_combined_R2_val_2.fq
    Approx 40% complete for OlyOv6_combined_R2_val_2.fq
    Approx 45% complete for OlyOv6_combined_R2_val_2.fq
    Approx 50% complete for OlyOv6_combined_R2_val_2.fq
    Approx 55% complete for OlyOv6_combined_R2_val_2.fq
    Approx 60% complete for OlyOv6_combined_R2_val_2.fq
    Approx 65% complete for OlyOv6_combined_R2_val_2.fq
    Approx 70% complete for OlyOv6_combined_R2_val_2.fq
    Approx 75% complete for OlyOv6_combined_R2_val_2.fq
    Approx 80% complete for OlyOv6_combined_R2_val_2.fq
    Approx 85% complete for OlyOv6_combined_R2_val_2.fq
    Approx 90% complete for OlyOv6_combined_R2_val_2.fq
    Approx 95% complete for OlyOv6_combined_R2_val_2.fq
    Analysis complete for OlyOv6_combined_R2_val_2.fq


#Assembly
```
Trinity.pl \
--seqType fq \
-JM 24G \
--left /Volumes/web/cnidarian/OlyOv6_combined_R1_val_1.fq \
--right /Volumes/web/cnidarian/OlyOv6_combined_R2_val_2.fq \
--CPU 16
```


```python

```
