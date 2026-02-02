---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 84
tags: green-crab-wc RNA-Seq EnTap blastn
---

## Transcriptome cleaning and annotation

Now that I have my transcriptome assembled and [have assembly statistics](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part84/), my next step in this workflow is to clean and annotate the transcriptome.

### 2026-02-02

According to Zac's paper, I'll first need to `blastn` the transcriptome and screen for contaminant taxa:

> Contigs were then queried against the NCBI nt database with blastn using blast+ version 2.7.1 (Camacho et al., 2009) to screen for potential contaminant taxa including fungi, bacteria, platyhelminthes and nematoda. Contigs with alignments to contaminant taxa references sequences with e-values of 1−10 or lower were removed.

I modified [Zac's code](https://github.com/tepoltlab/RhithroLoxo_DE/blob/master/Snakefile#L336), choosing to `blastn` the whole transcriptome as opposed to doing it in chunks since I don't have a time limit:

```
# Blast transcriptome against nt. database from Dec 31 2018 for initial round of contaminant filtering
mkdir ${OUTPUT_DIR}/blast-results

module load bio blast/2.7.1

blastn -query ${TRINITY_DIR}/trinity_out_dir/Trinity.fasta \
-db nt \
-evalue 1e-10 \
-max_target_seqs 1 \
-max_hsps 1 \
-num_threads 35 \
-outfmt "6 qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore stitle staxids" \
-out ${OUTPUT_DIR}/blast-results/transcriptome-contam.tab
```

The script was pending after submission so I decided to look at the next steps of code I'd need to run:

```
rule id_contam:
    input: 
        BLAST_CONTAM_MERGED,
        'scripts/map_contam_ids.py'
    output:
        BLAST_CONTAM_LIST
    conda:
        "envs/ete3.yaml"
    shell:
        """
        python {input[1]} {input[0]} {output}
        """

#remove contam contigs

rule remove_blast_contam:
    input:
        txm = TXM_LONG,
        contam_list = BLAST_CONTAM_LIST,
        script = {'scripts/fasta_subsetter.py'}
    output:
        txm_long = TXM_LONG_CLEAN
    shell:
        """
        python {input.script} {input.txm} {input.contam_list} REMOVE
        mv outputs/blast/contam_lis_REMOVE.fasta {output.txm_long} #'contam_lis' instead of 'contam_list' because .strip() bug in fasta_subsetter. ignore for now, fix later
        """
```

Alright, I'd need my transcriptome, a list of contaminant information, and a script to subset the FASTA into a clean transcriptome. I found the python script in [Zac's Github repository](https://github.com/tepoltlab/RhithroLoxo_DE/blob/master/scripts/fasta_subsetter.py). To create the list of contaminant information, I'd need [this python script](https://github.com/tepoltlab/RhithroLoxo_DE/blob/master/scripts/map_contam_ids.py). The contaminant IDs appear to be manually curated, so that will take some time to poke through the `blastn` results and verify the IDs.

It's also probably worth creating a new script and subdirectory (`06d-blast`) for this step to avoid having the world's longest script. So, I did that! The new script can be found [here](https://github.com/yaaminiv/wc-green-crab/blob/main/code/06d-blast.sh).

### Going forward

1. Clean transcriptome with `blastn`
2. Annotate transcriptome with `EnTap`
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
