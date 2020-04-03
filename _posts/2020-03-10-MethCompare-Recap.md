---
layout: post
comments: true
title: MethCompare Recap
tags: MethCompare
---

## Comparing methylation analysis methods for two coral species

Hollie was in town this weekend, which meant one thing: writing retreat! Over the past few days, we started analyzing WGBS, RRBS, MBD-BSSeq, and RNASeq data for *Montipora capitata* and *Pocillopora acuta.* The goal of this project is an extenson of what [FROGER](https://yaaminiv.github.io/FROGER-Recap/) set out to do last year: figure out how the efficient and accurate the different enrichment methods are in comparison to WGBS. Here's how I contributed to our project.

### Wrote up analysis methods

In this process, we learned a lot about different trimming recommendations from kits and `bismark`. Originally, Sam used `fastp` to trim the raw WGBS, RRBS, AND MBD-BSSeq data. He used a 10 bp hard trim for WGBS and MBD-BSSeq, and a 2 bp hard trim to replicate suggestions from `bismark`. However, when Mac looked at the data we found that there was this artificial methylation signal at the beginning of the data! When we looked at the RRBS library, we saw that the generated libraries were non-directional, while RRBS libraries are generally directional. We realized that we couldn't specify non-directional data using `fastp` so we went back to TrimGalore! for all methylation data. Using TrimGalore! we were able to specify `-non_directional` and `-rrbs` options to properly trim the data.

### Generated genome feature tracks

I started by downloading the *M. capitata* and *P. acuta* genomes in [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/tree/master/genome-feature-files). Looking at the GFF files, I could tell what pre-generated tracks already existed. *M. capitata* had gene, CDS, and intron tracks. I need to do some digging in the future to confirm this, but based on some preliminary IGV analysis, I'm pretty sure the CDS track is equivalent to an exon track. The *P. acuta* genome had two different annotation files, so Hollie recommended I use the ab intio output. Similar to the *M. capitata* GFF, I looked at the annotation to figure out what tracks were already created. There were some tracks that were marked with "AUGUSTUS" and other tracks from a different source. After doing a lot of digging into the genome paper and AUGUSTUS methods, I learned that the other stuff was transcriptomic information that was fed into the AUGUSTUS predictive model. Based on the results presented in the genome paper, I decided to only use annotations from AUGUSTUS. The *P. acuta* genome feature tracks include genes, transcripts, exons (initial, internal, and terminal are also differentiated), CDS, and introns. I don't think I'll need to mess with these tracks any further.

### Characterized CpG methylation

I created this [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.ipynb) to set up a CpG methylation characterization pipeline. I used a lot of code from my [*C. virginica* paper](https://github.com/epigeneticstoocean/paper-gonad-meth/blob/master/code/09-Characterizing-CpG-Methylation.ipynb). To test the pipeline, I downloaded 10x bedgraphs for a subset of *M. capitata* methylation data. I added an "Mcap" tag at the end of each file so I could use a wildcard to treat all files at the same time without mixing up different species. For each sample, I subset the number of methylated (> 50%), sparsely methylated (10-50%), and unmethylated (< 10%) CpG loci. I created a summary table for all of this information. Working with the subset, RRBS captured more CpG loci than WGBS and MBD-BSSeq; however, most of these loci were unmethylated. I put this data together for each method in a preliminary figure within [this R Markdown file](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.Rmd). It's rough so I won't include the figure, but it exists.

Using the genome feature tracks I created, I started characterizing the genomic locations of CpG loci. I started by creating BEDfiles. Each sample had four associated BEDfiles: all data, methylated, sparsely methylated, and unmethylated. Using loops with `intersectBed`, I identified which CpG loci overlaped with genic and intergenic regions. Again, I put summary tables in the Jupyter notebook based on the subsets. MBD-BSSeq seems to capture the most methylated CpG loci within gene bodies, while WGBS and RRBS captured high percentages of unmethylated CpGs within genes.

The last thing I did was set up code to identify methylation islands. In the future, I'll need to concatenate the full data from all samples somehow, then use that concatenation file to identify methylation islands. I can then figure out if islands are within or outside of genes.

### Other things

- Helped organize the [MethCompare Github](https://github.com/hputnam/Meth_Compare) so it would be easier for us find scripts and analysis output
- Wrote up lab methods for WGBS, RRBS, MBD-BSSeq library preparation based on lab notebook entries from the Putnam lab and commercial protocols
- [Created a project](https://github.com/hputnam/Meth_Compare/projects/1) so we can track our progress when we continue working remotely

### Going forward

1. Figure out if *M. capitata* CDS are exons and if not, derive an exon track
2. Create promoter, UTR, and intergenic region tracks for both species
2. Rerun the CpG characterization pipeline with full samples and incorporate new genome features
3. Create concatenation files and figure out methylation island analysis

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
