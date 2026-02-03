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

### 2026-02-03

...and my code errored out almost immediately because I didn't give SLURM a path to a working directory that existed. I fixed that and confirmed `blastn` is running:

> (base) [yaamini.venkataraman@poseidon-l1 ~]$ head /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06d-blast/blast-results/transcriptome-contam.tab
TRINITY_DN11_c0_g1_i15	gi|1379041273|gb|CP028800.1|	95.569	1354	54	6	1	1352	2593208	2591859	0.0	2163	Acinetobacter junii strain WCHAJ59 chromosome, complete genome	40215
TRINITY_DN11_c0_g1_i2	gi|359551946|gb|JN869067.1|	93.827	891	47	8	243	1129	1	887	0.0	1334	Uncultured bacterium clone AN62 16S ribosomal RNA gene, partial sequence	77133
TRINITY_DN11_c0_g1_i21	gi|1445136029|ref|NR_158069.1|	92.111	1369	92	14	297	1659	1	1359	0.0	1916	Nocardioides agrisoli strain djl-8 16S ribosomal RNA, partial sequence	1882242
TRINITY_DN11_c0_g1_i24	gi|404428310|gb|JQ516465.1|	94.310	580	33	0	2	581	465	1044	0.0	889	Uncultured Rhizobiales bacterium clone 0907_Mf_HT2_B11 16S ribosomal RNA gene, partial sequence	208549
TRINITY_DN11_c0_g1_i25	gi|684780902|gb|KJ995964.1|	88.112	1388	151	14	297	1677	3	1383	0.0	1637	Uncultured bacterium clone C-147 16S ribosomal RNA gene, partial sequence	77133
TRINITY_DN11_c0_g1_i26	gi|1233268392|gb|KX771411.1|	99.163	239	2	0	1	239	903	1141	5.39e-117	431	Uncultured bacterium clone 01G_N5Kili 16S ribosomal RNA gene, partial sequence	77133
TRINITY_DN11_c0_g1_i28	gi|1379041273|gb|CP028800.1|	90.559	2288	174	23	533	2787	2594137	2591859	0.0	2990	Acinetobacter junii strain WCHAJ59 chromosome, complete genome	40215
TRINITY_DN11_c0_g1_i3	gi|359551675|gb|JN868796.1|	87.857	1367	125	23	297	1659	1	1330	0.0	1567	Uncultured bacterium clone MW25 16S ribosomal RNA gene, partial sequence	77133
TRINITY_DN11_c0_g1_i31	gi|459218310|gb|KC442851.1|	88.408	1087	86	20	297	1378	1	1052	0.0	1273	Uncultured bacterium clone A40 16S ribosomal RNA gene, partial sequence	77133
TRINITY_DN11_c0_g1_i35	gi|643391102|gb|KF070860.1|	88.406	1242	111	28	317	1543	1	1224	0.0	1465	Uncultured bacterium clone ncd135b03c1 16S ribosomal RNA gene, partial sequence	77133

> (base) [yaamini.venkataraman@poseidon-l1 ~]$ wc -l /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06d-blast/blast-results/transcriptome-contam.tab
7834 /vortexfs1/scratch/yaamini.venkataraman/wc-green-crab/output/06d-blast/blast-results/transcriptome-contam.tab

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
