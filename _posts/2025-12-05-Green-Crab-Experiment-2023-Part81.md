---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 81
tags: green-crab-wc RNA-Seq trinity
---

## Troubleshooting script issues

### 2025-12-05

My script ran for 6 hours before it eventually got cancelled. Weird! I finally got around to checking the error log:

```
-------------------------------------------
----------- Jellyfish  --------------------
-- (building a k-mer catalog from reads) --
-------------------------------------------

CMD: jellyfish count -t 28 -m 25 -s 100000000  both.fa
CMD finished (8320 seconds)
CMD: jellyfish histo -t 28 -o jellyfish.K25.min2.kmers.fa.histo mer_counts.jf
CMD finished (317 seconds)
CMD: jellyfish dump -L 2 mer_counts.jf > jellyfish.K25.min2.kmers.fa
CMD finished (671 seconds)
CMD: touch jellyfish.K25.min2.kmers.fa.success
CMD finished (0 seconds)
-generating stats files
CMD: /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/..//Inchworm/bin/fastaToKmerCoverageStats --reads left.fa --kmers jellyfish.K25.min2.kmers.fa --kmer_size 25  --num_threads 14  > left.fa.K25.stats
CMD: /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/..//Inchworm/bin/fastaToKmerCoverageStats --reads right.fa --kmers jellyfish.K25.min2.kmers.fa --kmer_size 25  --num_threads 14  > right.fa.K25.stats
-reading Kmer occurrences...-reading Kmer occurrences...

slurmstepd: error: Job 1785248 exceeded memory limit (105782124 > 104857600), being killed
slurmstepd: error: Exceeded job memory limit
slurmstepd: error: *** JOB 1785248 ON pn052 CANCELLED AT 2025-12-01T22:13:32 ***
```

I upped the memory request to 180 GB in the SLURM header:

`#SBATCH --mem=180gb`

Based on the memory request, my job is pending until Dec. 6 So I guess I'll check in then.

### 2025-12-08

Job ran for 24 hours but was killed due to time restrictions! I'm not sure why this happened when I had an "unlimited" time restriction information in my SLURM header.

```
(base) [yaamini.venkataraman@poseidon-l1 ~]$ tail /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/yrv_trinity1792444.log
CMD: touch left.fa.K25.stats.sort.ok
CMD finished (0 seconds)
CMD: touch right.fa.K25.stats.sort.ok
CMD finished (0 seconds)
-defining normalized reads
CMD: /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/util/..//util/support_scripts//nbkc_merge_left_right_stats.pl --left left.fa.K25.stats.sort --right right.fa.K25.stats.sort --sorted > pairs.K25.stats
-opening left.fa.K25.stats.sort
-opening right.fa.K25.stats.sort
-done opening files.
slurmstepd: error: *** JOB 1792444 ON pn076 CANCELLED AT 2025-12-06T15:33:00 DUE TO TIME LIMIT ***
```

The response from WHOI IT:

> I just edited your submit script - there were several hidden characters throughout that may have been getting in the way of Slurm parsing the header. Sometimes when scripts are copied/pasted to/from Windows hidden characters come over... you can clean these up using vi and the :set list command. Please try to resubmit when you can, and we can keep an eye on how it progresses.

I resumbmitted the script and let's see what happens.

### 2025-12-09

Realized there was a follow-up message from WHOI IT!

> Hi Yaamini, looking over your submit script again, I realized you do not have a time statement - so even though you are using the unlim QOS, the default time limit of 24 hours is going to be applied. If you add the line to your slurm header like so it should be able to exceed 24hours: #SBATCH --time=2-24:25:00
The time format is d-hh:mm:ss
Apologies that I didn't notice this earlier!

I first checked the log to see the progress out of curiosity (script had been running for 23 hours):

```
(base) [yaamini.venkataraman@poseidon-l1 ~]$ tail /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/yrv_trinity1795649.log
-capturing normalized reads from: /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/25-055_R2_001_val_2_val_2.fq.gz
-capturing normalized reads from: /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/25-055_R1_001_val_1_val_1.fq.gz
-capturing normalized reads from: /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/25-056_R2_001_val_2_val_2.fq.gz
-capturing normalized reads from: /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/25-056_R1_001_val_1_val_1.fq.gz
-capturing normalized reads from: /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/25-058_R2_001_val_2_val_2.fq.gz
-capturing normalized reads from: /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/25-058_R1_001_val_1_val_1.fq.gz
-capturing normalized reads from: /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/25-059_R2_001_val_2_val_2.fq.gz
-capturing normalized reads from: /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/25-059_R1_001_val_1_val_1.fq.gz
-capturing normalized reads from: /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/25-060_R2_001_val_2_val_2.fq.gz
-capturing normalized reads from: /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06b-trimgalore/trim-illumina-polyA/25-060_R1_001_val_1_val_1.fq.gz
```

I then killed the script and modified the SLURM header:

```
#!/bin/bash

#SBATCH --partition=compute          								 				  								  	     		  # Queue selection
#SBATCH --job-name=yrv_trinity        							 															          # Job name
#SBATCH --mail-type=ALL              							   				     									     		  # Mail events (BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=yaamini.venkataraman@whoi.edu    				    									    		  # Where to send mail
#SBATCH --nodes=1                                                									          # One node
#SBATCH --exclusive                                                 								        # All 36 procs on the one node
#SBATCH --mem=180gb                                                 								        # Job memory request
#SBATCH --qos=unlim            								   															     	       	# Unlimited time allowed
#SBATCH --time=10-00:00:00           								   															     	  # Time limit (d-hh:mm:ss)
#SBATCH --output=yrv_trinity%j.log  								   															     		# Standard output/error
#SBATCH --chdir=/vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity	  # Working directory for this script
```

The revised script is now running! I'm going to keep an eye on it to ensure that it actually continues running the way it should.

### 2025-12-10

The script died after ~15 hours *lolsob*. Looks like `inchworm` was starting when the error occured:

```
--------------- Inchworm (K=25, asm) ---------------------
-- (Linear contig construction from k-mers) --
----------------------------------------------

* [Wed Dec 10 01:12:30 2025] Running CMD: /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/Inchworm/bin//inchworm --kmers jellyfish.kmers.25.asm.fa --run_inchworm -K 25 --monitor 1   --num_threads 6  --PARALLEL_IWORM   --min_any_entropy 1.0   -L 25  --no_prune_error_kmers  > /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/inchworm.fa.tmp
Kmer length set to: 25
Min assembly length set to: 25
Monitor turned on, set to: 1
min entropy set to: 1
setting number of threads to: 6
-setting parallel iworm mode.
-reading Kmer occurrences...
 [2710M] Kmers parsed.     
 done parsing 2710642398 Kmers, 2710642398 added, taking 8912 seconds.

TIMING KMER_DB_BUILDING 8912 s.
Pruning kmers (min_kmer_count=1 min_any_entropy=1 min_ratio_non_error=0.005)
Pruned 15962305 kmers from catalog.
	Pruning time: 1573 seconds = 26.2167 minutes.

TIMING PRUNING 1573 s.
-populating the kmer seed candidate list.
Kcounter hash size: 2710642398
terminate called after throwing an instance of 'std::bad_alloc'
  what():  std::bad_alloc
sh: line 1: 185644 Aborted                 /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/Inchworm/bin//inchworm --kmers jellyfish.kmers.25.asm.fa --run_inchworm -K 25 --monitor 1 --num_threads 6 --PARALLEL_IWORM --min_any_entropy 1.0 -L 25 --no_prune_error_kmers > /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/inchworm.fa.tmp
Error, cmd: /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/Inchworm/bin//inchworm --kmers jellyfish.kmers.25.asm.fa --run_inchworm -K 25 --monitor 1   --num_threads 6  --PARALLEL_IWORM   --min_any_entropy 1.0   -L 25  --no_prune_error_kmers  > /scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/trinity_out_dir/inchworm.fa.tmp died with ret 34304 No such file or directory at /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin/PerlLib/Pipeliner.pm line 187.
	Pipeliner::run(Pipeliner=HASH(0x55555558a8d8)) called at /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin//Trinity line 2729
	eval {...} called at /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin//Trinity line 2719
	main::run_inchworm("/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinit"..., "/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinit"..., "RF", "", 25, 0) called at /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin//Trinity line 1836
	main::run_Trinity() called at /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin//Trinity line 1500
	eval {...} called at /vortexfs1/home/yaamini.venkataraman/.conda/envs/trinity_env/bin//Trinity line 1499



If it indicates bad_alloc(), then Inchworm ran out of memory.  You'll need to either reduce the size of your data set or run Trinity on a server with more memory available.
```

I need more memory! I will follow advice from Rich of WHOI IT to use a node with a higher memory allocation:

> If you still are running out of memory at 180GB, there is a medium memory partition with nodes that have 512GB - if you need to use those, the partition name is medmem - simply replace compute with medmem in the --partition= statement...

I modified my SLURM header as follows:

```
#!/bin/bash

#SBATCH --partition=medmem          								 				  								  	     		    # Queue selection
#SBATCH --job-name=yrv_trinity        							 															        # Job name
#SBATCH --mail-type=ALL              							   				     									     		    # Mail events (BEGIN, END, FAIL, ALL)
#SBATCH --mail-user=yaamini.venkataraman@whoi.edu    				    									    		    # Where to send mail
#SBATCH --nodes=1                                                									            # One node
#SBATCH --exclusive                                                 								          # All 36 procs on the one node
#SBATCH --mem=500gb                                                 								          # Job memory request
#SBATCH --qos=unlim            								   															     	    	# Unlimited time allowed
#SBATCH --time=10-00:00:00           								   															     	    	# Time limit (d-hh:mm:ss)
#SBATCH --output=yrv_trinity%j.log  								   															     		# Standard output/error
#SBATCH --chdir=/vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity	  # Working directory for this script
```

The job is now in the queue. Given how much memory is needed for the run, I am not sure when it will start so I'll need to keep my eye on my email.

### Going forward

1. Index, get advanced transcriptome statistics, and pseudoalign with `salmon`
4. Clean transcriptome with `EnTap` and `blastn`
4. Identify differentially expressed genes
5. Additional strand-specific analysis in the supergene region
2. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper
6. Start methods and results of 2023 paper

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
