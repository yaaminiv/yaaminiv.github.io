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
