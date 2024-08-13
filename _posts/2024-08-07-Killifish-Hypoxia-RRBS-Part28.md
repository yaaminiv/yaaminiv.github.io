---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 28
tags: killifish RRBS
---

## DMR-DEG overlaps

Picking up again! The next thing I want to do is examine overlaps and correlations between DMR and DEG. I'll start by using the output from `closestBed`.

But first, a sob story. I tried to use `bash` to `join` two files and it went horribly so instead I created [this R Markdown script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/08-annotate-DMR.Rmd). I used to be so much better at `bash`. Sigh.

The first thing I did was import the DMR and DEG data. A couple of notes:

- There were no DMR identified in any Scorton Creek contrast, so I'm only looking at New Bedford Harbor DMR between different oxygen conditions
- Neel did gene expression analysis with the hypoxia and normoxia conditions vs. outside control for both populations, but he didn't send similar `edgeR` output looking at DEG between hypoxia and normoxia. Since I have DMR for this contrast in New Bedford Harbor, it may be nice to still get that DEG information.

As I imported the data, I made some minor changes to formatting:

```
NBH20v0CDMR <- read.delim("20_OC_N_DMR.closestGene.bed", header = FALSE,
                          col.names = c("chr", "DMR.start", "DMR.end", "DMR", "direction", "gene.chr", "Gnomon", "gene", "gene.start", "gene.end", "V11", "strands", "V13", "V14", "distance")) %>%
  separate_wider_delim(., cols = "V14", delim = ";Dbxref", names = c("gene.name", "trash")) %>%
  mutate(., gene.name = gsub(x = gene.name, pattern = "ID=gene-", replacement = "")) %>%
  dplyr::select(chr, DMR.start, DMR.end, DMR, direction, gene.name, gene.start, gene.end, strands, distance)
  #Import DMR data and add column names. Separate column with gene name by a specific delimiter, then remove characters before gene name. Retain only necessary columns.
```

```
NBH20vOCDEG <- read.csv("../../../data/NEW-n.NBHvso.NBH.csv") #Import DEG data
colnames(NBH20vOCDEG)[1] <- "gene.name"
head(NBH20vOCDEG)
```

I then used `inner_join` to identify if there was accompanying `edgeR` output for my genes of interest:

```
NBH20v0CDMRDEG <- inner_join(NBH20v0CDMR, NBH20vOCDEG, by = "gene.name") %>%
  mutate(contrast = "20_OC_N") #Use inner join to identify DMR that overlap with DEG.
```

In total, I identified 16 genes between two contrasts that had accompanying `edgeR` output . Each contrast had one gene that contained a DMR. The remaining genes are from the New Bedford Harbor 5 vs. outside control contrast. While they came up as beign the closest gene to a DMR, they do not overlap with the DMR. None of these 16 genes are differentially expressed (or even close). The output can be found [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/08-annotate-DMR/new-genome/NBH-DMR-DEG-overlaps.csv).

### Going forward

1. Update methods and results
2. Try `BAT_correlating`
3. Create DMR figure
5. Identify known SNP/DMR overlaps
2. Conduct pathway analysis for RNA-Seq data by population
6. Update OSF repository for all intermediate files

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
