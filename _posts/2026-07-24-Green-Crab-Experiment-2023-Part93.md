---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 93
tags: green-crab-wc RNA-Seq trinity
---

## Troubleshooting transcriptome assembly

### 2026-07-24

My transcriptome assembly finished running! It was a suspiciously fast run, so I went through the log file to see what happened:

```
#############################################################################
Finished.  Final Trinity assemblies are written to /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir.Trinity.fasta
#############################################################################


Can't open /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir.Trinity.fasta: No such file or directory at /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/support_scripts/get_Trinity_gene_to_trans_map.pl line 7.
mv: cannot stat 'trinity_out_dir.Trinity.fasta': No such file or directory
```

So the transcriptome waas created, but then the script couldn't find the actual output file. Turns out, neither could I?? I saw a temporary transcriptome file, but not the actual transcriptome that was written. I started going through the log again to see if I could figure anything out:

```
--------------------------------------------------------
-------------------- Chrysalis -------------------------
-- (Contig Clustering & de Bruijn Graph Construction) --
--------------------------------------------------------

inchworm_target: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/both.fa
bowtie_reads_fa: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/both.fa
chrysalis_reads_fa: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/both.fa


#######################################################################
Inchworm file: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/inchworm.fa detected.
Skipping Inchworm Step, Using Previous Inchworm Assembly
#######################################################################
```

NO TRINITY I NEEDED A NEW ASSEMBLY BASED ON MY REVISED ASSEMBLY PARAMETERS! Since `trinity` already found an `inchworm` file it didn't make a new assembly. Do I need to delete all previous assembly information? Probably. I cleared the `trinity_out_dir` folder to ensure that a brand new transcriptome assembly would be created. I then queued my job (job ID 2141130) and hoped a real transcriptome would be created this time. I am so ready to be done with this.

### 2026-07-30

I ended up with a new error today...joy:

```
----------------------------------------------

--------------- Inchworm (K=25, asm) ---------------------

-- (Linear contig construction from k-mers) --

----------------------------------------------

-- Skipping CMD: /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/Inchworm/bin//inchworm --kmers jellyfish.kmers.25.asm.fa --run_inchworm -K 25 --monitor 1   --num_threads 6  --PARALLEL_IWORM   --min_any_entropy 1.0   -L 25  --no_prune_error_kmers  > /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/inchworm.fa.tmp, checkpoint [.iworm.25.asm.ok] exists.

-- Skipping CMD: mv /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/inchworm.fa.tmp /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/inchworm.fa, checkpoint [.iworm_renamed.25.asm.ok] exists.

Thursday, July 30, 2026: 11:12:49 CMD: touch /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/inchworm.fa.finished

NON_FATAL_EXCEPTION: WARNING, no Inchworm output is detected at: /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/inchworm.fa at /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/Trinity line 1843.

Thursday, July 30, 2026: 11:12:49 CMD: /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/support_scripts/get_Trinity_gene_to_trans_map.pl /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir.Trinity.fasta > /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir.Trinity.fasta.gene_trans_map

# No butterfly assemblies to report.

Can't open /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir.Trinity.fasta: No such file or directory at /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/support_scripts/get_Trinity_gene_to_trans_map.pl line 7.

mv: cannot stat 'trinity_out_dir.Trinity.fasta': No such file or directory
```

So `inchworm` didn't run and the transcriptome wasn't actually created. Turns out when I cleared `trinity_out_dir`, I didn't do a good enough job. There were still some `inchworm` checkpoint files from 2025 in the folder that needed to be removed! I straight-up removed the folder and requeued the script. I also added a line to the top of the script removing any output from previous runs:

```
# Clean up any residual/stale run directories from previous attempts
rm -rf ${OUTPUT_DIR}/trinity_out_dir ${OUTPUT_DIR}/trinity_out_dir.Trinity.fasta
```

Now to wait and see.

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
