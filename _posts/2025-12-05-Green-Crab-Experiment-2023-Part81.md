---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 80
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
