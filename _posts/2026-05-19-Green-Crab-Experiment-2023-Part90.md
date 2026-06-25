---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 90
tags: green-crab-wc RNA-Seq trinity salmon
---

## Transcriptome QC

I called in the cavalry and talked to Carolyn. She suggested I do some transcriptome QC prior to `EnTAP`:

1. Collapse isoforms into supertranscripts using a script packaged within `trinity` (*Note: while this would remove the ability for me to do isoform-level analysis with the resultant count matrix, there are probably better tools than `DESeq2` or `edgeR` that would be good for this  and would require some input finagling anyways*)
2. Run `salmon` using all samples together, and apply an expression-based filter to remove all contigs with TPM less than 0.5 or 1

The goal of this QC would be to remove any low-quality transcripts, thereby reducing my transcriptome size and allowing `EnTAP` to run better.

She also mentioned that I wouldn't need the `blast` step for transcriptome annotation and cleaning. Zac included that because he knew there was contamination, but simply using `EnTAP` to annotate the transcriptome and filter out any contaminant sequences would be good enough for my purposes.

### Collapsing isoforms into supertranscripts

I found [this page on the `trinity` Wiki](https://github.com/trinityrnaseq/trinityrnaseq/wiki/SuperTranscripts) detailing the script that can be used to produce a new FASTA file with isoforms collapsed into supertranscripts. Previously, I'd been [retaining the longest transcript per gene](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part89/) as a way to reduce the number of input transcripts into `EnTAP`, but creating these supertranscripts seems like a better approach.

To run the built-in script, I added the following code to my [`trinity` script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/06c-trinity.sh):

```
# Collapse isoforms into supertranscripts.
# Output files are trinity_genes.fasta (supertranscripts in fasta format), trinity_genes.gtf (transcript structure annotation in gtf format), and trinity_genes.malign (multiple alignment view that contrasts the different candidate splicing isoforms)
${TRINITY}/Analysis/SuperTranscripts/Trinity_gene_splice_modeler.py \
--trinity_fasta ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
--incl_malign
```

My understanding is that the supertranscript-level transcriptome would be used for all downstream analyses, and there wouldn't be a need to further collapse anything into "genes." I sent a quick message to Carolyn to confirm this.

I ran the code to collapse the isoform into supertranscripts. I checked on the progress of the code using the out file, which shows that `trinity` identifies the number of isoforms for each supertranscript:

> INFO:__main__:-parsing Trinity fasta file: /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/Trinity.fasta
INFO:__main__:-organizing gene/isoform data
INFO:__main__:-computing supertranscripts
INFO:__main__:# Processing Gene: TRINITY_DN0_c0_g1 having 10 isoforms
INFO:__main__:Splice graph refinement underway
INFO:__main__:# Processing Gene: TRINITY_DN0_c0_g2 having 2 isoforms
INFO:__main__:Splice graph refinement underway


Interestingly, the output got created in the main `trinity` folder, so I chose to move them to a new folder to keep the supertranscript-level analysis separate.

```
# Move output files to a new folder
mkdir supertranscript_output
rsync --archive --progress --verbose trinity_genes.* supertranscript_output/.
```

I then ran the downstream code to get baseline assembly statistic information:

```
#Use within-trinity tools to get baseline assembly statistics and files necessary for downstream analysis
# Assembly stats
${TRINITY}/util/TrinityStats.pl \
${OUTPUT_DIR}/supertranscript_output/trinity_genes.fasta \
> ${assembly_stats}

# Create gene map files
${TRINITY}/util/support_scripts/get_Trinity_gene_to_trans_map.pl \
${OUTPUT_DIR}/supertranscript_output/trinity_genes.fasta \
> ${OUTPUT_DIR}/supertranscript_output/Trinity.fasta.gene_trans_map

# Create sequence lengths file (used for differential gene expression)
${TRINITY}/util/misc/fasta_seq_length.pl \
${OUTPUT_DIR}/supertranscript_output/trinity_genes.fasta \
> ${OUTPUT_DIR}/supertranscript_output/Trinity.fasta.seq_lens

# Create FastA index
${SAMTOOLS} faidx \
${OUTPUT_DIR}/supertranscript_output/trinity_genes.fasta \
```

Unsurprisingly, it wasn't able to create a gene-transcript map, since I had already collapsed the isoforms into supertranscripts! I removed that from the script to avoid confusion. I also forgot to modify the path for the assembly statistics, so I moved that to the new supertranscript folder as well then looked at the information:

> ################################
## Counts of transcripts, etc.
################################
Total trinity 'genes':	648340
Total trinity transcripts:	648340
Percent GC: 42.92
########################################
Stats based on ALL transcript contigs:
########################################
	Contig N10: 3530
	Contig N20: 1788
	Contig N30: 1106
	Contig N40: 763
	Contig N50: 559
	Median contig length: 307
	Average contig: 490.98
	Total assembled bases: 318325191
#####################################################
## Stats based on ONLY LONGEST ISOFORM per 'GENE':
#####################################################
	Contig N10: 3530
	Contig N20: 1788
	Contig N30: 1106
	Contig N40: 763
	Contig N50: 559
	Median contig length: 307
	Average contig: 490.98
	Total assembled bases: 318325191

Obviously, the stats based on all transcripts vs. longest isoform distinction is moot since the isoforms have been collapsed into supertranscripts. There are 648,340 supertranscripts identified by `trinity`.

### Applying an expression-based filter to the `salmon` count matrix

Before I could filter a count matrix, I needed to create a new count matrix based on the supertranscript-level transcriptome. I started by modifying and running existing code to perform transcript abundance estimation for each sample:

```
# Prepare the reference (target index) with bowtie2 prior to transcript abundance estimation. The output is a BAM file necessary for pseudo-alignment
${TRINITY}/util/align_and_estimate_abundance.pl \
--transcripts ${OUTPUT_DIR}/supertranscript_output/trinity_genes.fasta \
--est_method salmon \
--aln_method bowtie2 \
--prep_reference \
--coordsort_bam

# Perform transcript abundance estimation with salmon
${TRINITY}/util/align_and_estimate_abundance.pl \
--transcripts ${OUTPUT_DIR}/supertranscript_output/trinity_genes.fasta \
--seqType fq \
--samples_file ${trinity_file_list} \
--SS_lib_type RF \
--est_method salmon \
--aln_method bowtie2 \
--output_dir ${OUTPUT_DIR}/supertranscript_output \
--thread_count 16
```

I checked my job about a week later (it had finished in a few hours but time got away from me...) and found out that the new quant files were not created!

>-output already exists: 5-188/quant.sf, skipping.
-output already exists: 5-197/quant.sf, skipping.

I removed these quant files since I will no longer need them. I then reran the transcript abundance estimation to get the quant files associated with the revised transcriptome. I confirmed that the quantification step was running:

```
CMD: salmon quant -i /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/supertranscript_output/trinity_genes.fasta.salmon.idx -l ISR -1 /vortexfs1/scratch/yaa
mini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/15-033_R1_001_val_1_val_1.fq.gz -2 /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimga
lore/trim-illumina-polyA/15-033_R2_001_val_2_val_2.fq.gz -o 15-033  -p 16 --validateMappings
Version Server Response: Not Found
### salmon (selective-alignment-based) v1.10.3
### [ program ] => salmon
### [ command ] => quant
### [ index ] => { /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/supertranscript_output/trinity_genes.fasta.salmon.idx }
### [ libType ] => { ISR }
### [ mates1 ] => { /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/15-033_R1_001_val_1_val_1.fq.gz }
### [ mates2 ] => { /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/15-033_R2_001_val_2_val_2.fq.gz }
### [ output ] => { 15-033 }
### [ threads ] => { 16 }
### [ validateMappings ] => { }
Logs will be written to 15-033/logs
[2026-05-26 13:58:25.195] [jointLog] [info] setting maxHashResizeThreads to 16
[2026-05-26 13:58:25.195] [jointLog] [info] Fragment incompatibility prior below threshold.  Incompatible fragments will be ignored.
[2026-05-26 13:58:25.195] [jointLog] [info] Usage of --validateMappings implies use of minScoreFraction. Since not explicitly specified, it is being set to 0.65
[2026-05-26 13:58:25.195] [jointLog] [info] Setting consensusSlack to selective-alignment default of 0.35.
[2026-05-26 13:58:25.195] [jointLog] [info] parsing read library format
[2026-05-26 13:58:25.195] [jointLog] [info] There is 1 library.
[2026-05-26 13:58:25.206] [jointLog] [info] Loading pufferfish index
[2026-05-26 13:58:25.209] [jointLog] [info] Loading dense pufferfish index.
```

While that was running, I wanted modify the next steps of the script to 1) apply the expression-based filter and 2) check that all downstream file paths were correct. I updated the filepaths for the code that created the all-sample abundance matrix:

```
#Transcript-level estimates
## Get a list of the salmon quant.sf files so we don't have to list them individually
find ${OUTPUT_DIR}/. -maxdepth 2 -name "quant.sf" | tee ${OUTPUT_DIR}/supertranscript_output/salmon.quant_files.txt

## Generate a matrix with abundance estimates across all samples
$TRINITY_HOME/util/abundance_estimates_to_matrix.pl \
--est_method salmon \
--quant_files ${OUTPUT_DIR}/supertranscript_output/salmon.quant_files.txt \
--name_sample_by_basedir
```

I then added a chunk of code using a built-in `trinity` script to remove low-abundance transcripts (Gemini helped me with this):

```
${TRINITY}/util/filter_low_expr_transcripts.pl \
--matrix ${OUTPUT_DIR}/supertranscript_output/salmon.isoform.TPM.not_cross_norm \
--transcripts ${OUTPUT_DIR}/supertranscript_output/trinity_genes.fasta \
--min_expr_any 0.5 \
--trinity_mode
```

The individual files have been created!

> (base) [yaamini.venkataraman@poseidon-l1 15-033]$ head quant.sf
Name	Length	EffectiveLength	TPM	NumReads
TRINITY_DN0_c0_g1	607	385.368	13.009802	240.234
TRINITY_DN0_c0_g2	405	195.349	18.979226	177.655
TRINITY_DN0_c0_g4	313	119.104	0.000000	0.000
TRINITY_DN0_c1_g1	689	466.177	7.742741	172.955
TRINITY_DN1_c0_g1	6161	5931.690	1.809426	514.289
TRINITY_DN1_c1_g1	4670	4440.690	3.638114	774.132
TRINITY_DN1_c1_g2	413	202.416	1.020890	9.902
TRINITY_DN1_c2_g1	1446	1216.690	0.308770	18.001
TRINITY_DN1_c3_g1	287	99.306	0.000000	0.000

Now that I have the individual files, I ran the following to piece them together into one count matrix:

```
#Transcript-level estimates
## Get a list of the salmon quant.sf files so we don't have to list them individually
find ${OUTPUT_DIR}/. -maxdepth 2 -name "quant.sf" | tee ${OUTPUT_DIR}/supertranscript_output/salmon.quant_files.txt

## Generate a matrix with abundance estimates across all samples
$TRINITY_HOME/util/abundance_estimates_to_matrix.pl \
--est_method salmon \
--quant_files ${OUTPUT_DIR}/supertranscript_output/salmon.quant_files.txt \
--name_sample_by_basedir \
--gene_trans_map none
```
Once I got my count matrix, I filtered it:

```
${TRINITY}/util/filter_low_expr_transcripts.pl \
--matrix ${OUTPUT_DIR}/supertranscript_output/salmon.isoform.TPM.not_cross_norm \
--transcripts ${OUTPUT_DIR}/supertranscript_output/trinity_genes.fasta \
--min_expr_any 0.5 \
--trinity_mode
```

The filtering ran, but I have no idea where the output file is, or if it simply re-wrote the files I had made previously. Since I never specified an output file, I seems like the filtered transcripts were directed to the standard error file. Rookie mistake (aka what I get for trusting Gemini code without checking it! I forgot the Golden Rule of Bioinformatics)! I reviewed the script requirements [in the `trinity` wiki](https://github.com/trinityrnaseq/trinityrnaseq/wiki/Trinity-Transcript-Quantification#filtering-transcripts) and found [this protocol document](https://www.protocols.io/view/transcriptome-assembly-dfib3kan.pdf) that outlined the necessary modifications. I then reran a modified chunk of code to perform filtering AND create a new output file:

```
${TRINITY}/util/filter_low_expr_transcripts.pl \
--matrix ${OUTPUT_DIR}/supertranscript_output/salmon.isoform.TPM.not_cross_norm \
--transcripts ${OUTPUT_DIR}/supertranscript_output/trinity_genes.fasta \
--min_expr_any 0.5 \
--trinity_mode \
> ${OUTPUT_DIR}/supertranscript_output/filtered_transcripts_min_exp_0.5.fasta
```

The new file was created, and the standard error shows all the transcripts that were removed due to low expression, and the total percent of transcripts retained:

> -excluding TRINITY_DN698243_c0_g1, max_expr: 0.275037 < 0.5
-excluding TRINITY_DN698249_c0_g1, max_expr: 0.490458 < 0.5
-excluding TRINITY_DN698262_c0_g1, max_expr: 0.336862 < 0.5
-excluding TRINITY_DN698267_c0_g1, max_expr: 0.352543 < 0.5
-excluding TRINITY_DN698281_c0_g1, max_expr: 0.357754 < 0.5
Retained 408651 / 648340 = 63.03% of total transcripts.

This seemed like important information, so I made a copy of the log file and saved it as `transcripts_removed_min_exp_0.5.txt`.

Now that I had the filtered transcriptome, my final step was to calculate Ex50 and N50 statistics. I decided to first calculate these statistics for the unfiltered transcriptome, so I had an idea of the assembly quality for raw transcriptome straight from `trinity`. I can calculate the assembly statistics for the final transcriptome after TPM filtering and contaminant removal later!

```
# TRANSCRIPTOME ASSEMBLY STATISTICS

# Calculate Ex50 statistics
contig_ExN50_statistic.pl ${OUTPUT_DIR}/supertranscript_output/salmon.isoform.TPM.not_cross_norm \
${OUTPUT_DIR}/supertranscript_output/trinity_genes.fasta \
| tee ${OUTPUT_DIR}/supertranscript_output/ExN50.stats

# Calculate N50 statistics
TrinityStats.pl ${OUTPUT_DIR}/supertranscript_output/trinity_genes.fasta \
> ${OUTPUT_DIR}/supertranscript_output/N50.txt
```

The E90N50 value is 2584, or ~2.6 kb. There are 12,398 supertranscripts that were used to contribute this value. This is much better than the N50 statistics.

At this point, I realized that my script needed some guideposting. I reorganized so the filtering would occur at the very end of the script, after assembly statistic calculation for the raw transcriptome. I also moved the output from filtering to a different folder to keep everything organized.

Reviewing Carolyn's suggestions, she expected 50,000-70,000 contigs after expression filtering. I have a whole order of magnitude more after filtering! I don't want to do too much filtering at this stage and lose some lowly-expressed unique transcripts that may be important for understanding crab responses to thermal exposure. I decided to repeat with a TPM < 1 filter to see if that would make a significant difference.

> (base) [yaamini.venkataraman@poseidon-l1 filtered_supertranscript_output]$ tail transcripts_removed_min_exp_1.txt
-excluding TRINITY_DN698267_c0_g1, max_expr: 0.352543 < 1
-excluding TRINITY_DN698269_c0_g1, max_expr: 0.618227 < 1
-excluding TRINITY_DN698271_c0_g1, max_expr: 0.537815 < 1
-excluding TRINITY_DN698276_c0_g1, max_expr: 0.518825 < 1
-excluding TRINITY_DN698281_c0_g1, max_expr: 0.357754 < 1
Retained 201238 / 648340 = 31.04% of total transcripts.

Repeating the analysis with a TPM < 1 filter only retained 201,238 transcripts! I think this is way too harsh of a filter, as it only keeps ~30% of the original transcriptome and could be removing lowly-expressed transcripts or transcripts uniquely expressed in a certain condition. I pinged Carolyn to get her thoughts on this. She suggested I actually go back to the transcriptome assembly step, since 600k supertranscripts is way too high, and perhaps there has been a change in the `trinity` algorithm that now leads to more, small transcripts being produced. The goal is to get more in the realm of the 50,000-70,000 supertranscripts she mentioned earlier, as this is less likely to spread my reads across assembly artifacts.

Back to the drawing board I guess!

### Going forward

1. Tweak transcriptome assembly parameters to reduce the number of assembly artifacts and total supertranscripts
2. Annotate transcriptome with `EnTAP`
2. Remove contaminant sequences identified by `EnTAP`
3. Create count matrix for clean transcriptome
2. Calculate Ex50 and N50 statistics for clean transcriptome
4. Repeat analysis with clean transcriptome and fuller annotations in `edgeR`
5. Identify temperature- and genotype-specific differentially expressed genes at the end of the experiment
6. Identify genes influenced by both temperature and time
2. Determine methods for functional analysis
7. Additional strand-specific analysis in the supergene region
8. Examine HOBO data from 2023 experiment
9. Demographic data analysis for 2023 paper

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
