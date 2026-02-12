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

### 2026-02-06

My `blastn` job finished! There are 115,204 results. The next step is to use the python scripts to clean the transcriptome. I decided to just...run the scripts as is this first time around to see what would happen. I made copies of both `python` scripts. When I was looking at [Zac's Snakefile](https://github.com/tepoltlab/RhithroLoxo_DE/blob/master/Snakefile), I noticed that there was a [specific `conda` environment](https://github.com/tepoltlab/RhithroLoxo_DE/blob/master/envs/ete3.yaml) he used to run the first python script. I needed to figure out how to replicate that `conda` environment before I could run the script.

A quick search provided this [Stack Overflow](https://stackoverflow.com/questions/44742138/how-to-create-a-conda-environment-based-on-a-yaml-file) link with the following solution:

`conda env create --file=myfile.yaml`

### 2026-02-11

Picking pu where I left off last week!

I used `wget` to download the YAML file onto poseidon, then ran `conda env create --file=ete3.yaml`.

I got this error as it was solving the environment:

> (base) [yaamini.venkataraman@poseidon-l1 ~]$ conda env create -f ete3.yaml
Collecting package metadata (repodata.json): done
Solving environment: failed
ResolvePackageNotFound:
  - openssl==1.1.1=h7b6447c_0

I modified the YAML file to remove "=h7b6447c_0" from the version strings, since that may be specific to the Poseidon system when Zac built the file (according to Gemini). Just removing the version strings for that one package worked! I then started running the first `python` script.

> Uploading to /vortexfs1/home/yaamini.venkataraman/.etetoolkit/taxa.sqlite
Traceback (most recent call last):
  File "/vortexfs1/home/yaamini.venkataraman/06d-map_contam_ids.py", line 4, in <module>
    ncbi = NCBITaxa()
  File "/vortexfs1/home/yaamini.venkataraman/.conda/envs/ete3/lib/python3.7/site-packages/ete3/ncbi_taxonomy/ncbiquery.py", line 110, in __init__
    self.update_taxonomy_database(taxdump_file)
  File "/vortexfs1/home/yaamini.venkataraman/.conda/envs/ete3/lib/python3.7/site-packages/ete3/ncbi_taxonomy/ncbiquery.py", line 129, in update_taxonomy_database
    update_db(self.dbfile)
  File "/vortexfs1/home/yaamini.venkataraman/.conda/envs/ete3/lib/python3.7/site-packages/ete3/ncbi_taxonomy/ncbiquery.py", line 760, in update_db
    upload_data(dbfile)
  File "/vortexfs1/home/yaamini.venkataraman/.conda/envs/ete3/lib/python3.7/site-packages/ete3/ncbi_taxonomy/ncbiquery.py", line 802, in upload_data
    db.execute("INSERT INTO synonym (taxid, spname) VALUES (?, ?);", (taxid, spname))
sqlite3.IntegrityError: UNIQUE constraint failed: synonym.spname, synonym.taxid

I used Gemini to help me troubleshoot what to do. It seems like the code is erroring out when it tries and updates the `blast` database. Gemini suggested doing a manual update of the database prior to running the script to avoid any timeout issues on the cluster:

```
python -c "from ete3 import NCBITaxa; ncbi = NCBITaxa(); ncbi.update_taxonomy_database()"
```

When I ran this code, I still got the error! It seems like the `ete3` version I'm using cannot handle the current format of the NCBI taxonomy database without triggering a "Duplicate Entry" error. I need to first fix that issue:

```
sed -i 's/INSERT INTO synonym (taxid, spname) VALUES (?, ?);/INSERT OR IGNORE INTO synonym (taxid, spname) VALUES (?, ?);/' /vortexfs1/home/yaamini.venkataraman/.conda/envs/ete3/lib/python3.7/site-packages/ete3/ncbi_taxonomy/ncbiquery.py
```

Once I ran the `sed` command, I reran the manual python script update. It finished without error:

> (ete3) [yaamini.venkataraman@poseidon-l1 ~]$ python -c "from ete3 import NCBITaxa; ncbi = NCBITaxa(); ncbi.update_taxonomy_database()"
NCBI database not present yet (first time used?)
Downloading taxdump.tar.gz from NCBI FTP site (via HTTP)...
Done. Parsing...
Loading node names...
2723150 names loaded.
428355 synonyms loaded.
Loading nodes...
2723150 nodes loaded.
Linking nodes...
Tree is loaded.
Updating database: /vortexfs1/home/yaamini.venkataraman/.etetoolkit/taxa.sqlite ...
 2723000 generating entries...
Uploading to /vortexfs1/home/yaamini.venkataraman/.etetoolkit/taxa.sqlite
Inserting synonyms:      425000
Inserting taxid merges:  95000
Inserting taxids:       2720000
Downloading taxdump.tar.gz from NCBI FTP site (via HTTP)...
Done. Parsing...
Loading node names...
2723150 names loaded.
428355 synonyms loaded.
Loading nodes...
2723150 nodes loaded.
Linking nodes...
Tree is loaded.
Updating database: /vortexfs1/home/yaamini.venkataraman/.etetoolkit/taxa.sqlite ...
 2723000 generating entries...
Uploading to /vortexfs1/home/yaamini.venkataraman/.etetoolkit/taxa.sqlite
Inserting synonyms:      425000
Inserting taxid merges:  95000
Inserting taxids:       2720000

I then reran the the `bash` script that runs the `python` script! I now got a new error:

> Traceback (most recent call last):
  File "/vortexfs1/home/yaamini.venkataraman/06d-map_contam_ids.py", line 17, in <module>
    lineage = ncbi.get_lineage(taxid)
  File "/vortexfs1/home/yaamini.venkataraman/.conda/envs/ete3/lib/python3.7/site-packages/ete3/ncbi_taxonomy/ncbiquery.py", line 238, in get_lineage
    raise ValueError("%s taxid not found" %taxid)
ValueError: 1859514 taxid not found

Gemini suggested adding this to the `python` script:

```
contam = []
for i in hits.index:
    contig = hits.loc[i,0]
    taxid = hits.loc[i,13]
    try:
        lineage = ncbi.get_lineage(taxid)
    except ValueError:
        print(f"Warning: TaxID {taxid} not found in local database. Skipping.")
        lineage = [] # Or handle as "Unknown"
    if bool(set(lineage) & set(candidate_contam)):
        contam.append(contig)
```

Once again, I revised my script and now it's running.

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
