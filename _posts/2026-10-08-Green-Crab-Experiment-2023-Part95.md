---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 94
tags: green-crab-wc RNA-Seq EnTAP
---

## Returning to EnTAP

### 2026-10-08

Since I have a more reasonable supertranscript assembly, I am going to move onto `EnTAP` annotation. The purpose of this is to identify potential contamination sequences and removed them. Initially, I was following Zac's workflow and doing both an `blast` and `EnTAP` contamination removal, but Carolyn mentioned that I could stick to just an `EnTAP` annotation since I wasn't working with samples with known parasites like Zac was.

Before modifying the script, I activated my `EnTAP` `conda` environment and ensured that I could use pipes by re-installing `conda` libraries:
```
conda activate EnTAP

# Reinstall glibc/GCC compatibility libraries
conda install -c conda-forge libgcc-ng libstdcxx-ng coreutils --force-reinstall -y

# Test if piping works now without error
echo "test" | sort | tee /tmp/test_pipe.txt
```

The libraries installed without any issue. I then returned to [my `EnTAP` script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/06e-entap.sh) and made the following changes:

- Replaced `/vortexfs1/home` with `/user` to follow `hydra` path naming conventions
- Commented out the part of the script that had me remove short isoforms (I have a supertranscript assembly so there are no isoforms) and remove contaminant sequences identified by `blast` (no longer performing this step)
  - I didn't want to remove this for easy access in case I needed to make changes (sure I could go through the Github commit history........but that's more effort)
  - # Retain only the longest isoform for annotation - significantly speeds up annotation process
  # python ${HOME_DIR}/06e-list-trinity-dups-to-remove.py ${BLAST_DIR}/transcriptome_contamRemoved.fasta
  # python ${HOME_DIR}/06d-fasta_subsetter.py ${BLAST_DIR}/transcriptome_contamRemoved.fasta ${BLAST_DIR}/transcriptome_contamRemoved.fasta_dups.txt REMOVE
  # mv ${BLAST_DIR}/transcriptome_contamRemoved.fasta ${BLAST_DIR}/transcriptome_contamRemoved_dupRemoved.fasta

I decided not to re-download databases since they were already present. However, I overconfidently removed the `entap_outfiles` directory from scratch.....without realizing that all of the configuration output ends up there. FML. So I will need to start by reconfiguring `EnTAP`. Annoyingly, `EnTAP` has config and parameter files that need to be modified as well:

Changes made to `entap_config.ini`:
  - Replaced `/vortexfs1/home` with `/user` to follow `hydra` path naming conventions
  - Checked path to apps: /apps/bio/interproscan-5.51-85.0/interproscan.sh
    - The `bio` module no longer exists?? Which is bad because so many of my loaded modules came from there.
    - Used `module avail` to get a list of available modules to load, and I found `transdecoder/5.7.1`. This is the exact version of `transdecoder` needed by `EnTAP` so I modified my main script to load this module.
    - However, could not find `interproscan`, the correct version of `blast`, or `rsem`
    - Submitted a ticket to IS to resolve this issue.

Changes made to `entap_run.params`:
  - Changed path for transcriptome: `/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/filtered_supertranscript_output/filtered_transcripts_min_exp_1.fasta`

### 2026-10-11

I couldn't start the `EnTAP` run because IS was working on installing programs for me since I didn't have all of the modules available. Instead, I modified my `trinity` script to get N50 and ExN50 stats for the TPM > 1 filtered transcriptome:

```
# TRANSCRIPTOME ASSEMBLY STATISTICS FOR FILTERED TRANSCRIPTOME

echo "Start transcript assembly statistic calculations"

# Calculate Ex50 statistics
contig_ExN50_statistic.pl ${OUTPUT_DIR}/supertranscript_output/salmon.isoform.TPM.not_cross_norm \
${OUTPUT_DIR}/filtered_supertranscript_output/filtered_transcripts_min_exp_1.fasta \
| tee ${OUTPUT_DIR}/filtered_supertranscript_output/filtered_min_exp_1_ExN50.stats

# Calculate N50 statistics
TrinityStats.pl ${OUTPUT_DIR}/filtered_supertranscript_output/filtered_transcripts_min_exp_1.fasta \
> ${OUTPUT_DIR}/filtered_supertranscript_output/filtered_min_exp_1_N50.txt

echo "Completed transcript assembly statistic calculations"
```

The script produced the N50 output:

> ## Counts of transcripts, etc.
################################
Total trinity 'genes':	79814
Total trinity transcripts:	79814
Percent GC: 42.65
########################################
Stats based on ALL transcript contigs:
########################################
	Contig N10: 4005
	Contig N20: 2120
	Contig N30: 1158
	Contig N40: 719
	Contig N50: 516
	Median contig length: 296
	Average contig: 476.85
	Total assembled bases: 38059635
#####################################################
## Stats based on ONLY LONGEST ISOFORM per 'GENE':
#####################################################
	Contig N10: 4005
	Contig N20: 2120
	Contig N30: 1158
	Contig N40: 719
	Contig N50: 516
	Median contig length: 296
	Average contig: 476.85
	Total assembled bases: 38059635

However, I did get the following error:

> Error, no seq length for acc: TRINITY_DN218524_c2_g2.  Be sure to give the isoform.TPM expression matrix as input parameter at /user/yaamini.venkataraman/.conda/envs/trinity_env/bin/contig_ExN50_statistic.pl line 73, <$fh> line 3.

This suggests that I need to do something else upstream in order to get the correct ExN50 information. I looked at the input files pre- and post-filtering:

> (base) [yaamini.venkataraman@hydra-l1 06c-trinity]$ cat supertranscript_output/salmon.isoform.TPM.not_cross_norm | grep "TRINITY_DN218524_c2_g2"
TRINITY_DN218524_c2_g2	0	0	0	0	0	0	0	0	0	0	0	0	0
(base) [yaamini.venkataraman@hydra-l1 06c-trinity]$ cat filtered_transcripts_min_exp_1.fasta | grep "TRINITY_DN218524_c2_g2"
cat: filtered_transcripts_min_exp_1.fasta: No such file or directory
(base) [yaamini.venkataraman@hydra-l1 06c-trinity]$ cat filtered_supertranscript_output/filtered_transcripts_min_exp_1.fasta | grep "TRINITY_DN218524_c2_g2"
(base) [yaamini.venkataraman@hydra-l1 06c-trinity]$ cat supertranscript_output/salmon.isoform.TPM.not_cross_norm | grep "TRINITY_DN218524_c2_g2"
TRINITY_DN218524_c2_g2	0	0	0	0	0	0	0	0	0	0	0	0	0

I think the issue is I am using the TPM matrix pre-filtering, but I want to calculate information for the filtered transcriptome. What I need to do is filter the TPM matrix to remove the lines I don't need. I added the following to the code with some assistance from Gemini (note to self: I need to re-familiarize myself with `awk` because I used to be able to do this myself):

```
# Create new TPM matrix for filtered transcriptome
#Parse the input files and extract matching IDs
#Only retain lines if they are in the filtered FASTA file
awk '
NR==FNR {
    if (/^>/) {
        sub(/^>/, "")
        ids[$1] = 1
    }
    next
}
FNR==1 || ($1 in ids)
' ${OUTPUT_DIR}/filtered_supertranscript_output/filtered_transcripts_min_exp_1.fasta \
${OUTPUT_DIR}/supertranscript_output/salmon.isoform.TPM.not_cross_norm \
> ${OUTPUT_DIR}/filtered_supertranscript_output/filtered_transcripts_min_exp_1_salmon.TPM.not_cross_norm

# Calculate Ex50 statistics
contig_ExN50_statistic.pl ${OUTPUT_DIR}/filtered_supertranscript_output/filtered_transcripts_min_exp_1_salmon.TPM.not_cross_norm \
${OUTPUT_DIR}/filtered_supertranscript_output/filtered_transcripts_min_exp_1.fasta \
| tee ${OUTPUT_DIR}/filtered_supertranscript_output/filtered_min_exp_1_ExN50.stats
```

This modification worked! The E90N50 is 518 with 4542 transcripts contributing.

### 2026-10-15

Gretchen installed a new `EnTAP` module on `hydra` with all of the `EnTAP` dependencies. Now, I just need to run `module load entap` instead of loading `conda` environments and modules. She said I could run the script interactively without configuration which was confusing to me. I couldn't find any of the configuration output in an `entap_outfiles` directory, nor did the `/apps/EnTAP/2.3.0/databases/` folder have all of the databases I needed:

> (base) [yaamini.venkataraman@hydra-l2 databases]$ ls
entap_database.bin  uniprot_sprot.dmnd  uniref90.fasta  uniref90_rf.fasta

To compare, these are the databases I download for configuration in my script:

> (base) [yaamini.venkataraman@hydra-l2 06e-entap]$ ls *gz
nr.gz                   uniprot_sprot.fasta.gz   uniref90.fasta.gz
refseq_complete.faa.gz  uniprot_trembl.fasta.gz  uniref90_rf.fasta.gz

However, I noticed that there is a new `entap_config.ini` in the `/apps/EnTAP/2.3.0/bin` directory which includes all of the appropriate paths to scripts, programs, and dependencies, which suggests that it was configured after all. I asked for clarification to see if `EnTAP` had already been configured. Gretchen said that it was minimally configured. She also let me know that the `EnTAP` module does not include the `interproscan` dependency, since another user has asked for `interproscan`. My next steps are as follows:

1. Wait for confirmation that Gretchen has installed the `interproscan` module and load it in my script.
2. Reconfigure `EnTAP`. Based on the [`EnTAP documentation`](https://entap.readthedocs.io/en/latest/Getting_Started/Configuration/configuration.html), `EnTAP` can download and configure commonly-used databases for me. The only database I have in my original script that is not included in the common database list is [UniRef90](https://www.uniprot.org/uniref/). I will keep the code for downloading and modifying the UniRef90 database, but will download and configure all other databases through `EnTAP`.
3. Run `EnTAP` on the filtered transcriptome! To do this, I will need to cross reference [the execution flags](https://entap.readthedocs.io/en/latest/Running_EnTAP/execution_flags.html).

To prepare for step 2, I made the following modifications to the configuration script. I don't want to run it until I can also load `interproscan`, since the configuration will update the `interproscan` path in the configuration file:

```
# Make EnTap database
# Several databases can be downloaded and configured within EnTAP (UniProt Trembl, Uniprot Swiss-Prot, NCBI NR, NCBI Refseq Complete). However, the UnRef 90 database must be downloaded separately
# Download UniRef90
wget ftp://ftp.uniprot.org/pub/databases/uniprot/uniref/uniref90/uniref90.fasta.gz

# EnTAP parses taxonomic information from reference database headers for contaminant filtering and taxonomic weighting.
# The existing uniref header format is incompatible with EnTAP. Using Zac's script to reformat the headers so they can be parsed by EnTAP
gunzip -c uniref90.fasta.gz > uniref90.fasta
python ${HOME_DIR}/06e-reformat_uniref.py uniref90.fasta uniref90_rf.fasta
gzip uniref90_rf.fasta
rm uniref90.fasta
```

### 2026-10-22

Finally have time to think about this again! I need to update the `EnTAP` parameter file to include paths to the databases I want to use so I can perform the configuration:

```
#Note: if your databases do not have the typical NCBI or UniProt header format, taxonomic  information and filtering may not be utilized. Refer to the documentation to see how>
#type:list (string)
database= ncbi_nr, uniprot_trembl, refseq_complete, uniprot_sprot, /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/uniref90_rf.fasta.gz
```

I then updated the configuration code in my script:

```
echo "Start EnTAP configuration"

# Configure EnTAP to index the reference databases into diamond format
${ENTAP}/bin/EnTAP --config \
--run-ini ${OUTPUT_DIR}/entap_run.params \
--entap-ini ${ENTAP}/entap_config.ini \
-t 35

echo "EnTAP configuration complete"
```

I proceeded to run just the `EnTAP` configuration, since I already had the external database downloaded.

### 2026-10-23

My `EnTAP` configuration finished! However, there was something going on with the WHOI VPN and I couldn't access it...

In the meantime, I decided to look through the latest documentation for `EnTAP` to ensure my transcriptome annotation code was up to snuff. I have to update the following information in the parameterization file:

 - path to transcriptome
 - path to databases
 - list contaminant flags under `contam`
 - list taxon under the `taxon` flag
 - `ontology=0,1` to include EggNOG and InterProScan inforamtion in transcriptome annotation

Currently, this information is in my code chunk, but it seems like `EnTAP` is moving away from flags with commands and instead is relying on information provided in the configuration and parameterization files. I will need to save these files to my Github repository for reproducibility.

-d entap_outfiles/bin/nr.dmnd \
-d entap_outfiles/bin/refseq_complete.dmnd \
-d entap_outfiles/bin/uniprot_sprot.dmnd \
-d entap_outfiles/bin/uniprot_trembl.dmnd \
-d entap_outfiles/bin/uniref90_rf.dmnd \
-t 35 \
-c bacteria \
-c archaea \
-c viruses \
-c platyhelminthes \
-c nematoda \
-c fungi \
-c alveolata \
-c viridiplantae \
-c rhodophyta \
-c amoebozoa \
-c rhizaria \
-c stramenopiles \
-c rhizocephala \
-c entoniscidae \
--taxon brachyura

### 206-10-26

I can access Hydra again! I checked that the `EnTAP` configuration did indeed complete correctly:

> Start EnTAP configuration
Thu Oct 22 13:10:11 2026 -- Start EnTAP
        Parsing ini file at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_run.params
        Parsing ini file at: /apps/EnTAP/2.3.0/entap_config.ini
        ini files parsed, debug logging will continue at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/debug_2026Y10M22D-13h10m11s.txt
Thu Oct 22 13:10:11 2026 -- Running EnTAP Configuration...
Thu Oct 22 13:10:11 2026 -- Verifying EnTAP Databases...
        EnTAP Database Already Exists At: /apps/EnTAP/2.3.0/databases/entap_database.bin
Thu Oct 22 13:10:34 2026 -- EnTAP Database Verification Complete [0 min]
Thu Oct 22 13:10:34 2026 -- Preparing to index DIAMOND databases...
        Indexing DIAMOND database at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/temp/ncbi_nr.faa...
        Complete [42 min]
        Indexing DIAMOND database at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/temp/uniprot_trembl.faa...
        Complete [8 min]
        Indexing DIAMOND database at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/temp/refseq_complete.faa...
        Complete [28 min]
        Indexing DIAMOND database at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/temp/uniprot_sprot.faa...
        Complete [0 min]
        Indexing DIAMOND database at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/uniref90_rf.fasta.gz...
        Complete [19 min]
Thu Oct 22 21:48:32 2026 -- DIAMOND Database Indexing Complete [517 min]
Thu Oct 22 21:48:32 2026 -- Verifying EggNOG Databases...
        EggNOG SQL Database Already Exists At: /apps/EnTAP/2.3.0/eggnog-data/eggnog.db
        EggNOG DIAMOND Database Already Exists At: /apps/EnTAP/2.3.0/eggnog-data/eggnog_proteins.dmnd
Thu Oct 22 21:48:32 2026 -- EggNOG Database Verification Complete [0 min]
Thu Oct 22 21:48:36 2026 -- EnTAP Configuration Complete
 **** Be sure to update the EnTAP ini files with the new database paths ****
Thu Oct 22 21:48:36 2026 -- EnTAP Complete! [total runtime 518 minute(s)]
EnTAP configuration complete

Time to update the parameterization file.

- Path to transcriptome was already in the parameterization file: `/scratch/yaamini.venkataraman/wc-green-crab/output/06c-trinity/filtered_supertranscript_output/filtered_transcripts_min_exp_1.fasta`
- Listed taxon under the `taxon` flag: `taxon=brachyura`
- Listed contaminant flags under `contam`: `contam=bacteria, archaea, viruses, platyhelminthes, nematoda, fungi, alveolata, viridiplantae, rhodophyta, amoebozoa, rhizaria, stramenopiles, rhizocephala , entoniscidae`
- Listed multiple ontology source flags:

```
# Specify the ontology source databases you would like to use
#Note: it is possible to specify more than one! Just usemultiple --ontology_source flags
#Specify flags as follows:
#    0. EggNOG (default)
#    1. InterProScan
#type:list (integer)
ontology_source=0,1
```

I wasn't sure exactly how I needed to update the paths to the databases, so I checked the [`EnTAP` documentation](https://entap.readthedocs.io/en/latest/Running_EnTAP/Execution/execution_sim_search.html). I need to find the configured database paths with `.dmnd` extensions to use in the parameterization file:

> Similarity Searching is a mandatory stage of EnTAP. In order to run this, the user must input multiple DIAMOND configured databases (should have the ‘.dmnd’ extension) through the database command.

```
#Note: if your databases do not have the typical NCBI or UniProt header format, taxonomic  information and filtering may not be utilized. Refer to the documentation to see how to properly for>
#type:list (string)
database= /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/bin/ncbi_nr.dmnd, /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/bin/refseq_>
```

I then started the annotation:

```
echo "Start EnTAP annotation"

#Run EnTAP. The goal is to identify contaminated sequences to remove from the transcriptome.

${ENTAP}/bin/EnTAP --run \
--run-ini ${OUTPUT_DIR}/entap_run.params \
--entap-ini ${ENTAP}/entap_config.ini \
-t 35

echo "EnTAP annotation complete"
```

I immediately got an error:

> Start EnTAP annotation
Mon Oct 26 13:04:50 2026 -- Start EnTAP
        Parsing ini file at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_run.params
        Parsing ini file at: /apps/EnTAP/2.3.0/entap_config.ini
        ini files parsed, debug logging will continue at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/debug_2026Y10M26D-13h4m50s.txt
Error code: 10
InterProScan selected, but invalid databases input!
Mon Oct 26 13:05:42 2026 -- EnTAP has Failed! [total runtime 0 minute(s)]

Looks like the error comes from InterProScan! I needed to select a database for InterProScan to work on, which is separate from the `DIAMOND` databases. The `DIAMOND` databases were read in okay:

> Mon Oct 26 13:13:45 2026: User has input a database at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/bin/ncbi
_nr.dmnd
Mon Oct 26 13:13:45 2026: User has input a database at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/bin/refs
eq_complete.dmnd
Mon Oct 26 13:13:45 2026: User has input a database at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/bin/unip
rot_sprot.dmnd
Mon Oct 26 13:13:45 2026: User has input a database at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/bin/unip
rot_trembl.dmnd
Mon Oct 26 13:13:45 2026: User has input a database at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/bin/unir
ef90_rf.dmnd
Mon Oct 26 13:13:45 2026: Verifying EnTAP database...
Mon Oct 26 13:13:45 2026: Reading serialized database from: /apps/EnTAP/2.3.0/databases/entap_database.bin
 Of type: 2
Mon Oct 26 13:14:00 2026: Success!
Mon Oct 26 13:14:00 2026: Spawn Object - QueryData
Mon Oct 26 13:14:00 2026: Generating transcriptome mappings...
Mon Oct 26 13:14:00 2026: Transcriptome Lines - START
Mon Oct 26 13:14:00 2026: >TRINITY_DN0_c0_g1 top_iso:0 pct_iso_expr=NA pct_dom_iso_expr=NA max_expr_any=11.777358
Mon Oct 26 13:14:00 2026: ATTTGATTTTGCATAATTCAATCTTCCCTTCCTATGCTCTTCTTGCCTTGCCAATATTTC
Mon Oct 26 13:14:00 2026: Transcriptome Lines - END
Mon Oct 26 13:14:01 2026: Success!
Mon Oct 26 13:14:01 2026: Verifying input flag taxon
Mon Oct 26 13:14:01 2026: Verified species: brachyura
Mon Oct 26 13:14:01 2026: Verifying input flag contam
Mon Oct 26 13:14:01 2026: Verified species: bacteria
Mon Oct 26 13:14:01 2026: Verified species: archaea
Mon Oct 26 13:14:01 2026: Verified species: viruses
Mon Oct 26 13:14:01 2026: Verified species: platyhelminthes
Mon Oct 26 13:14:01 2026: Verified species: nematoda
Mon Oct 26 13:14:01 2026: Verified species: fungi
Mon Oct 26 13:14:01 2026: Verified species: alveolata
Mon Oct 26 13:14:01 2026: Verified species: viridiplantae
Mon Oct 26 13:14:01 2026: Verified species: rhodophyta
Mon Oct 26 13:14:01 2026: Verified species: amoebozoa
Mon Oct 26 13:14:01 2026: Verified species: rhizaria
Mon Oct 26 13:14:01 2026: Verified species: stramenopiles
Mon Oct 26 13:14:01 2026: Verified species: rhizocephala
Mon Oct 26 13:14:01 2026: Verified species: entoniscidae
Mon Oct 26 13:14:01 2026: Verifying input flag eggnog-contaminant
Mon Oct 26 13:14:01 2026: Verifying input flag fpkm
Mon Oct 26 13:14:01 2026: Verifying input flag qcoverage
Mon Oct 26 13:14:01 2026: Verifying input flag tcoverage
Mon Oct 26 13:14:01 2026: Verifying input flag ontology_source
Mon Oct 26 13:14:01 2026: Verifying input flag level
Mon Oct 26 13:14:01 2026: Killing Object - EntapDatabase
Mon Oct 26 13:14:04 2026: Killing Object - QueryData
Mon Oct 26 13:14:04 2026: QuerySequence data freed
Mon Oct 26 13:14:04 2026: Error code: 10
InterProScan selected, but invalid databases input!

I reviewed Zac's paper and confirmed that he actually didn't use InterProScan! So I don't have to worry about this after all. I changed the ontology source back to EggNOG only in the parameterization file:

```
#type:list (integer)
ontology_source=0
```

Of course, I was faced with a new error:

> Mon Oct 26 13:47:48 2026: Verifying EggNOG-mapper inputs...
Mon Oct 26 13:47:48 2026: Executing command:
/apps/EnTAP/2.3.0/eggnog-mapper-2.1.12/emapper.py --version
Mon Oct 26 13:47:48 2026:
Std Err:
/apps/EnTAP/2.3.0/eggnog-mapper-2.1.12/eggnogmapper/utils.py:61: SyntaxWarning: "\[" is an invalid escape sequence. Such sequences will not work in the future. Did you mean "\\["? A raw string is also an option.
  return re.sub("\\033\[[^m]+m", "", string)
Traceback (most recent call last):
  File "/apps/EnTAP/2.3.0/eggnog-mapper-2.1.12/emapper.py", line 16, in <module>
    from eggnogmapper.emapper import Emapper
  File "/apps/EnTAP/2.3.0/eggnog-mapper-2.1.12/eggnogmapper/emapper.py", line 4, in <module>
    import errno, os, shutil, psutil
ModuleNotFoundError: No module named 'psutil'
Mon Oct 26 13:47:48 2026: Killing Object - EntapDatabase
Mon Oct 26 13:47:52 2026: Killing Object - QueryData
Mon Oct 26 13:47:52 2026: QuerySequence data freed
Mon Oct 26 13:47:52 2026: Error code: 10
Test of EggNOG mapper failed, ensure python is properly installed and the paths are correct

I ran into a `python` issue previously where I had to unset system-level `python` variables in order to get `EnTAP` to work. I tried that again:

```
#Unset python variables again to avoid conflicts with EnTAP run
unset PYTHONPATH
unset PYTHONHOME

echo "Start EnTAP annotation"
```

Unsetting the python variables didn't work. I think there's an issue with the `EnTAP` installation itself, based on the error messages about EggNOG mapper. Since I didn't install the module, I put a ticket in with IS.

### Going forward

1. Annotate transcriptome with `EnTAP`
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
