---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 93
tags: green-crab-wc RNA-Seq trinity
---

## Troubleshooting transcriptome assembly

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
