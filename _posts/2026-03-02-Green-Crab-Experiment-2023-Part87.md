---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 87
tags: green-crab-wc RNA-Seq EnTAP trinity salmon
---

## Transcriptome annotation, continued

### 2026-03-02

My script errored out:

> (base) [yaamini.venkataraman@poseidon-l2 06e-entap]$ tail yrv_blast1875379.log
WARNING: $PATH does not agree with $PATH_modshare counter. The following directories' usage counters were adjusted to match. Note that this may mean that module unloading may not work correctly.
 /vortexfs1/apps/mambaforge/bin /cm/local/apps/environment-modules/4.0.0//bin
Fri Feb 27 17:45:27 2026 -- Start EnTAP
EnTAP config ini not found, generated at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_config.ini
EnTAP run parameter ini not found, generated at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_run.params
Error code: 14
Configuration file was not found and was generated for you, make sure to check the paths before continuing.
Both INI files are required for EnTAP execution, missing file(s) generated at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap
Fri Feb 27 17:45:27 2026 -- EnTAP has Failed! [total runtime 0 minute(s)]

I see the `EnTAP` config and run parameter files in my directory, but maybe there was an issue with `EnTAP` not knowing where they were? I specified the files in the code:

When I reran my script, I still got an error!

>  /vortexfs1/apps/mambaforge/bin /cm/local/apps/environment-modules/4.0.0//bin
Mon Mar  2 16:22:08 2026 -- Start EnTAP
        Parsing ini file at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_run.params
        Parsing ini file at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_config.ini
        ini files parsed, debug logging will continue at: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/debug_2026Y3M2D-16h22m8s.txt
Error code: 10
Databases have been selected for indexing. The test run of DIAMOND has failed!
Mon Mar  2 16:22:08 2026 -- EnTAP has Failed! [total runtime 0 minute(s)]

The log states that there is a separate debug log, so I investigated that to see why the `DIAMOND` test failed:

> Mon Mar  2 16:22:08 2026: Executing command:
/vortexfs1/home/yaamini.venkataraman/EnTAP-v2.3.0/libs/diamond-v2.1.8/bin/diamond --version
Mon Mar  2 16:22:08 2026:
Std Err:
sh: /vortexfs1/home/yaamini.venkataraman/EnTAP-v2.3.0/libs/diamond-v2.1.8/bin/diamond: No such file or directory
Mon Mar  2 16:22:08 2026: Error code: 10
Databases have been selected for indexing. The test run of DIAMOND has failed!
Mon Mar  2 16:22:08 2026: End - EnTAP


Ah, it seems like the issue is that `EnTAP` is looking within the `EnTAP` directory for its dependencies, but that is certainly not where they are found. When I checked in that directory, I did see that there are files that can be unzipped and installed. Since I already have some dependencies installed and loaded in modules, I thought I would start by seeing if I could modify [the config file](https://entap.readthedocs.io/en/latest/Getting_Started/Configuration/configuration.html). Turns out there were too many places to adjust in the config file, so I decided to just [install the dependencies](https://entap.readthedocs.io/en/latest/Getting_Started/Installation/Installation_From_Source_Code/installation_from_source_code.html#installing-pipeline-software) from the source code. I installed `diamond`, `eggnog-mapper`, and `RSEM`. I think the `RSEM` installation went okay? If not, I have the module I can rely on.

Since `interproscan` is not bundled with EnTAP, I had to load the module. To find the path to the program, I ran `module show interproscan`. I now know that all of the `bio` module programs these programs are housed within `/vortexfs1/apps/bio/`. I went in the config file and changed the path to `interproscan.sh`:

```
interproscan-exe=/vortexfs1/apps/bio/interproscan-5.51-85.0/interproscan.sh
```

Now that I had all of that sorted, I tried running `EnTAP` again. It failed with the same error! I copied and pasted the same code it was running into my Terminal to reproduce the error:

> (base) [yaamini.venkataraman@poseidon-l2 ~]$ EnTAP-v2.3.0/libs/diamond-v2.1.8/bin/diamond --version
-bash: EnTAP-v2.3.0/libs/diamond-v2.1.8/bin/diamond: No such file or directory

I could, however, get it to print out the `diamond` version when removing the path to my home directory:

> (base) [yaamini.venkataraman@poseidon-l2 ~]$ EnTAP-v2.3.0/libs/diamond-2.1.8/bin/diamond --version
diamond version 2.1.8

I even navigated to the script's output directory (where the script was running from) and ran the following:

> (base) [yaamini.venkataraman@poseidon-l2 06e-entap]$ /vortexfs1/home/yaamini.venkataraman/EnTAP-v2.3.0/libs/diamond-2.1.8/bin/diamond --version

And just to confirm, I activated the `EnTAP` environment and ran the same code:

> (EnTAP) [yaamini.venkataraman@poseidon-l2 06e-entap]$ /vortexfs1/home/yaamini.venkataraman/EnTAP-v2.3.0/libs/diamond-2.1.8/bin/diamond --version
diamond version 2.1.8

So clearly, there's an issue with `EnTAP` calling `diamond`. When I copied and pasted the line from the error log, I kept getting errors where it couldn't find the file, so I deleted one piece of it at a time to figure out what could be causing the error. I then copied and pasted a "clean" version into the config file. I truly do not understand what happened but simply doing that allowed the indexing to start. :lolsob:

## 2026-03-03

`EnTAP` finished configuring!

>  **** Be sure to update the EnTAP ini files with the new database paths ****
Mon Mar  2 20:52:35 2026 -- EnTAP Complete! [total runtime 192 minute(s)]

It seems like there are a couple of new database paths I need to update in the config files.

```
Downloading EnTAP Database To: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/bin/entap_database.bin

Downloading EggNOG SQL Database to: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/databases/eggnog.db

Downloading EggNOG DIAMOND Database to: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/bin/eggnog_proteins.dmnd
```

Here are the old paths, just for reference:

```
entap-db-bin=/vortexfs1/home/yaamini.venkataraman/EnTAP-v2.3.0/bin/entap_database.bin

#Path to directory containing the EggNOG SQL database(s).
#type:string
eggnog-map-data=/databases

#Path to EggNOG DIAMOND configured database that was generated during the Configuration stage
#type:string
eggnog-map-dmnd=/vortexfs1/home/yaamini.venkataraman/EnTAP-v2.3.0/bin/eggnog_proteins.dmnd
```

I then added the chunk to actually run `EnTAP`:

```
#Run EnTAP. The goal is to identify sequences for another round of cleaning.

${ENTAP}/EnTAP --runN -i ${BLAST_DIR}/transcriptome_contamRemoved.fasta \
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
```

I got the following error:

> Tue Mar  3 14:16:07 2026 -- Start EnTAP
PARSE ERROR: Argument: --runN
             Couldn't find match for argument

I decided to check the `EnTAP` documentation to see if there was an alternative I should use. I found my answer in the [change log](https://entap.readthedocs.io/en/latest/Development/changelog.html#entap-v2-1-0-january-11-2025):

> Removed ‘runP’ and ‘runN’ flags, replacing them with ‘run’ and ‘frame_selection’ for clarity. ‘run’ will always be used to execute the main EnTAP annotation pipeline (as opposed to ‘config’), while ‘frame_selection’ can be set to true/false if you would like to perform frame selection on nucleotide input sequences

Seems like I need to add the `run` and `frame_selection` flags. I add `run` to the code:

```
${ENTAP}/EnTAP --run \
-i ${BLAST_DIR}/transcriptome_contamRemoved.fasta \
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
```

And according to [the example](https://entap.readthedocs.io/en/latest/Running_EnTAP/execution_index.html#sample-run), I need to add `frame_selection` to the parameter file. I checked and this was already selected!

```
#Specify if you would like to perform frame selection/gene prediction on your input nucleotide transcriptome. This flag will be ignored i$
#type:boolean (true/false)
frame-selection=true
```

There were also places for me to add transcriptome and database information into the parameter file itself. I figured I would see what happened if I didn't do that first. Obviously, I got an error, but surprisingly it was not one I expected:

> Test of EggNOG mapper failed, ensure python is properly installed and the paths are correct
Tue Mar  3 14:39:48 2026 -- EnTAP has Failed! [total runtime 0 minute(s)]

```
(base) [yaamini.venkataraman@poseidon-l2 ~]$ less /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/debug_2026Y3M3D-14h39m1s.txt

  PYTHONPATH = '/vortexfs1/apps/python-3.6.5'
  program name = '/vortexfs1/home/yaamini.venkataraman/.conda/envs/EnTAP/bin/python'
  isolated = 0
  environment = 1
  user site = 1
  import site = 1
  sys._base_executable = '/vortexfs1/home/yaamini.venkataraman/.conda/envs/EnTAP/bin/python'
  sys.base_prefix = '/vortexfs1/apps/python-3.6.5'
  sys.base_exec_prefix = '/vortexfs1/apps/python-3.6.5'
  sys.platlibdir = 'lib'
  sys.executable = '/vortexfs1/home/yaamini.venkataraman/.conda/envs/EnTAP/bin/python'
  sys.prefix = '/vortexfs1/apps/python-3.6.5'
  sys.exec_prefix = '/vortexfs1/apps/python-3.6.5'
  sys.path = [
    '/vortexfs1/apps/python-3.6.5',
    '/vortexfs1/apps/python-3.6.5/lib/python39.zip',
    '/vortexfs1/apps/python-3.6.5/lib/python3.9',
    '/vortexfs1/apps/python-3.6.5/lib/python3.9/lib-dynload',
  ]
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'

Current thread 0x00002aaaaaaecd80 (most recent call first):
<no Python frame>

Tue Mar  3 14:39:37 2026: Killing Object - EntapDatabase
Tue Mar  3 14:39:45 2026: Killing Object - QueryData
Tue Mar  3 14:39:47 2026: QuerySequence data freed
Tue Mar  3 14:39:48 2026: Error code: 10

Test of EggNOG mapper failed, ensure python is properly installed and the paths are correct
```

Ah, my old nemesis. Freaking `python` version conflicts. I'm surprised this issue is taking place even though I unset all the python variables at the top of my script. I tried unsetting them again right before running `EnTAP` to see if that would help.

Unsetting the variables once again helped! I ran into a separate `transdecoder` issue:

> Tue Mar  3 15:11:29 2026 -- Beginning Frame Selection...
        Running TransDecoder Frame Selection...
---------------------------------------------------------
Here are a few ways to help diagnose some general issues:
        1. Check the detailed printed error message below this list
        2. Review the .err files of the execution stage (they will be in the directory for whatever stage failed)
        3. Check the debug file that is printed after execution
        4. Ensure your paths/inputs are correct in entap_config.ini or entap_run.params and log_file.txt (this will show your inputs)
---------------------------------------------------------
Error code: 105
Error in running TransDecoder.LongOrfs
TransDecoder Error: Error, don't understand options: -O /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/frame_selection/TransDecoder -t /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/transcriptomes/transcriptome_contamRemoved.fasta at /vortexfs1/apps/bio/transdecoder-5.3.0/TransDecoder.LongOrfs line 117.

Looking at the more detailed run log:

> Tue Mar  3 15:11:29 2026: Training TransDecoder data...
Tue Mar  3 15:11:29 2026: Executing command:
TransDecoder.LongOrfs -m 100 -O /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/frame_selection/TransDecoder -t /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/transcriptomes/transcriptome_contamRemoved.fasta
Tue Mar  3 15:11:29 2026: WARNING: Suppressing error log from child process, check error file generated for full output
Tue Mar  3 15:11:29 2026:
Printing to files:
Std Out: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/frame_selection/TransDecoder/long_orfs_std.out
Std Err: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/frame_selection/TransDecoder/long_orfs_std.err
Tue Mar  3 15:11:29 2026: Killing object - ModTransdecoder
Tue Mar  3 15:11:29 2026: Killing Object - QueryData
Tue Mar  3 15:11:33 2026: QuerySequence data freed
Tue Mar  3 15:11:33 2026: Killing Object - GraphingManager
Tue Mar  3 15:11:33 2026: Killing Object - EntapDatabase
Tue Mar  3 15:11:40 2026: Error code: 105
Error in running TransDecoder.LongOrfs
TransDecoder Error: Error, don't understand options: -O /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/frame_selection/TransDecoder -t /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/transcriptomes/transcriptome_contamRemoved.fasta at /vortexfs1/apps/bio/transdecoder-5.3.0/TransDecoder.LongOrfs line 117.

Alright, seems like it's looking for my transcriptome in the `EnTAP` folder, but the transcriptome is in the `blast` folder. Perhaps I need to modify that in the input parameters after all.

## 2026-03-05

I modified the input parameter and changed the command:

```
#Path to the input transcriptome file
#type:string
input=/scratch/yaamini.venkataraman/wc-green-crab/output/06d-blast/transcriptome_contamRemoved.fasta
```

```
${ENTAP}/EnTAP --run \
--run-ini ${OUTPUT_DIR}/entap_run.params \
--entap-ini ${OUTPUT_DIR}/entap_config.ini \
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
```

I still got the same error!

> Error in running TransDecoder.LongOrfs
TransDecoder Error: Error, don't understand options: -O /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/frame_selection/TransDecoder -t /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/transcriptomes/transcriptome_contamRemoved.fasta at /vortexfs1/apps/bio/transdecoder-5.3.0/TransDecoder.LongOrfs line 117.

I looked into whether each of these files actually existed at the indicated path. Yes! They were all found there. So it seems like at some point during the run, my transcriptome file got moved to the output file directory for `EnTAP` runs. I decided to dig into the "don't understand options" `transdecoder` error more. The first thing I wondered was if it was a `transdecoder` version issue. I am using version 5.3.0 from Poseidon, which is the minimum allowable version for `EnTAP`. However, version 5.7.1 was packaged with `EnTAP` and I did not install that dependency manually. When I looked into the latest version of `transdecoder`, I saw that [the `-O` flag was only introduced with  version 5.7.1](https://github.com/TransDecoder/TransDecoder/releases)!! So it really is a version issue.

I needed to figure out how to install `transdecoder`, since the [`EnTAP` dependency page](https://entap.readthedocs.io/en/latest/Getting_Started/Installation/Installation_From_Source_Code/installation_from_source_code.html#installing-pipeline-software) doesn't have installation instructions. Based on the [`transdecoder` documentation](https://github.com/TransDecoder/TransDecoder/wiki), there's nothing to compile. I confirmed this by unzipping the directory (`tar -xvzf`) and looking at the `Makefile`:

> (base) [yaamini.venkataraman@poseidon-l2 TransDecoder-TransDecoder-v5.7.1]$ cat Makefile
SHELL := /bin/bash
all:
	@echo Nothing to build.  Run \'make test\' to run example executions on each of the sample data sets provided.
clean:
	cd ./sample_data && make clean
test:
	cd ./sample_data/ && make test
(base) [yaamini.venkataraman@poseidon-l2 TransDecoder-TransDecoder-v5.7.1]$ make
Nothing to build. Run 'make test' to run example executions on each of the sample data sets provided.

My next steps were as follows:

- Comment out the line loading the `transdecoder` module
- Make sure the `transdecoder` path is correct in the config and run parameter files

In the `config` file, I found the following chunk:

```
#Method to execute TransDecoder.LongOrfs. This may be the path to the executable or simply TransDecoder.LongOrfs
#type:string
transdecoder-long-exe=TransDecoder.LongOrfs
#Method to execute TransDecoder.Predict. This may be the path to the executable or simply TransDecoder.Predict
#type:string
transdecoder-predict-exe=TransDecoder.Predict
```

I changed it to this:

```
#Method to execute TransDecoder.LongOrfs. This may be the path to the executable or simply TransDecoder.LongOrfs
#type:string
transdecoder-long-exe=/vortexfs1/home/yaamini.venkataraman/EnTAP-v2.3.0/libs/TransDecoder-TransDecoder-v5.7.1/TransDecoder.LongOrfs
#Method to execute TransDecoder.Predict. This may be the path to the executable or simply TransDecoder.Predict
#type:string
transdecoder-predict-exe=/vortexfs1/home/yaamini.venkataraman/EnTAP-v2.3.0/libs/TransDecoder-TransDecoder-v5.7.1/TransDecoder.Predict
```

There were no flags to modify in the run parameter file, but again, it seems like all the options I'm providing for taxonomy and contaminants can also be indicated in the run parameter file. I started the revised script.

### 2026-03-06

When I checked on my script today it was still running! It finished frame selection with `transdecoder` and moved onto similarity search with `diamond`. The job is ID 1876450.

### 2026-03-10

#### Gene-level count matrix

Ashray is starting the preliminary `DESeq2` analysis. One issue he is running into is that `DESeq2` is performing the analysis on all of the transcripts and isoforms, but for this analysis working with gene-level data would be more appropriate. There is an R package called [`tximport`](https://bioconductor.org/packages/release/bioc/vignettes/tximport/inst/doc/tximport.html#Import_transcript-level_estimates) that is built for transcript-level count matrix data, but a brief read of the manual was pretty confusing! I decided to go back to the `salmon` script and modify it to generate a gene-level count matrix:

```
#Gene-level estimates
## Get a list of the salmon quant.sf files so we don't have to list them individually
find ${OUTPUT_DIR}/. -maxdepth 2 -name "quant.sf.genes" | tee ${OUTPUT_DIR}/salmon.quant_genes_files.txt

## Generate a matrix with all abundance estimates
$TRINITY_HOME/util/abundance_estimates_to_matrix.pl \
--est_method salmon \
--gene_trans_map none \
--quant_files ${OUTPUT_DIR}/salmon.quant_genes_files.txt \
--name_sample_by_basedir

## Calculate Ex50 statistics
contig_ExN50_statistic.pl ${OUTPUT_DIR}/salmon.isoform.TPM.not_cross_norm \
${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
| tee ${OUTPUT_DIR}/ExN50.stats.genes

## Calculate N50 statistics
TrinityStats.pl ${OUTPUT_DIR}/trinity_out_dir/Trinity.fasta \
> ${OUTPUT_DIR}/N50.txt.genes
```

I can run this in addition to the transcript-level estimates so I have both pieces of information prior to working with `DESeq2`. The job is pending.

#### Adding large files to OSF

Another issue I ran into when Ashray was doing the data analysis is file sharing! The `salmon` output is too large to host on Github, and I completely forgot about OSF...until now. I created [this OSF repository](https://osf.io/ea8mq/overview) to host the large files. I also added the link to the OSF repo to [the Github repository](https://github.com/yaaminiv/wc-green-crab/tree/main).

#### `EnTAP` progress

My `EnTAP` run didn't finish in the 5 day window! Looks like the `diamond` database search takes a really long time. I now needed to do two things:

1. Figure out to resume an `EnTAP` run from where it left off
2. Change the run time of the `EnTAP` script

The second one was easy enough to do. I modified the following in the run parameter file:

```
#Select this option if you would like EnTAP to continue execution if existing files are found from a previous run. If false$
#type:boolean (true/false)
resume=true
```

I restarted my run! Surprisingly it started running immediately. I realized I was using the compute note for this but not the `trinity` script. When I switched that to the compute node, it didn't start running immediately! No clue what's going on here.

I did confirm that my script started the `diamond` similarity search instead of re-running `transdecoder`:

```
Tue Mar 10 17:27:13 2026 -- Running EnTAP Execution...
Tue Mar 10 17:28:06 2026 -- Beginning Frame Selection...
        Parsing TransDecoder Frame Selection Results...
        Complete [0 min]
        Results written to: /scratch/yaamini.venkataraman/wc-green-crab/output/06e-entap/entap_outfiles/frame_selection/TransDecoder
Tue Mar 10 17:28:12 2026 -- Frame Selection Complete [0 min]
Tue Mar 10 17:28:13 2026 -- Beginning Similarity Search...
        Running DIAMOND Similarity Search...
                Running DIAMOND Similarity Search against database at entap_outfiles/bin/nr.dmnd...
```

### 2026-03-12

#### Gene-level count matrix

I have a gene-level count matrix! Unfortunately, it overwrote all the files that were transcript-level and previously created from `trinity`. Not an issue since I have those files saved on my computer and OSF, but annoying if I wanted to work from the scratch directory. This should be a bit more straightforward to work with, but I still have 648,341 genes in the file. When I checked the isoform count matrix, I had 913681 entries. This could just be the result of having to create a de novo transcriptome. I may need to mess around with `tximport` after all just to see if there's any other redundancy built in. Either way, I renamed the count matrix and moved it to my home directory, Github repository, and OSF repository.

#### `EnTAP` progress

Still running! Job ID is 1880693.

### 2026-03-13

#### Gene-level count matrix

I had an epiphany. I probably have a lot of contamination sequences since I've been generating information for the uncleaned transcriptome! While `EnTAP` is running (and will likely identify more contamination), I can at least work with the transcriptome that was [cleaned after my `blastn` run](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part85/). I had a couple of tasks for today:

1. Find the annotated transcriptome generated from `blastn`

I navigated to the `blastn` output directory to look at the files that were created. I have a file called `blast-results/transcriptome-contam.tab` with 115,204 entries:

> (base) [yaamini.venkataraman@poseidon-l1 blast-results]$ head transcriptome-contam.tab
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

I also have a list of 55,624 contaminated sequences to remove:

> (base) [yaamini.venkataraman@poseidon-l1 blast-results]$ head ../contam_list.txt
>TRINITY_DN11_c0_g1_i15
>TRINITY_DN11_c0_g1_i2
>TRINITY_DN11_c0_g1_i21
>TRINITY_DN11_c0_g1_i24
>TRINITY_DN11_c0_g1_i25
>TRINITY_DN11_c0_g1_i26
>TRINITY_DN11_c0_g1_i28
>TRINITY_DN11_c0_g1_i3
>TRINITY_DN11_c0_g1_i31
>TRINITY_DN11_c0_g1_i35

In theory, there are annotations present in `transcriptome-contam.tab` for clean sequences found in `transcriptome_contamRemoved.fasta`. To test this, I decided to use `grep` to look at HSPs:

> (base) [yaamini.venkataraman@poseidon-l1 06d-blast]$ grep "HSP" blast-results/transcriptome-contam.tab
TRINITY_DN197_c1_g2_i10	gi|558549575|gb|KC493625.1|	83.781	2084	322	10	103	2178	179	2254	0.0	1962	Eriocheir sinensis heat shock protein 70 (HSP70) mRNA, complete cds	95602
TRINITY_DN1236_c17_g1_i1	gi|982224718|ref|XM_005595941.2|	88.889	99	8	3	21	116	596	694	6.73e-23	119	PREDICTED: Macaca fascicularis heat shock protein family A (Hsp70) member 6 (HSPA6), mRNA	9541
TRINITY_DN12542_c0_g1_i1	gi|723587483|gb|KM277361.1|	95.349	86	4	0	334	419	103	188	2.46e-28	137	Charybdis japonica heat shock protein 70 (HSP70) mRNA, complete cds	80839
TRINITY_DN12542_c0_g1_i6	gi|723587483|gb|KM277361.1|	95.349	86	4	0	339	424	103	188	2.49e-28	137	Charybdis japonica heat shock protein 70 (HSP70) mRNA, complete cds	80839

Many of those entries above and the others I looked through have annotations in crustacean and/or marine invertebrate species, so they would not have been removed during the cleaning step! So I definitely have a bare-bones annotation file to work with. The cleaned transcriptome from `blast` has 9,355,650, so I am missing a lot of annotations. I think that's fine for now, considering that this is a preliminary analysis.

2. Get gene-level count matrix information for the transcriptome cleaned after running `blastn`

Alright, now that I know I have annotations to work with and I have a cleaner (but not cleanest!) transcriptome, it's time to get gene-level count matrix information for that cleaner transcriptome. I figured a good place to start would be to remove the contamination sequences in `contam_list.txt` from the gene-level count matrix I had. I transferred the files to my home directory and wanted to transfer them to my laptop, but for some reason I couldn't log into `vast`?! I've ran into this problem before, and I don't know why sometimes I can log in to the server and sometimes I cannot. Unclear what's happening but I just used `cat` to get the file information and created [`contam-list.txt`](https://github.com/yaaminiv/wc-green-crab/blob/main/output/06d-blast/contam-list.txt) and [`transcriptome-contam.tab`](https://github.com/yaaminiv/wc-green-crab/blob/main/output/06d-blast/blast-results/transcriptome-contam.tab).

I figured the sequence removal step should be straightforward to do in R. I opened the [R Markdown file Ashray created to start the preliminary analysis](https://github.com/yaaminiv/wc-green-crab/blob/main/code/06f-gene-expression.Rmd) and imported the gene-level count matrix information and metadata. I then imported the list of contaminated sequences, removing the ">" prefix and isoform indications at the end to match the syntax in the gene-level count matrix. I successfully figured out the `regex` syntax thanks to the help of [this website](https://regexr.com):

```
contamSeqBlast <- read.delim("../output/06d-blast/contam-list.txt", header = FALSE, check.names = FALSE, col.names = "ID") %>%
  mutate(ID = gsub(pattern = ">", replacement = "", x = ID)) %>%
  mutate(ID = gsub(pattern = "_i.", replacement = "", x = ID)) %>%
  arrange(ID) %>%
  distinct(.)
  #Import list of contaminated sequences. Use gsub to get rid of ">". Use gsub to also get rid of isoform designation (_i = start of isoform, . = any character except new line) so entries can be mapped to gene-level count matrix. Arrange alphanumerically and retain distinct rows.
head(contamSeqBlast) #Confirm changes
nrow(contamSeqBlast) #51785 contaminated genes
```

I then used `anti_join` to remove contamination sequences:

```
# making new data subset for end of exp THIS IS WHERE ROW NAMES GETS ADDED(?)
End.data <- salmon.data %>%
  select(ID, any_of(sequencing.picks.8.9.23$crab.ID)) %>%
  mutate(across(where(is.numeric), round, 0)) %>%
  anti_join(., y = contamSeqBlast, by = "ID") %>% #Use anti-join to return all rows from salmon.data that do not match list of contamination sequences
  arrange(ID)
head(End.data)
nrow(End.data) #597263 genes without contamination
```

Now that I had this figured out, I created [this Github issue](https://github.com/yaaminiv/wc-green-crab/issues/5) with the code so Ashray could do the same.

3. Annotate cleaner gene-level count matrix

The last task I had was annotating the gene-level count matrix. I imported and modified the annotations in R:

```
blastAnnotations <- read.delim("../output/06d-blast/blast-results/transcriptome-contam.tab", header = FALSE, check.names = FALSE, ) %>%
  select(1, 13) #Import annotations from blast. Keep only columns 1 and 13
colnames(blastAnnotations) <- c("ID", "annotation") #Rename columns
```
```{r}
blastAnnotationsGenes <- blastAnnotations %>%
    mutate(ID = gsub(pattern = "_i.", replacement = "", x = ID)) %>%
  distinct(.) #Remove isoform indication from annotations to match gene-level count matrix and keep only distinct rows.
head(blastAnnotationsGenes)
nrow(blastAnnotationsGenes)
```

I then used `left_join` to match the annotations with the differentially expressed transcripts:

```
lfc2.results.annot <- left_join(lfc2.results, blastAnnotationsGenes, by = "ID") %>%
  distinct(.)
lfc2.results.annot %>%
  filter(is.na(annotation) == FALSE) %>%
  nrow(.) #897 transcripts with annotations
```

Only 897 differentially expressed transcripts were annotated. I suspect that I will have more annotations once `EnTAP` finishes. I posted [this Github issue](https://github.com/yaaminiv/wc-green-crab/issues/6) for Ashray to annotate the DET list himself.

### Going forward

1. Annotate transcriptome with `EnTAP`
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
