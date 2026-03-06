---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 87
tags: green-crab-wc RNA-Seq EnTAP
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
