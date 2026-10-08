---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 94
tags: green-crab-wc RNA-Seq trinity
---

## Troubleshooting transcriptome assembly (still...fml)

### 2026-09-21

Well, my script failed with a new error I'd never seen before! It seemed like `trinity` was looking for a previously-completed checkpoint (`.../read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa`), but was unable to find it:

```
cat: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa.out/inchworm.fa.SR.24: No such file or directory

Trinity run failed. Must investigate error above.

warning, cmd: /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/support_scripts/../../Trinity --single "/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa" --output "/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa.out" --CPU 1 --max_memory 1G --run_as_paired --SS_lib_type F --seqType fa --trinity_complete --full_cleanup --min_contig_length 200 --no_salmon   failed with ret: 6400, going to retry.

succeeded(183490), failed(1)   100% completed.  
```

To fix this, I added this chunk of code to the top of my script to remove any unfinished checkpoints (yes...Gemini did help me with this):

```
# Automatically remove incomplete/corrupted partition folders so Trinity can retry them cleanly
if [ -d "${OUTPUT_DIR}/trinity_out_dir/read_partitions" ]; then
    echo "Cleaning up any incomplete partition directories..."
    find ${OUTPUT_DIR}/trinity_out_dir/read_partitions -type d -name "*.trinity.reads.fa.out" | while read -r dir; do
        if [ ! -f "$dir/Trinity.fasta" ] && [ ! -f "$dir/inchworm.fa" ]; then
            rm -rf "$dir"
        fi
    done
fi
```

I then increased the memory for `trinity`, since the checkpoints likely failed creation because the run ran out of memory:

```
# Run Trinity to assemble de novo transcriptome. Using primarily default parameters.
${TRINITY}/Trinity \
--seqType fq \
--max_memory 400G \
--samples_file ${trinity_file_list} \
--SS_lib_type FR \
--min_contig_length 200 \
--full_cleanup \
--CPU 28
```

I restarted the job! Hopefully it starts (and finishes) soon so I can evaluate the assembly and move onto annotation.

### 2026-09-28

My job started after ~5 days and then immediately crapped out! I found this in the error log:

> /var/spool/slurmd/job233328/slurm_script: line 51: Automatically: command not found

MY DUMB ASS FORGOT TO ADD A "#" BEFORE THE COMMENT ABOUT CLEANING UP CORRUPTED PARTITIONS FML. I added the "#" and reviewed the rest of the script to ensure there wasn't anything glaringly obvious I needed to fix. I didn't see anything, so I submitted the job again.

### 2026-09-30

It failed again! This time it wasn't because I did something silly in my script. I got the following error:

> warning, cmd: /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/support_scripts/../../Trinity --single "/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa" --output "/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa.out" --CPU 1 --max_memory 1G --run_as_paired --SS_lib_type F --seqType fa --trinity_complete --full_cleanup --min_contig_length 200 --no_salmon   failed with ret: 6400, going to retry.
cat: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa.out/inchworm.fa.SR.18: No such file or directory
cat: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa.out/inchworm.fa.SR.20: No such file or directory
cat: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa.out/inchworm.fa.SR.22: No such file or directory
cat: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa.out/inchworm.fa.SR.24: No such file or directory
Trinity run failed. Must investigate error above.
warning, cmd: /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/support_scripts/../../Trinity --single "/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa" --output "/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/read_partitions/Fb_2/CBin_2499/c250132.trinity.reads.fa.out" --CPU 1 --max_memory 1G --run_as_paired --SS_lib_type F --seqType fa --trinity_complete --full_cleanup --min_contig_length 200 --no_salmon   failed with ret: 6400, going to retry.
^Msucceeded(0), failed(1)   100% completed.    
We are sorry, commands in file: [FailedCommands] failed.  :-( 
Error, cmd: /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/trinity-plugins/BIN/ParaFly -c recursive_trinity.cmds -CPU 28 -v -shuffle  died with ret 256 at /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/Trinity line 2919.
        main::process_cmd("/user/yaamini.venkataraman/.conda/envs/trinity_env/bin/trinit"...) called at /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/Trinity line 3653
        main::run_partitioned_cmds("recursive_trinity.cmds") called at /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/Trinity line 2528
        main::run_recursive_trinity("/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinit"...) called at /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/Trinity line 2271
        main::run_chrysalis("/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinit"..., "/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinit"..., 200, 500, "FR", "/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinit"..., "/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinit"...) called at /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/Trinity line 1895
        main::run_Trinity() called at /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/Trinity line 1500
        eval {...} called at /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/Trinity line 1499
Trinity run failed. Must investigate error above.

It seemed like there was still an issue with incomplete partitions, but I thought that my previous code addition would have cleaned up anything incomplete. I returned to Gemini where it suggested a really convoluted solution for dealing with empty and incomplete partitions. After some back-and-forth, I got a chunk of code that is a native `trinity` bypass for failed partitions:

```
# Native Trinity bypass for failed edge-case partitions from previous runs
if [ -f "${OUTPUT_DIR}/trinity_out_dir/FailedCommands" ]; then
    cat ${OUTPUT_DIR}/trinity_out_dir/FailedCommands >> ${OUTPUT_DIR}/trinity_out_dir/recursive_trinity.cmds.completed
fi
```

I added this code right to my script before the `trinity` command and restarted my job. The job started running immediately, and I checked that it finished phase 2 of `trinity` without error! Let's see what happens next.

### 2026-10-05

I got another error! My error log had the following on it:

> (base) [yaamini.venkataraman@hydra-l1 ~]$ tail /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/yrv_trinity240697.log
INFO:__main__:# Processing Gene: TRINITY_DN697283_c0_g1 having 1 isoforms
INFO:__main__:Splice graph refinement underway
INFO:__main__:# Processing Gene: TRINITY_DN697290_c0_g1 having 1 isoforms
INFO:__main__:Splice graph refinement underway
INFO:__main__:# Processing Gene: TRINITY_DN697299_c0_g1 having 1 isoforms
INFO:__main__:Splice graph refinement underway
INFO:__main__:Done.  Total runtime: 4.3 min
mkdir: cannot create directory ‘supertranscript_output’: File exists

Looks like the supertranscriptome output was created, but it was not moved to the correct folder since the folder already existed:

```
# Collapse isoforms into supertranscripts.
# Output files are trinity_genes.fasta (supertranscripts in fasta format), trinity_genes.gtf (transcript structure annotation in gtf format), and trinity_genes.malign (multiple alignment view that contrasts the different candidate splicing isoforms)
${TRINITY}/Analysis/SuperTranscripts/Trinity_gene_splice_modeler.py \
--trinity_fasta ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
--incl_malign

# Move output files to a new folder
mkdir supertranscript_output
rsync --archive --progress --verbose trinity_genes.* supertranscript_output/.
```

I restarted the script from the `rsync` chunk. The next step should be moving the supertranscriptome files, creating supplemental files, doing the abundance estimation, and filtering the transcriptome!

### 2026-10-06

Another error! Looks like there was a script failure during the transcript abundance estimation, since I could find the "Start transcript abundance estimation" memo but not the "Copmleted transcript abundance estimation" one (side note: so glad past me put these little memos in).

Going through the error log, I saw these lines:

> -output already exists: 15-033/quant.sf, skipping.
-output already exists: 15-091/quant.sf, skipping.
-output already exists: 15-092/quant.sf, skipping.
-output already exists: 15-152/quant.sf, skipping.
-output already exists: 15-154/quant.sf, skipping.
-output already exists: 15-161/quant.sf, skipping.
-output already exists: 25-051/quant.sf, skipping.
-output already exists: 25-053/quant.sf, skipping.

There are already abundance estimation output files for individual samples. These are likely from previous runs, so I removed them using `rm */quant.sf`.

The end of the error log looked like this:

> -reading file: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/./5-187/quant.sf
-reading file: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/./5-188/quant.sf
-reading file: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/./5-189/quant.sf
-reading file: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/./5-197/quant.sf
* Outputting combined matrix.
/user/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/support_scripts/run_TMM_scale_matrix.pl --matrix salmon.isoform.TPM.not_cross_norm > salmon.isoform.TMM.EXPR.matrixCMD: R --no-save --no-restore --no-site-file --no-init-file -q < salmon.isoform.TPM.not_cross_norm.runTMM.R 1>&2
/user/yaamini.venkataraman/.conda/envs/trinity_env/bin/R: line 250: /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/lib/R/etc/ldpaths: No such file or directory
Error, cmd: R --no-save --no-restore --no-site-file --no-init-file -q < salmon.isoform.TPM.not_cross_norm.runTMM.R 1>&2  died with ret (256)  at /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/support_scripts/run_TMM_scale_matrix.pl line 105.
Error, CMD: /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/support_scripts/run_TMM_scale_matrix.pl --matrix salmon.isoform.TPM.not_cross_norm > salmon.isoform.TMM.EXPR.matrix died with ret 6400 at /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/abundance_estimates_to_matrix.pl line 386.
(END)

The run failed in the process of combining the matrices, likely due a dependency issue. The run is calling for libraries that exist on `vortex`, but I now work on `hydra`~ I used the following code to replace `/vortexfs1/home` with `/user`:

```
conda activate trinity_env

#Replace vortex locations with locations on hydra for R + wrappers
sed -i 's|/vortexfs1/home/|/user/|g' ${CONDA_PREFIX}/bin/R
sed -i 's|/vortexfs1/home/|/user/|g' ${CONDA_PREFIX}/lib/R/etc/ldpaths
sed -i 's|/vortexfs1/home/|/user/|g' ${CONDA_PREFIX}/lib/R/etc/Makeconf

#Fix any remaining binaries/scripts in the environment referencing the old path
grep -rl "/vortexfs1/home" ${CONDA_PREFIX}/bin/ ${CONDA_PREFIX}/lib/R/etc/ 2>/dev/null | xargs -r sed -i 's|/vortexfs1/home/|/user/|g'

R --version #Confirm that R loads without errors now that paths are fixed
```

I started the transcript abundance estimation again. Let's see what happens?

### 2026-10-07

New day, new error (this is what happens when I only get 30 min a day to check on this script).

> (base) [yaamini.venkataraman@hydra-l2 ~]$ tail /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/yrv_trinity251883.log
[2026-10-06 17:59:25.061] [jointLog] [info] finished quantifyLibrary()
[2026-10-06 17:59:25.066] [jointLog] [info] Starting optimizer
[2026-10-06 17:59:25.195] [jointLog] [info] Marked 0 weighted equivalence classes as degenerate
[2026-10-06 17:59:25.210] [jointLog] [info] iteration = 0 | max rel diff. = 1454.29
[2026-10-06 17:59:26.309] [jointLog] [info] iteration = 100 | max rel diff. = 0.000108295
[2026-10-06 17:59:26.327] [jointLog] [info] Finished optimizer
[2026-10-06 17:59:26.327] [jointLog] [info] writing output
/var/spool/slurmd/job251883/slurm_script: line 68: 1403010 Broken pipe             find ${OUTPUT_DIR}/. -maxdepth 2 -name "quant.sf"
     1403011 Segmentation fault      (core dumped) | tee ${OUTPUT_DIR}/supertranscript_output/salmon.quant_files.txt

So it looks like `trinity` can't find the `quant.sf` files? I confirmed that the files were actually created and present by looking through the error log (files created!) and checking the directories:

> (base) [yaamini.venkataraman@hydra-l2 06c-trinity]$ cd 25-055
(base) [yaamini.venkataraman@hydra-l2 25-055]$ ls -la
total 31301
drwxrwxr-x 2 yaamini.venkataraman domain_users     4096 Oct  6 14:55 .
drwxrwxr-x 2 yaamini.venkataraman domain_users     4096 Oct  6 13:40 ..
drwxrwxr-x 2 yaamini.venkataraman domain_users     4096 Jan 26  2026 aux_info
-rw-rw-r-- 1 yaamini.venkataraman domain_users      558 Oct  6 14:55 cmd_info.json
drwxrwxr-x 2 yaamini.venkataraman domain_users     4096 Jan 26  2026 libParams
-rw-rw-r-- 1 yaamini.venkataraman domain_users      744 Oct  6 14:55 lib_format_counts.json
drwxrwxr-x 2 yaamini.venkataraman domain_users     4096 Jan 26  2026 logs
-rw-r--r-- 1 yaamini.venkataraman domain_users 32049202 Oct  6 14:55 quant.sf

Looking at the error message again, I think there's an issue with the code pointing `trinity` to the files:

> /var/spool/slurmd/job251883/slurm_script: line 68: 1403010 Broken pipe             find ${OUTPUT_DIR}/. -maxdepth 2 -name "quant.sf"
1403011 Segmentation fault      (core dumped) | tee ${OUTPUT_DIR}/supertranscript_output/salmon.quant_files.txt

I decided to run this piece of code interactively to see if I could reproduce the error:

> (base) [yaamini.venkataraman@hydra-l2 ~]$ find ${OUTPUT_DIR}/. -maxdepth 2 -name "quant.sf"
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/./15-033/quant.sf

Ah, this is where that extra "." is being thrown in! I decided to fix that and recheck:

> find ${OUTPUT_DIR}/ -maxdepth 2 -name "quant.sf"
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/15-033/quant.sf

I saw that there was already a `salmon.quant_files.txt` file, so I removed it. I then ran the potential problem line of code interactively:

> (base) [yaamini.venkataraman@hydra-l2 ~]$ find ${OUTPUT_DIR}/ -maxdepth 2 -name "quant.sf" | tee ${OUTPUT_DIR}/supertranscript_output/salmon.quant_files.txt
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/15-033/quant.sf
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/15-091/quant.sf

> (base) [yaamini.venkataraman@hydra-l2 ~]$ ls ${OUTPUT_DIR}/supertranscript_output -la
total 1453960
drwxrwxr-x 2 yaamini.venkataraman domain_users      4096 Oct  7 11:24 .
drwxrwxr-x 2 yaamini.venkataraman domain_users      4096 Oct  6 13:40 ..
-rw-r--r-- 1 yaamini.venkataraman domain_users  17283639 Oct  5 18:43 Trinity.fasta.seq_lens
-rw-r--r-- 1 yaamini.venkataraman domain_users       817 Oct  5 18:43 assembly_stats.txt
-rw-r--r-- 1 yaamini.venkataraman domain_users      6834 Oct  7 11:24 salmon.quant_files.txt
-rw-r--r-- 1 yaamini.venkataraman domain_users 335275660 Oct  1 17:00 trinity_genes.fasta
-rw-r--r-- 1 yaamini.venkataraman domain_users  28970836 Oct  5 18:43 trinity_genes.fasta.fai
drwxr-xr-x 2 yaamini.venkataraman domain_users      4096 Oct  5 18:45 trinity_genes.fasta.salmon.idx
-rw-r--r-- 1 yaamini.venkataraman domain_users 368059479 Oct  1 17:00 trinity_genes.gtf
-rw-r--r-- 1 yaamini.venkataraman domain_users 739255680 Oct  1 17:00 trinity_genes.malign

> (base) [yaamini.venkataraman@hydra-l2 ~]$ head ${OUTPUT_DIR}/supertranscript_output/salmon.quant_files.txt
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/15-033/quant.sf
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/15-091/quant.sf
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/15-092/quant.sf
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/15-152/quant.sf
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/15-154/quant.sf
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/15-161/quant.sf
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/25-046/quant.sf
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/25-047/quant.sf
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/25-048/quant.sf
/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/25-049/quant.sf

Well...seems like it works now? I restarted the SLURM script from the creation of a matrix with count information for all samples. I then got a brand new error:

> * Outputting combined matrix.
Error, no target samples. Shouldn't get here. at /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/abundance_estimates_to_matrix.pl line 351.

This is weird! I confirmed that the `salmon.quant_files.txt` file was created, but `trinity` does not have any target samples. I did a sanity check by making sure the path actually worked:

> (base) [yaamini.venkataraman@hydra-l1 ~]$ head /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/25-049/quant.sf
Name	Length	EffectiveLength	TPM	NumReads
TRINITY_DN0_c0_g1	3655	3467.928	7.093967	656.727
TRINITY_DN0_c0_g2	2122	1934.928	87.114698	4499.683
TRINITY_DN0_c1_g1	1820	1632.928	0.000000	0.000
TRINITY_DN0_c1_g2	214	57.377	0.000000	0.000
TRINITY_DN0_c1_g3	245	79.846	0.000000	0.000
TRINITY_DN0_c2_g1	1012	824.928	0.000000	0.000
TRINITY_DN0_c3_g1	1036	848.928	1.897458	43.000
TRINITY_DN0_c3_g2	1322	1134.928	0.066014	2.000
TRINITY_DN0_c4_g1	3018	2830.928	0.052930	4.000

Huh, I am so stumped. I tried rerunning the script starting with the line of code that creates `salmon.quant_files.txt` (aka what I know works). I returned to the same error as before!

> /var/spool/slurmd/job252632/slurm_script: line 47: 1286027 Broken pipe             find ${OUTPUT_DIR}/ -maxdepth 2 -name "quant.sf"
     1286028 Segmentation fault      (core dumped) | tee ${OUTPUT_DIR}/supertranscript_output/salmon.quant_files.txt

Turns out there is a segmentation error stemming from a package incompatibility between `hydra` and my `trinity_env` `conda` environment. I used Gemini to figure out how to reinstall packages to allow me to use pipes since that would be important later on:

```
conda activate trinity_env

# Reinstall glibc/GCC compatibility libraries
conda install -c conda-forge libgcc-ng libstdcxx-ng coreutils --force-reinstall -y

# Test if piping works now without error
echo "test" | sort | tee /tmp/test_pipe.txt
```

After activating `trinity_env` and trying to install the libraries, I ran into an error where I had to accept the terms of service for some `conda` channels. The error message provided me the code to do so:

```
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r
```

Once I accepted the TOS, I reran the `conda install` step. The `echo` command worked! Once the pipe capabilities were restored, I reran the script from the creation of the `salmon.quant_files.txt` file through to the end of the script:

> (trinity_env) [yaamini.venkataraman@hydra-l1 ~]$ tail /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/yrv_trinity252677.log
-excluding TRINITY_DN697283_c0_g1, max_expr: 0.695445 < 1
-excluding TRINITY_DN697290_c0_g1, max_expr: 0 < 1
-excluding TRINITY_DN697299_c0_g1, max_expr: 0 < 1
	Retained 79814 / 651948 = 12.24% of total transcripts.
Completed transcript filtering: TPM < 1
Ready to proceed to the next analytical step

Alright, now that I had my revised transcriptome, I wanted to look through the ExN50 statistics and number of total transcripts:

- There were a total of 651,948 supertranscripts
- ExN50 of 1880 for the transcripts that constituted 90% of the total expression
- TPM < 0.5 filtering: 132,720 total supertranscripts
- TPM < 1 filtering: 79,814 total supertranscripts

This already looks like an order of magnitude better than [the previous assembly(https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part90/), and more in-line with Carolyn's expectations! I sent the highlights over to Carolyn, but I think I'm good to proceed to the next step!

### Going forward

1. Annotate transcriptome with `EnTAP`
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
