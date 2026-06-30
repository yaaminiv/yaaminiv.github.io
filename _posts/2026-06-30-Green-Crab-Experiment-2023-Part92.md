---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 91
tags: green-crab-wc RNA-Seq
---

## Modifying the transcriptome assembly

I ran my action plan by Carolyn. She didn't provide any comments on which samples to input, and said my plan of running the assembly with the correct library type was a good first step! I started a script for 15 days to run the transcriptome assembly and collapse the isoforms into supertranscripts.

```
# DE NOVO TRANSCRIPTOME ASSEMBLY

echo "Start de novo transcriptome assembly"

# Run Trinity to assemble de novo transcriptome. Using primarily default parameters.
${TRINITY}/Trinity \
--seqType fq \
--max_memory 100G \
--samples_file ${trinity_file_list} \
--SS_lib_type FR \
--min_contig_length 200 \
--full_cleanup \
--CPU 28

# Move transcriptome to the correct location
mv trinity_out_dir.Trinity.fasta ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta

# Collapse isoforms into supertranscripts.
# Output files are trinity_genes.fasta (supertranscripts in fasta format), trinity_genes.gtf (transcript structure annotation in gtf format), and trinity_genes.malign (multiple alignment view that contrasts the different candidate splicing isoforms)
${TRINITY}/Analysis/SuperTranscripts/Trinity_gene_splice_modeler.py \
--trinity_fasta ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
--incl_malign

# Move output files to a new folder
mkdir supertranscript_output
rsync --archive --progress --verbose trinity_genes.* supertranscript_output/.

echo "Completed de novo transcriptome assembly"
```

The job was pending due to resources, but hopefully it starts sooner rather than later!

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
