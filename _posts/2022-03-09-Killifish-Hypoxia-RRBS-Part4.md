---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 4
tags: killifish RRBS poseidon BAT
---

## Continuing mapping tests

### Finishing test mapping

I ran [my test mapping script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-alignment.sh) for 10 hours. In those 10 hours, one sample finished mapping and another sample's mapping was truncated. I removed the sample that finished mapping from the script, extended my time on the cluster to 24 hours (the maximum time I can use without asking permission), and kept running the script. After 24 hours, it looked like the remaining three samples had mapped. Based on this information, I can conservatively say that mapping 48 samples will require 16 days on the cluster using 16 threads. Yikes.

Once I finished mapping all the samples, I needed to get mapping statistics with `BAT_mapping_stat`. The command provides the following information:

> Statistics
1. Total amount of mappings: Number of read alignments. Possibly larger than number of mapped reads due to multiple aligned reads.
2. Mapped reads: Number of aligned reads subdivided into paired-end reads aligned as pair that were aligned once (unique paired-end) or multiple times (multiple paired ends), paired-end reads where only one end was aligned once (unique one mate) or multiple times (multiple one mate), as well as into single-end reads that were aligned once (unique single-end) or multiple times (multiple single-end).
3. Sum of all mapped reads: and if paired ends sum of all mapped fragments
Amount of split and not split mates: only if split reads are present
4. Frequencies of multiple hits: List of number of hits and the frequency of reads with this number of hits; separately for paired-end reads and one mates and single-end reads. Format: nr_of_hits <tab> frequency.
5. Frequencies of the e-distances: List of edit distance and the frequency of non-split read alignments with this edit distance. Format: e_distance <tab> frequency.
6. Frequency of number of split fragments: List of number of split-read fragments and the frequency of read alignments that are split into this number of fragments. Format: nr_split_fragments <tab> frequency.

The code was pretty simple to put together for each sample:

```
singularity exec --bind /vortexfs1/home/naluru/:/naluru,/vortexfs1/scratch/yaamini.venkataraman:/scratch /vortexfs1/home/naluru/bat_latest.sif \
BAT_mapping_stat \
--bam /scratch/03-mapping/190626_I114_FCH7TVNBBXY_L2_20-N4_nondirectional.bam \
--excluded /scratch/03-mapping/190626_I114_FCH7TVNBBXY_L2_20-N4_nondirectional.excluded.bam \
--fastq /scratch/02-directional/190626_I114_FCH7TVNBBXY_L2_20-N4_1_val_1.fq.gz \
> /scratch/03-mapping/190626_I114_FCH7TVNBBXY_L2_20-N4_nondirectional.stat
```

When I ran the code, I got an error:

```
/cm/local/apps/slurm/var/spool/job2414799/slurm_script: line 26: /scratch/03-mapping/190626_I114_FCH7TVNBBXY_L2_20-N4_nondirectional.stat: No such file or directory
```

Of course the file doesn't exist — I'm trying to create it! I didn't get any information from the [`BAT_mapping_stat`](https://github.com/helenebioinf/BAT/blob/master/BAT_mapping_stat) source code. Since the error was coming from SLURM, it made me think that I needed to include the path on my host system since I was writing standard output, not referring to a path for the output within the `singularity` container. When I changed the path, I was able to get mapping statistics. It took 30 minutes to get mapping statistics for 4 samples, so it should be 6 hours for all 48 samples.

### Directional vs. nondirectional trimming

I needed the mapping statistics to determine if the samples were directional or non-directional! I used `head` to get a glimpse at mapping efficiency:

```
base) [yaamini.venkataraman@poseidon-l1 03-mapping]$ head *stat
==> 190626_I114_FCH7TVNBBXY_L2_20-N4_directional.stat <==
/usr/bin/R
-----------------------------
--------   SUMMARY   --------
-----------------------------
total amount of MAPPINGS:	0
total amount of query READS:	31980722

MAPPED READS:
unique paired end:	0 (0%)
multiple paired end:	0 (0%)

==> 190626_I114_FCH7TVNBBXY_L2_20-N4_nondirectional.stat <==
/usr/bin/R
-----------------------------
--------   SUMMARY   --------
-----------------------------
total amount of MAPPINGS:	156239942
total amount of query READS:	31980722

MAPPED READS:
unique paired end:	22779198 (71.23%)
multiple paired end:	3751151 (11.73%)

==> 190626_I114_FCH7TVNBBXY_L3_OC-S3_directional.stat <==
/usr/bin/R
-----------------------------
--------   SUMMARY   --------
-----------------------------
total amount of MAPPINGS:	0
total amount of query READS:	23136011

MAPPED READS:
unique paired end:	0 (0%)
multiple paired end:	0 (0%)

==> 190626_I114_FCH7TVNBBXY_L3_OC-S3_nondirectional.stat <==
/usr/bin/R
-----------------------------
--------   SUMMARY   --------
-----------------------------
total amount of MAPPINGS:	131047078
total amount of query READS:	23136011

MAPPED READS:
unique paired end:	16685092 (72.12%)
multiple paired end:	3182660 (13.76%)
```

Based on the percent of mapped reads, it's pretty obvious that I have nondirectional data. I'll use nondirectional trimming for the last sample when Neel uploads it (he thinks that file transfer may have been interrupted and that's why I have a truncated sample), then use nondirectional trimmed files and associated parameters with `BAT_mapping`. I added the statistical information and distribution of mapped reads (pdfs) to [this repository folder](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/03-mapping/test).

### Going forward

1. Trim sample 190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz after Neel reloads it
2. Get sample metadata to write `BAT_merging` code (equivalent to `methylKit::unite`)
3. Start alignment with all samples

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
