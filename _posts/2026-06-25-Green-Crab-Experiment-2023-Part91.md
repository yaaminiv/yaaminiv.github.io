---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 91
tags: green-crab-wc RNA-Seq
---

## Transcriptome assembly modification

My current transcriptome assembly has almost 700k supertranscripts prior to filtering. Filtering out transcripts with TPM < 1 provides about ~200k supertranscripts. Carolyn suspects that there have been some changes to the `trinity` algorithm such that it spreads out information across a lot of smaller transcripts and/or creates a lot of assembly artifacts. Time to go down the rabbit hole to figure out how to proceed!

### Trinity manual

My first step was to review any information provided by `trinity` itself. I found [this page](https://github.com/trinityrnaseq/trinityrnaseq/wiki/There-are-too-many-transcripts!-What-do-I-do%3F) in the `trinity` Wiki that seemed particularly relevant. The bottom line of this page is that it is "normal" by `trinity` standards to have a lot of transcripts when performing a de novo assembly. While this may be the case, it doesn't provide any information about flags to alter within the transcriptome assembly itself to deal with this issue.

One thing Carolyn suggested was to look for any flags that may reduce the number of microsatellites (aka. very short repetitive elements in the transriptome). I don't remember any specific `trinity` flags to reduce microsatellites, but I went through the options again using `--show_full_usage_info` to see if there was anything else I missed. Here are a few things that jumped out:

- I did standard Illumina sequencing, so read 1 is the forward read and read 2 is the reverse read. While the the samples are listed correctly in my [sample list](https://github.com/yaaminiv/wc-green-crab/blob/main/metadata/RNA-Seq/trinity-samples.txt), I have `--SS_lib_type RF` instead of `--SS_lib_type FR` in [my final script](https://github.com/yaaminiv/wc-green-crab/blob/main/code/06c-trinity.sh). I looked in my lab notebook to double check what I did. Turns out [I used FR for the library type while running `trinity`](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part76/). However, after the script errored out, [I used RF for the library type](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part81/). So, this could be a big issue.
- I could increase the minimum contig size. I used the default of 200 bp, but I could increase that to 250 bp or 300 bp. FWIW, Zac found that the default worked well.
- Modify kmer size and coverage. Zac created several transcriptome with different kmer sizes (21, 25, 29) and coverages (1 vs. 2), then compared the transcriptome ExN50 values to determine which parameter set was best. He found that the default settings for kmer size (25) and coverage (1) produced the transcriptome with the highest ExN50 value. When looking at the full usage information, I only see options for kmer coverage and not size, which could be a versioning issue.

Another thing I'm trying to figure out is if I need to put in information from *all* my samples, or just the control. I've noticed that sometimes transcriptomes are assembled using all available data, other times just with data from a subset of the sequenced samples. I chose to use all of my samples in case there are "rare" isoforms in the 25ºC or 5ºC treatment that I'd want to include in the final transcriptome.

### Comparable de novo assemblies

I know that crustacean genomes tend to be large, but just how large are they? I figured I'd start looking at some other crustacean `trinity` de novo assemblies to start.

- Porcelain crab transcriptomes (assembled 2016): 200,000-300,000 contigs depending on the species, 1000-1500 bp mean length
- Zac's mud crab transcriptome (assembled 2021): 146,332 contigs with 577 bp average length; 60,488 transcripts after filtering

Alright, the [TPM < 1 filter gets me 201,238 supertranscripts](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part90/) prior to contaminant screening, which is closer to the porcelain crab transcriptome size. It is still much larger than the mud crab transcriptome that Zac assembled.

### Green crab genome

Interestingly, there is a [published green crab genome](https://academic.oup.com/jhered/article/117/3/537/8296971). I wonder if it would make sense to try a genome-guided assembly with my reads and see what happens? It would be interesting to see just how many unmapped reads I have. Additionally, comparing the de novo transcriptome with and without the genome as a guide, and then mapping reads directly to the existing genome, could be good for de novo quality assessment.

### Post-assembly clean-up

Contrary to what I thought previously, I think using a TPM < 1 filter after assembly is actually the better approach when using a hard TPM filter. Another potential option would be to use [`ROAST`](https://pmc.ncbi.nlm.nih.gov/articles/PMC10763045/). This is a external tool that I would use post-TPM filtering to clean up the de novo assembly.

### Action plan

1. Determine what samples should go into transcriptome assembly
2. Rerun `trinity` specifying the correct library type to determine if that improves the supertranscriptome assembly
3. Run `trinity` on a subset of data with different minimum kmer coverage and minimum contig length parameters
4. Attempt a genome-guided de novo assembly using the published green crab genome
5. Try `ROAST` with my existing supertranscriptome to see how it performs the clean-up to use if necessary
6. Based on results from previous steps, assemble a transcriptome and feed it through `EnTAP` so I can move on with my life

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
