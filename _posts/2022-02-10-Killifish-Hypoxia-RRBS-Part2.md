---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 2
tags: killifish RRBS poseidon
---

## Trimming RRBS data

Before I ran any alignments, I wanted to trim adapter sequences and check post-trimming quality with `multiqc`. Neel couln't remember if the sequences were non-directional or directional, and from prior experience he said that he manually checked potential directionality in the alignment step. My plan is to get fully trimmed files using both methods, then troubleshoot alignment with a subset from each! All output from trimming can be found [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/02-trimgalore).

### Non-directional Trimming

I ran [this script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/02-trimgalore.sh) for `trimgalore`, and made sure I included the `-rrbs` argument necessary for hard-trimming base pairs off of the 3' end. Everything was going great...until the penultimate sample. I got the following error for `190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz`:

```
Using Illumina adapter for trimming (count: 10552). Second best hit was smallRNA (count: 0)

Writing report to '/vortexfs1/scratch/yaamini.venkataraman/02-trimgalore/190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz_trimming_report.txt'

SUMMARISING RUN PARAMETERS
==========================
Input filename: /vortexfs1/home/naluru/Killifish/WHOI-Mummichog_RRBS/190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz
Trimming mode: paired-end
Trim Galore version: 0.6.6
Cutadapt version: 3.5
Number of cores used for trimming: 1
Quality Phred score cutoff: 20
Quality encoding type selected: ASCII+33
Adapter sequence: 'AGATCGGAAGAGC' (Illumina TruSeq, Sanger iPCR; auto-detected)
Maximum trimming error rate: 0.1 (default)
Minimum required adapter overlap (stringency): 1 bp
Minimum required sequence length for both reads before a sequence pair gets removed: 20 bp
File was specified to be an MspI-digested RRBS sample. Read 1 sequences with adapter contamination will be trimmed a further 2 bp from their 3' end, and Read 2 sequences will be trimmed by 2 bp from their 5' end to remove potential methylation-biased bases from the end-repair reaction
File was specified to be a non-directional MspI-digested RRBS sample. Sequences starting with either 'CAA' or 'CGA' will have the first 2 bp trimmed off to remove potential methylation-biased bases from the end-repair reaction
Running FastQC on the data once trimming has completed
Running FastQC with the following extra arguments: '--outdir /vortexfs1/scratch/yaamini.venkataraman/02-trimgalore --threads 28'
Output file(s) will be GZIP compressed

Cutadapt seems to be fairly up-to-date (version 3.5). Setting -j 1
  >>> Now performing adaptive quality trimming with a Phred-score cutoff of: 20 <<<

This is cutadapt 3.5 with Python 3.9.5
Command line parameters: -j 1 -e 0.1 -q 20 -a X /vortexfs1/home/naluru/Killifish/WHOI-Mummichog_RRBS/190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz
Processing reads on 1 core in single-end mode ...
10000000 sequences processed
20000000 sequences processed
cutadapt: error: Error in FASTQ file at line 91357035: Line expected to start with '+', but found '@'

  >>> Quality trimming completed <<<
22839258 sequences processed in total

Unable to close QUAL filehandle:
```

I removed this sample and ran the rest of my script successfully, including `multiqc`. I decided to figure out what was happening with the last sample! I extracted the sequence with the error, as well as upstream and downstream sequences:

```
zcat 190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz | sed -n '91357028,91357032p;91357041q'

AAFFFJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJJ
@K00114:958:H7TVNBBXY:4:2218:9435:44535 1:N:0:TATAATAT_AGATCTCG
TGGTAAGTTGAGAGTGATAGTTTTTTTTGGTTTGGTTTGTTTTTTGTTT
+
AAFFFJJJJJJJJJFJJJJJJJJJJJJJJJJJJJFJFJJJFJJJJJJJJ
@K00114:958:H7TVNBBXY:4:2218:10044:44535 1:N:0:TATAATAT_AGACCTCA
T:2218:7FJFAFJJJFAJJJJJJFJFFJFFFJJJJJFJJJJJJA<
@K00114:958:H7TVNBBXY:4:2218:32684:44517 1:N:0:TATAATAA_AAACCCCG # Line with issue
GGAGGGTGTCAATCCTGACGGTTATTTCCTAGCCAACTTAGAGCCAATA
+
<AA-FF-A-AAFJJFA7AF7AFFJ-77FAFJ--A<--<77<7-7---7F
@K00114:958:H7TVNBBXY:4:2218:1641:44535 1:N:0:NATAATAT_NGATCTCG
NGAAAAAAATGGTATAAATTGGAATGATGTTTTTTCGTTATTATATTTA
```

It looks like the second sequence, `@K00114:958:H7TVNBBXY:4:2218:10044:44535 1:N:0:TATAATAT_AGACCTCA...T:2218:7FJFAFJJJFAJJJJJJFJFFJFFFJJJJJFJJJJJJA<` may not have the correct balance of sequence and quality information that the other sequences do. I'm not exactly sure how to fix it, so I sent Neel an email.

### Directional trimming

While waiting for Neel's response, I decided to get a start on directional trimming. Since it takes a while, I figured that I may have a solution about the file with the error before the script starts trimming that file. I created a [new script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/02-trimgalore-directional.sh), and was sure to remove the `--non_directional` argument.

As I predicted, I ran into the same issue with sample 190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz!

```
This is cutadapt 3.5 with Python 3.9.5
Command line parameters: -j 1 -e 0.1 -q 20 -a X /vortexfs1/home/naluru/Killifish/WHOI-Mummichog_RRBS/190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz
Processing reads on 1 core in single-end mode ...
10000000 sequences processed
20000000 sequences processed
cutadapt: error: Error in FASTQ file at line 91357035: Line expected to start with '+', but found '@'

  >>> Quality trimming completed <<<
22839258 sequences processed in total

Unable to close QUAL filehandle:
```

Neel wasn't sure what's going on with the sequences either, so I just ran the rest of the script without that file and with `multiqc`...and it kept failing because apparently sample 190626_I114_FCH7TVNBBXY_L4_OC-S5_1.fq.gz was empty:

```
Output will be written into the directory: /vortexfs1/scratch/yaamini.venkataraman/02-directional/
Setting the option '--clip_r2 2' (to remove methylation bias from the start of Read 2)
gzip: /vortexfs1/home/naluru/Killifish/WHOI-Mummichog_RRBS/190626_I114_FCH7TVNBBXY_L4_OC-S5_1.fq.gz: Permission denied
Input file '/vortexfs1/home/naluru/Killifish/WHOI-Mummichog_RRBS/190626_I114_FCH7TVNBBXY_L4_OC-S5_1.fq.gz' seems to be completely empty. Consider respecifying!
```

I know a copy of these files exists at `/vortexfs1/scratch/yaamini.venkataraman/01-fastqc` from when I did my initial QC, so I changed the script parameters to see if I could run TrimGalore from there. Thankfully that worked!

### Issues with sequences

Since Neel wasn't sure what was going on and doing a deep dive on the TrimGalore! Github wasn't helpful, I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1401) to see if Sam had seen anything like this before. He said the sequence looked corrupted, and to confirm checksums with where the files were originally downloaded and the sequencer. If the files are not corrupted, then I may need to try using a different trimming software. I emailed Neel to see if I can get the original checksums.

### Results

In the meantime, I decided to look at the `multiqc` reports for my samples. I used `rsync` to move files from `vortex` mounted on my computer [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/02-trimgalore) for non-directional input and [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/02-directional) for directional input.

Looking at the [non-directional `multiqc` report](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/02-trimgalore/multiqc_report.html), all samples looked good in terms of adapters! The per tile sequence qualtiy, per sequence base content, and sequence duplication failed, but that makes sense because it's RRBS data and highly duplicated. When I looked at the [directional `multiqc` report](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/02-directional/multiqc_report.html), the per base N content looked worse for a few samples. This could be a slight indication that the data is non-directional, but I won't know until I try mapping.

### Going forward

1. Troubleshoot alignment code with a few samples
2. Figure out what's happening with sample 190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz
3. Trim sample 190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz appropriately
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
