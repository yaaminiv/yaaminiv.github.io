---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 23
tags: killifish RNA-Seq
---

## Pathway analysis for RNA-Seq data

I'm continuing to dig into the RNA-Seq data so I can understand the results and create figures for the manuscript! For each population, I'm looking at differences between normoxia and hypoxia.

### Visualize DEG

First things first, I wanted to visualize the DEG! Unfortunately, I couldn't find a file with logCPM information for all the samples. The closest I got was one with 5 samples from each population. I figured this was good enough to mess with `pheatmap` code. I created output pdfs just to ensure the code worked, but it's not worth looking at since I'm pretty sure it's just samples for hypoxia for each population.

### Overlapping DEG

The first thing I wanted to do was determine if there were any common DEG between New Bedford Harbor and Scorton Creek. In [this R Markdown document](), I used `inner_join` to get common gene IDs and logFC for both contrasts. I saved the list of common DEG [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/09-RNA-Seq/common-DEG.csv) and created a [heatmap](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/09-RNA-Seq/commonDEG-heatmap.pdf) to visualize if there were common DEG that were differentially expressed in opposite directions.

<img width="690" alt="Screenshot 2022-11-07 at 4 12 16 PM" src="https://user-images.githubusercontent.com/22335838/200416480-5b1ae691-a131-4507-b9a2-e65e181e85df.png">

**Figure 1**. Heatmap of DEG common to both New Bedford Harbor and Scorton Creek fish exposed to hypoxia. Higher logFC indicates higher expression in fish exposed to hypoxia.

There aren't any genes differentially expressed in opposite directions. Based on the colors in the heatmap, I picked genes with the largest differences in FC:

- Funhe2EKm021334: NADH dehydrogenase subunit
- Funhe2EKm006719: RING finger protein
- Funhe2EKm002577: Class E basic helix loop helix protein 40
- Funhe2EKm039163: neuronal cell death-inducible putative kinase
- Funhe2EKm027081: uncharacterized protein

### Specific pathways

Before launching into a formal pathway analysis, I wanted to see if any genes from the HIF (related to treatment) or AHR (where the mutations that separate NBH and SC occured) pathways were differentially expressed.

Within the Scorton Creek population, I saw differential expression of HIF-PH1 (Funhe2EKm035892, upregulated) and HIF-PH4 (Funhe2EKm035088, downregulated), p300/CBP-associated factor (Funhe2EKm029335, downregulated), and HIF-1 responsive proteins (Funhe2EKm038583, upregulated). Prolyl hydroxylases are responsible for the first step of HIF degradation and p300/CBP-associated factor is a transcriptional co-activator, indicating regulation of the HIF pathway in low oxygen conditions for sensitive fish. Interestingly, no genes in the HIF pathway were differentially expressed in the tolerant New Bedford Harbor fish.

AHR is important for toxicant response, but also for hypoxia signaling! So I looked into the genes in the AHR pathway to see if there would be any differences between tolerant and sensitive fish. HSP90 (Funhe2EKm038380), CYP1A (Funhe2EKm038981), and TIPARP (Funhe2EKm033539) were all upregulated in sensitive fish exposed to low oxygen. HSP90 is a molecular chaperone that stabilizes HIF1A, and there's some indication that it's involved in regulating cell proliferation during hypoxia. TIPARP is important for turning off hypoxia-inducible factor. Again, no genes in the AHR pathway were DEG for New Bedford Harbor fish.

### Setting up a pathway analysis

The next step was to set up a pathway analysis. I found the [`rWikiPathways`](https://bioconductor.org/packages/release/bioc/vignettes/rWikiPathways/inst/doc/Pathway-Analysis.html) package which not only does a gene ontology analysis, but a pathway analysis! Since it requires Ensembl IDs and the killifish genome has those IDs, I thought it would be a good package to try. My first hurdle was installing the packages . The way the documentation is laid out has `rWikiPathways` installed prior to `GSEABase`, but there are some dependencies for `rWikiPathways` block installation of `GSEABase`. So I installed `GSEABase` separately first, then went through the rest of the installation code in the manual.

The second hurdle (and where I'm currently stuck) is mapping the transcript IDs from RNA-Seq data to Ensembl gene IDs. My RNA-Seq data has transcript IDs that start with "Funhe2EKm", but the genome files have Ensembl IDs that do not map to those transcript IDs! Neel suggested I use [biomart](http://useast.ensembl.org/biomart/martview) to get Ensembl IDs. However, this tool would give me the Ensembl IDs I already have without a way to map them to the transcripts! I wondered if these IDs were created by STAR when Neel did the mapping. I looked in the Dropbox folder to see if I could find a mapping file, but no luck. I'll email Neel when he gets back from vacation.

### Going forward

1. Map STAR transcript names to Ensembl IDs from genome
2. Conduct pathway analysis for RNA-Seq data by population
5. Identify known SNP/DMR overlaps
1. Update methods and results
1. Figure out why there is such low methylation with the new genome
2. Continue BAT workflow with new genome
6. Create OSF repository for all intermediate files

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
