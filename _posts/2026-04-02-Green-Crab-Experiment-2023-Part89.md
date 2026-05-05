---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 89
tags: green-crab-wc RNA-Seq EnTAP
---

## Troubleshooting `EnTAP` analysis

Somehow `EnTAP` is still taking forever even though I've changed the threads to 35 in multiple places! I need to do the following:

1. Figure out how to get `EnTAP` to run quickly
2. Ensure that it's actually running properly

### 2026-04-02

Looking at the more detailed `EnTAP` file, I saw the following:

```
Thu Mar 26 00:33:25 2026: Verifying previous execution of database: entap_outfiles/bin/nr.dmnd...
Thu Mar 26 00:33:25 2026: File for database nr does not exist.
/scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/similarity_search/DIAMOND/blastp_transcriptome_contamRemoved_final_nr.out
Thu Mar 26 00:33:25 2026: Deleting file at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/similarity_search/DIAMOND/blastp_transcriptome_contamRemoved_final_nr.out
Thu Mar 26 00:33:25 2026: Verifying previous execution of database: entap_outfiles/bin/refseq_complete.dmnd...
Thu Mar 26 00:33:25 2026: File for database refseq_complete does not exist.
/scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/similarity_search/DIAMOND/blastp_transcriptome_contamRemoved_final_refseq_complete.out
Thu Mar 26 00:33:25 2026: Verifying previous execution of database: entap_outfiles/bin/uniprot_sprot.dmnd...
Thu Mar 26 00:33:25 2026: File for database uniprot_sprot does not exist.
/scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/similarity_search/DIAMOND/blastp_transcriptome_contamRemoved_final_uniprot_sprot.out
Thu Mar 26 00:33:25 2026: Verifying previous execution of database: entap_outfiles/bin/uniprot_trembl.dmnd...
Thu Mar 26 00:33:25 2026: File for database uniprot_trembl does not exist.
/scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/similarity_search/DIAMOND/blastp_transcriptome_contamRemoved_final_uniprot_trembl.out
Thu Mar 26 00:33:25 2026: Verifying previous execution of database: entap_outfiles/bin/uniref90_rf.dmnd...
Thu Mar 26 00:33:25 2026: File for database uniref90_rf does not exist.
/scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/similarity_search/DIAMOND/blastp_transcriptome_contamRemoved_final_uniref90_rf.out
Thu Mar 26 00:33:25 2026: Success! Verified files for DIAMOND, continuing...
Thu Mar 26 00:33:25 2026: Executing DIAMOND for necessary files....
Thu Mar 26 00:33:25 2026: File not found, executing against database at: entap_outfiles/bin/nr.dmnd
Thu Mar 26 00:33:25 2026: Executing command:
/vortexfs1/home/yaamini.venkataraman/EnTAP-v2.3.0/libs/diamond-2.1.8/bin/diamond blastp -o /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/similarity_search/DIAMOND/blastp_transcriptome_contamRemoved_final_nr.out -f 6 qseqid sseqid pident length mismatch gapopen qstart qend sstart send evalue bitscore qcovhsp stitle --very-sensitive -p 35 --max-target-seqs 3 --evalue 0.000010 -d entap_outfiles/bin/nr.dmnd -q /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/transcriptomes/transcriptome_contamRemoved_final.fasta --subject-cover 50.000000 --query-cover 50.000000
```

It's possible that the reason why the `diamond` run is taking so long is just because I have a lot of contigs and the non-redundant database is also quite large. I asked Zac about his run, and he said it didn't take him that long. Another thing that may be taking up time is frame selection. My run is doing frame selection, but I didn't see a frame selection indication on Zac's or Rayna's run. I asked both of them if they used frame selection or not. Talking to Rayna about how to interpret the new syntax versus the default `-runN` that Carolyn recommends for most transcriptomes with the previous `EnTAP` version, it seems like a good course of action is to use `frame-selection = false`. I cancelled the existing job, then made the following changes in my run parameter file:

```
#Switching overwrite to TRUE and resume to FALSE. This way, all previous runs that included frame selection will be overwritten.

#Select this option if you would like to overwrite files from a previous execution of EnTAP. This will supercede the 'resume' flag which enables you to continue an annotation from$
#type:boolean (true/false)
overwrite=true
#Select this option if you would like EnTAP to continue execution if existing files are found from a previous run. If false is selected, EnTAP will stop execution once it finds fi$
#type:boolean (true/false)
resume=false
```

```
#Removing the trailing comma after 7

#type:list (integer)
output-format=1,3,4,7
```

```
#Switching frame-selection = false to better match runN defaults in previous EnTAP versions

#-------------------------------
# [frame_selection]
#-------------------------------
#Specify if you would like to perform frame selection/gene prediction on your input nucleotide transcriptome. This flag will be ignored if you input protein sequences. If this is $
#type:boolean (true/false)
frame-selection=false
```

I then started a new job with the modified run parameters. I confirmed that frame selection was skipped:

```
Thu Apr  2 14:32:25 2026: STATE - FRAME SELECTION
Thu Apr  2 14:32:25 2026: Determining if we want to run frame selection...
Thu Apr  2 14:32:25 2026: NO, nucleotide data, with no frame selection specified
```

Similarity search is now running on the first `diamond` database. Let's see how long this takes!

### 2026-04-09

I talked to Zac more and he suggested retaining the longest isoform of each gene prior to the `EnTAP` annotation. By paring down the number of transcripts that need to be annotated, my job would likely take less time. Zac did something similar in [his script](https://github.com/tepoltlab/RhithroLoxo_DE/blob/master/Snakefile#L300), so I revisited it.

```
#retain only longest isoform per 'gene'

rule txm_long:
    input:
        txm = 'txms/rhithro/trinity_kmer25cov1.Trinity.fasta',
        script1 = {'scripts/list-trinity-dups-to-remove.py'},
        script2 = {'scripts/fasta_subsetter.py'}
    output:
        txm_long = TXM_LONG
    shell:
        """
        python {input.script1} {input.txm}
        python {input.script2} {input.txm} txms/rhithro/trinity_kmer25cov1.Trinity.fasta_dups.txt REMOVE
        mv txms/rhithro/trinity_kmer25cov1.Trinity.fasta_dups_REMOVE.fasta {output.txm_long}
        """
```

Looks like he used [this `python` script](https://github.com/tepoltlab/RhithroLoxo_DE/blob/master/scripts/list-trinity-dups-to-remove.py) Carolyn wrote to retain only the longest isoform. I created a version of it and saved it [here](https://github.com/yaaminiv/wc-green-crab/blob/main/code/06e-list-trinity-dups-to-remove.py). I then added this code to my `EnTAP` script:

```
# Retain only the longest isoform for annotation - significantly speeds up annotation process
python ${HOME_DIR}/06e-list-trinity-dups-to-remove.py ${BLAST_DIR}/transcriptome_contamRemoved.fasta
python ${HOME_DIR}/06d-fasta_subsetter.py ${BLAST_DIR}/transcriptome_contamRemoved.fasta ${BLAST_DIR}/transcriptome_contamRemoved.fasta_dups.txt REMOVE
mv ${BLAST_DIR}/transcriptome_contamRemoved.fasta ${BLAST_DIR}/transcriptome_contamRemoved_dupRemoved.fasta
```

After it ran, I confirmed that the list of duplicated sequences made sense:

> (base) [yaamini.venkataraman@poseidon-l2 06d-blast]$ head transcriptome_contamRemoved.fasta_dups.txt
>TRINITY_DN0_c0_g1_i11
>TRINITY_DN0_c0_g1_i13
>TRINITY_DN0_c0_g1_i12
>TRINITY_DN0_c0_g1_i4
>TRINITY_DN0_c0_g1_i5
>TRINITY_DN0_c0_g1_i2
>TRINITY_DN0_c0_g1_i7
>TRINITY_DN0_c0_g1_i6
>TRINITY_DN0_c0_g1_i9
>TRINITY_DN0_c0_g2_i2

> (base) [yaamini.venkataraman@poseidon-l2 06d-blast]$ grep ">" transcriptome_contamRemoved_dupRemoved.fasta | wc -l
597666
(base) [yaamini.venkataraman@poseidon-l2 06d-blast]$ grep ">" transcriptome_contamRemoved.fasta | wc -l
858056

It seems ~250000 sequences were removed. I then made the following modifications under `general` in the `EnTAP` run parameter file:

```
input=/scratch/yaamini.venkataraman/wc-green-crab/output/06d-blast/transcriptome_contamRemoved_dupRemoved.fasta
```

I ran the script, bumping the time up to 14 days. Since I already changed settings earlier to not resume previous runs and overwrite files, I should be good to go. Here's hoping I actually make it through annotation...

### 2026-04-13

Alright, we're still on the first database! My new theory is that maybe the non-redundant protein sequences aren't what I think they are. [When I downloaded the sequences](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part86/), I noticed that the `complete.nonredundant_protein` files no longer exist and just downloaded the protein reference that I could. Looking at [Rayna's config file](https://github.com/tepoltlab/Hemigrapsus_transcriptome_pipeline/blob/main/3_filter_transcriptome/config.yaml), it seems like there may already be `diamond` databases present in the Tepolt lab folder. A good first step would be for me to see if there is a discrepancy in number of entries between the `diamond` database that I'm using and the one in the Tepolt Lab folder. The database in the Tepolt lab folder has a size of 378,461,519,025, while the one I'm using has a size of 371,476,293,189.

Okay, so the file size is not drastically different...there goes that theory. I'll check with Rayna to see if there are other things I should try.

### 2026-04-17

Rayna did some independent testing of my files. She initially thought that `diamond` exited because the temporary files that get updated throughout the run stopped updating on April 9. After testing the files some more, she determined that the `EnTAP` run was completing with smaller databases, but not the very large non-redundant protein database. She suggested I change to the `bigmem` partition with 1000G of memory to run the script. I cancelled my existing job, made the change to the SLURM header, and added the job to the queue.

### 2026-04-23

After being stuck in the queue for several days, `EnTAP` is running! I'm going to let it run through today and if the files don't seem like they're updating any more, I'll reach out to Rayna about splitting the transcriptome into different chunks.

### Going forward

1. Annotate transcriptome with `EnTAP`
3. Quantify transcripts with `salmon`
1. Use `DESeq2` for preliminary temperature-specific gene expression analysis
2. Determine methods for functional analysis
3. Compare `DESeq2` with `edgeR` and pick a method to move forward with
4. Repeat analysis with clean transcriptome and fuller annotations
5. Identify temperature- and genotype-specific differentially expressed genes at the end of the experiment
6. Identify genes influenced by both temperature and time
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
