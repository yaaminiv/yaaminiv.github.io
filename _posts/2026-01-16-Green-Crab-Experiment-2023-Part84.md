---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 84
tags: green-crab-wc RNA-Seq trinity
---

## Troubleshooting script issues (part 2)

### 2026-01-16

I [started running the script on Dec. 31](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part81/), but then there was a node failure and my script was restarted by IS. I got an email last night saying that my job failed! I started by looking at the error log:

> (base) [yaamini.venkataraman@poseidon-l1 ~]$ tail /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/yrv_trinity1816194.log
[Fri Jan 16 01:43:23 2026] Running CMD: /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/support_scripts/filter_transcripts_require_min_cov.pl Trinity.tmp.fasta /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/both.fa salmon_outdir/quant.sf 2 > /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir.Trinity.fasta
Friday, January 16, 2026: 01:44:19	CMD: /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/support_scripts/get_Trinity_gene_to_trans_map.pl /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir.Trinity.fasta > /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir.Trinity.fasta.gene_trans_map
#############################################################################
Finished.  Final Trinity assemblies are written to /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir.Trinity.fasta
#############################################################################
Error: Couldn't open /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/Trinity.fasta

Looks like the transcriptome was written! However, it was written to the wrong place (again...yikes). The file path for the transcriptome is `/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir.Trinity.fasta` but `trinity` was looking for the transcriptome in `/vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/Trinity.fasta`. I first checked whether or not that transcriptome file even existed.

> (base) [yaamini.venkataraman@poseidon-l1 06c-trinity]$ head trinity_out_dir.Trinity.fasta
>TRINITY_DN0_c0_g1_i11 len=236 path=[2:0-27 3:28-31 4:32-95 5:96-109 8:110-111 10:112-122 11:123-134 12:135-158 14:159-159 29:160-161 32:162-163 33:164-202 35:203-235]
GCGCTGGAGGGCGCCGTGGTCATCCGTTTAGTAGCCGCTGGAGGGCGCCATGGTCATCCT
TAAAAGCAGGTCCTGGAGGGCGCCATGGTCATCCGTGTAGTAGGCGCTGGATGGCGCCGT
GGTCATCCGTGTAGTATGTGCTGGAGAGCGCCATGGTCAGCCATAAAAGCAGGTCCTGGA
GGGCGCCGTGGTCATCCGTGTAGTAGATGCTGGAGGGCGCCATGGTCATCCGTGTA
>TRINITY_DN0_c0_g1_i12 len=274 path=[4:0-63 6:64-78 9:79-79 10:80-90 11:91-102 12:103-126 13:127-131 16:132-138 17:139-139 20:140-140 21:141-172 22:173-181 24:182-183 25:184-215 26:216-216 28:217-230 29:231-232 32:233-234 33:235-273]
AGCCGCTGGAGGGCGCCATGGTCATCCTTAAAAGCAGGTCCTGGAGGGCGCCATGGTCAT
CCGTTTAGTAGGCGCTGAAGGGCGCCGTGGTCATCCGTGTAGTATGTGCTGGAGAGCGCC
ATGGTCATCCATAAAAGCAGGTCCTGGAGGGCGCCGTGATCATCCGTGTAGTAGGCGCTG
GAGTGCGCCATGGTCATCCGTGTAGTAGGCGCTGGATGGCGCCATGGTCATCCATAAAAG

SHE EXISTS! I HAVE A TRANSCRIPTOME! There was another file that looks like a gene-isoform map as well:

>(base) [yaamini.venkataraman@poseidon-l1 06c-trinity]$ head trinity_out_dir.Trinity.fasta.gene_trans_map
TRINITY_DN0_c0_g1	TRINITY_DN0_c0_g1_i11
TRINITY_DN0_c0_g1	TRINITY_DN0_c0_g1_i12
TRINITY_DN0_c0_g1	TRINITY_DN0_c0_g1_i13
TRINITY_DN0_c0_g1	TRINITY_DN0_c0_g1_i2
TRINITY_DN0_c0_g1	TRINITY_DN0_c0_g1_i4
TRINITY_DN0_c0_g1	TRINITY_DN0_c0_g1_i5
TRINITY_DN0_c0_g1	TRINITY_DN0_c0_g1_i6
TRINITY_DN0_c0_g1	TRINITY_DN0_c0_g1_i7
TRINITY_DN0_c0_g1	TRINITY_DN0_c0_g1_i8
TRINITY_DN0_c0_g1	TRINITY_DN0_c0_g1_i9

Now I had a couple of issues:

1. Rename the transcriptome and gene map files to be consistent with what the script expects
2. Fix any file paths in the script to ensure that if this script was run again, I wouldn't end up with the same issues
3. Figure out how to continue with the `trinity` workflow

I used `mv` to move and rename the files:

```
(base) [yaamini.venkataraman@poseidon-l1 06c-trinity]$ mv trinity_out_dir.Trinity.fasta trinity_out_dir/Trinity.fasta
(base) [yaamini.venkataraman@poseidon-l1 06c-trinity]$ mv trinity_out_dir.Trinity.fasta.gene_trans_map trinity_out_dir/Trinity.fasta.gene_trans_map
```

For the second point, I'm actually not sure what governs the file path distinction, since I already have all of my paths listed:

```
#Program paths
TRINITY=/vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/
CUTADAPT=/vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/cutadapt
FASTQC=/vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/fastqc
python=/vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/python
JELLYFISH=//vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/jellyfish
SALMON=/vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/salmon
SAMTOOLS=/vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/samtools

#Directory and file paths
DATA_DIR=/vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA
OUTPUT_DIR=/vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity
assembly_stats=assembly_stats.txt
trinity_file_list=/vortexfs1/home/yaamini.venkataraman/trinity-samples.txt
```

I did a quick Google search and found [this page](https://github.com/trinityrnaseq/trinityrnaseq/wiki/Output-of-Trinity-Assembly), which says `trinity` will create a `trinity_out_dir.Trinity.fasta` output file by default. If I supply a prefix, it may write the file to a different location. Since this is the default behavior, I decided to add a line of code to the script to move the transcriptome to the correct directory for future steps:

```
# Run Trinity to assemble de novo transcriptome. Using primarily default parameters.
${TRINITY}/Trinity \
--seqType fq \
--max_memory 100G \
--samples_file ${trinity_file_list} \
--SS_lib_type RF \
--min_contig_length 200 \
--full_cleanup \
--CPU 28

# Move transcriptome to the correct location
mv trinity_out_dir.Trinity.fasta trinity_out_dir/Trinity.fasta
```

Now to figure out how to move forward. The transcriptome existing means that `trinity` is finished! So the error must have come from the subsequent sections. I think the issue is that a perl script meant to get assembly statistics couldn't find the transcriptome! I confirmed this by the fact that the output assembly statistics file is empty, but was created shortly before the script failed. I restarted the script to run just the assembly statistics parts:

```
# Assembly stats
${TRINITY}/util/TrinityStats.pl \
${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
> ${assembly_stats}

# Create gene map files
${TRINITY}/util/support_scripts/get_Trinity_gene_to_trans_map.pl \
${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
> ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta.gene_trans_map
```

### 2026-01-20

I got the assembly statistics information and a revised gene map! I moved the transcriptome, assembly stats, and gene map to my home directory to avoid any fishy cluster business. I also moved the assembly stats and gene map to [this repository folder](https://github.com/yaaminiv/wc-green-crab/tree/main/output/06c-trinity).

I then took a look at the assembly statistics:

```
################################
## Counts of transcripts, etc.
################################
Total trinity 'genes':	648340
Total trinity transcripts:	913680
Percent GC: 43.38

########################################
Stats based on ALL transcript contigs:
########################################

	Contig N10: 3987
	Contig N20: 2408
	Contig N30: 1530
	Contig N40: 1009
	Contig N50: 699

	Median contig length: 327
	Average contig: 555.10
	Total assembled bases: 507186700


#####################################################
## Stats based on ONLY LONGEST ISOFORM per 'GENE':
#####################################################

	Contig N10: 2696
	Contig N20: 1423
	Contig N30: 903
	Contig N40: 641
	Contig N50: 488

	Median contig length: 300
	Average contig: 454.68
	Total assembled bases: 294789897
```

I need to understand if this is pretty common. The median contig length is quite short, around ~300 bp. I used a 200 bp minimum contig length. The N50 is 699, which means that half of the nucleotides in the transcriptome are at least 699 bp long. However, there can be some issue when translating N50 to transcriptome data, as it is meant for genomes. I need to get the Ex50 (N50 for transcripts that constitute 90% of the total expression) information instead, which means I will need to look at Zac's scripts to figure out how to do that. There are also some tools listed on the [`trinity` wiki for understanding assembly quality](https://github.com/trinityrnaseq/trinityrnaseq/wiki/Transcriptome-Assembly-Quality-Assessment).

I decided to start by comparing my assembly to the one in Tepolt et al. (2015). The initial assembly was created with CLC workbench, not `trinity`, and it was done over 10 years ago, so a direct comparison probably isn't the easiest. However, Tepolt et al. reported 117,189 contigs with an N50 of 1016. This means that their assembly had fewer contigs, but they were much longer. I compared my assembly to Zac's as well, since he used similar methods. He reported 146,332 contigs with an average contig length of 577 bp and a total N50 of 774 bp. The average contig length and N50 are pretty comparable to my assembly. I just have way more contigs, but I think that's pretty standard for the dumpster fire that is the green crab genome.

### 2026-01-21

My task today was to figure out how to move forward with getting assembly statistics. The main thing I want to do is get ExN50, since that's a better metric of transcriptome assembly quality than the N50 provided by `trinity`. I first looked at my script and realized there were a couple of things I missed from Grace's script within the assembly-adjacent steps!

```
# Create sequence lengths file (used for differential gene expression)
${TRINITY}/util/misc/fasta_seq_length.pl \
${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
> ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta.seq_lens

# Create FastA index
${SAMTOOLS} faidx \
${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta
```
I ran these two pieces of code.

> (base) [yaamini.venkataraman@poseidon-l2 trinity_out_dir]$ head Trinity.fasta.seq_lens
fasta_entry	length
TRINITY_DN0_c0_g1_i11	236
TRINITY_DN0_c0_g1_i12	274
TRINITY_DN0_c0_g1_i13	221
TRINITY_DN0_c0_g1_i2	274
TRINITY_DN0_c0_g1_i4	243
TRINITY_DN0_c0_g1_i5	227
TRINITY_DN0_c0_g1_i6	294
TRINITY_DN0_c0_g1_i7	252
TRINITY_DN0_c0_g1_i8	380

> (base) [yaamini.venkataraman@poseidon-l2 trinity_out_dir]$ head Trinity.fasta.fai
TRINITY_DN0_c0_g1_i11	236	168	60	61
TRINITY_DN0_c0_g1_i12	274	642	60	61
TRINITY_DN0_c0_g1_i13	221	1091	60	61
TRINITY_DN0_c0_g1_i2	274	1557	60	61
TRINITY_DN0_c0_g1_i4	243	2049	60	61
TRINITY_DN0_c0_g1_i5	227	2433	60	61
TRINITY_DN0_c0_g1_i6	294	2866	60	61
TRINITY_DN0_c0_g1_i7	252	3360	60	61
TRINITY_DN0_c0_g1_i8	380	3874	60	61
TRINITY_DN0_c0_g1_i9	355	4518	60	61

I also moved these files to my home directory on and to [this repository folder](https://github.com/yaaminiv/wc-green-crab/tree/main/output/06c-trinity). I then reviewed the methods section of Zac's manuscript. It seems like he was able to get ExN50 information from within `trinity`. I returned to that very helpful Assembly Quality Assessment page and followed [this link](https://github.com/trinityrnaseq/trinityrnaseq/wiki/Transcriptome-Contig-Nx-and-ExN50-stats) to information for Ex50 statistics. Looks like there is a `perl` script for Ex50 calculation, but I need to perform transcript abundance estimation first. Once again, the `trinity` wiki had a page [all about it](https://github.com/trinityrnaseq/trinityrnaseq/wiki/Trinity-Transcript-Quantification) (side note: this is an extremely helpful manual we love reproducibility).

I think the workflow is to:

1. Prepare the reference
2. Run abundance estimation with `salmon` within `trinity` (no need to build transcript or gene expression matrices until I need to start `DESeq2`)
3. Calculate Ex50 statistics within `trinity`

The important part of Zac's `snakemake` code seems to start [here](https://github.com/tepoltlab/RhithroLoxo_DE/blob/master/Snakefile#L222). He prepared the reference with `salmon` because he wanted to pass kmer options to the salmon index, but I don't know how necessary that is for me since I haven't iterated on multiple different kmer parameters like he has. I think I could use the default `trinity` method of preparing the reference with `bowtie2` within `trinity`. I will follow the `trinity` recommendation to run this in two separate coding steps, as opposed to doing all of it in one coding step:

```
# Prepare the reference (target index) with bowtie2 prior to transcript abundance estimation. The output is a BAM file necessary for pseudo-alignment
${TRINITY}/util/align_and_estimate_abundance.pl \
--transcripts ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
--est_method salmon \
--aln_method bowtie2 \
--gene_trans_map ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta.gene_trans_map \
--prep_reference \
--coordsort_bam

# Perform transcript abundance estimation with salmon
${TRINITY}/util/align_and_estimate_abundance.pl \
--transcripts ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
--seqType fq \
--samples_file ${trinity_file_list} \
--SS_lib_type RF \
--est_method salmon \
--aln_method bowtie2 \
--gene_trans_map ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta.gene_trans_map \
--output_dir ${OUTPUT_DIR} \
--salmon_add_opts "--validateMappings " \
--thread_count 16
```

The `bowtie2` part was done based on the `trinity` manual, while I added some arguments from Zac's code to the salmon part. I also noticed that `bowtie2` was not included in my path list. I need to see if I even have `bowtie2`!

> (base) [yaamini.venkataraman@poseidon-l2 ~]$ conda activate trinity_env
(trinity_env) [yaamini.venkataraman@poseidon-l2 ~]$ which bowtie2
~/.conda/envs/trinity_env/bin/bowtie2

I have `bowtie2` installed in my conda environment, so I went ahead and added that to my path list. I then started the script.

### Going forward

1. Index, get advanced transcriptome statistics, and pseudoalign with `salmon`
4. Clean transcriptome with `EnTap` and `blastn`
3. Quantify transcripts with `salmon`
4. Identify differentially expressed genes
5. Additional strand-specific analysis in the supergene region
2. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper

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
