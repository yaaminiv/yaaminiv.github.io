---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 18
tags: hawaii gigas-ploidy blast uniprot GOterms
---

## Functional annotation for new *C. gigas* genome

Now that I have DML lists from `methylKit` and `DSS`, I need to determine if any biological processes associated with genic DML are enriched. The first step in that process is getting annotation information for the Roslin genome!

### The game plan

But first, the larger game plan. I know gene enrichment methods aren't standardized in our lab yet, so I did some digging to see what other papers have used. Here are some of the methods I saw:

- My *C. virginica* paper: `GO-MWU`, then examination of GO Slim terms
- [Li et al. 2018](https://advances.sciencemag.org/content/4/8/eaat2142): Enrichment with `topGO`
- [Chile et al. 2021](https://www.biorxiv.org/content/10.1101/2021.04.14.439692v1): Enrichment with `goseq`, visualization with `simplifyEnrichment`

I liked the visualizations from Chile et al. (2021), so I dug into the workflow more. They used several databases to actually get GOterms: `DIAMOND blastx` to the NCBI database, InterPro Scan for GO and KEGG terms, Blast2GO to combine `DIAMOND` and InterPro Scan output, and Uniprot matching to pick up anything else. I didn't think this was necessary for my immediate purposes, but it's good to consider for later.

Here's what I'll do for functional annotation and gene enrichment in the meantime:

- `blastx` to Uniprot database
- Match accession terms to GOterms using lab workflow
- Work through `goseq` and `simplifyEnrichment` to produce gene enrichment figures

I'll tackle the functional annotation part in this notebook post.

### `DIAMOND blastx` struggles

Sam has been using `DIAMOND` to run `blastx` on `mox` in a more time-efficient manner. I decided to do the same! I created [this script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/11-DIAMOND-blastx.sh) in my Manchester analysis repository to annotate the Roslin genome. I noticed there were already Uniprot databases on `mox`, so I didn't download another one. I ran the following code based on some modifications Sam made in [a script he ran previously](https://robertslab.github.io/sams-notebook/2020/01/23/Transcriptome-Annotation-Hematodinium-MEGAN-Trinity-Assembly-Using-DIAMOND-BLASTx-on-Mox.html):

```
# Exit script if any command fails
set -e

# Program paths
diamond=/gscratch/srlab/programs/diamond-2.0.4/diamond

# Run DIAMOND with blastx
# Output format 6 produces a standard BLAST tab-delimited file
${diamond} blastx \
--db /gscratch/srlab/blastdbs/uniprot_sprot_20200123/uniprot_sprot.dmnd \
--query /gscratch/scrubbed/yaaminiv/data/cgigas_uk_roslin_v1_genomic-mito.fa \
--out 20210526-cgigas-roslin-blastx.outfmt6 \
--outfmt 6 \
--evalue 1e-4 \
--max-target-seqs 1 \
--block-size 15.0 \
--index-chunks 4
```

This took 20 minutes to run! When I counted the lines of the output, I only had 209 entries...This didn't seem right to me, so I posted [this issue](https://github.com/RobertsLab/resources/discussions/1226). I also decided to try running `DIAMOND blastx` using the just the somatic sequence and not the mitochondrial sequence. Since there was no methylation in the mitochondrial sequence to begin with, I thought there was no harm in just translating the original sequence in case the mitochondrial information I appended was causing the program to freak out. I got the Roslin genome sequence [from NCBI](https://www.ncbi.nlm.nih.gov/assembly/GCF_902806645.1/) and saved it [here](gannet.fish.washington.edu/spartina/project-oyster-oa/data/Cg-roslin/GCF_902806645.1_cgigas_uk_roslin_v1_genomic.fna) for future reference. I then modified my `DIAMOND blastx` code to use the somatic FASTA as the query:

```
${diamond} blastx \
--db /gscratch/srlab/blastdbs/uniprot_sprot_20200123/uniprot_sprot.dmnd \
--query /gscratch/scrubbed/yaaminiv/data/GCF_902806645.1_cgigas_uk_roslin_v1_genomic.fna \
--out 20210526-cgigas-roslin-blastx.outfmt6 \
--outfmt 6 \
--evalue 1e-4 \
--max-target-seqs 1 \
--block-size 15.0 \
--index-chunks 4
```

I realized too late that I didn't modify the output file name! In any case, I moved the output to [this `gannet` location](https://gannet.fish.washington.edu/spartina/project-oyster-oa/data/Cg-roslin/20210526-cgigas-roslin-blastx.outfmt6) prior to the line count investigation. Once `DIAMOND blastx` finished running, I renamed the output `20210601-cgigas-roslin-somatic-blastx.outfmt6` and saved both files in [this `gannet` folder](https://gannet.fish.washington.edu/spartina/project-oyster-oa/data/Cg-roslin/). The new output also only had 208 lines. My guess is that there's something in my database. I looked at the README file associated with it and saw the following:

```
Directory containing files for BLASTing against the Uniprot SwissProt database.

---

FILES

- uniprot_sprot.fasta: Uniprot SwissProt protein FastA file. Downloaded 20200123.

- uniprot_sprot.dmnd: DIAMOND Uniprot (Reviewed) BLAST database generated with the following command:

makedb --in uniprot_sprot.fasta --threads 27 --db uniprot_sprot --taxonmap ../ncbi-nr-20190925/prot.accession2taxid --taxonnodes ../ncbi-nr-20190925/nodes.dmp
```

Based on the `--taxonmap` argument, my guess is that I shouldn't be using this database for my needs! I decided to modify my `DIAMOND blastx` code to create a database first, then proceed with `blastx`:

```
# Create database from Uniprot-SwissProt information
# Use 27 threads to create database
# Save to srlab/yaamini/blastdbs
${diamond} makedb \
--in /gscratch/srlab/blastdbs/uniprot_sprot_20200123/uniprot_sprot.fasta \
--threads 27 \
-d /gscratch/srlab/yaaminiv/blastdbs/20210601-uniprot-sprot.dmnd

# Run DIAMOND with blastx
# Output format 6 produces a standard BLAST tab-delimited file
${diamond} blastx \
--db /gscratch/srlab/yaaminiv/blastdbs/20210601-uniprot-sprot.dmnd \
--query /gscratch/scrubbed/yaaminiv/data/cgigas_uk_roslin_v1_genomic-mito.fa \
--out 20210601-cgigas-roslin-mito-blastx.outfmt6 \
--outfmt 6 \
--evalue 1e-4 \
--max-target-seqs 1 \
--block-size 15.0 \
--index-chunks 4
```

I still only got 209 entries! So then I decided to go one step further back: downloading the Uniprot-SwissProt database from the website, then using that as input in `DIAMOND blastx`. When I ran the code, I still only got 209 matches! Then I realized I may have been missing an important argument from my code to make the database. I tried this next:

```
# Create database from Uniprot-SwissProt information
# Use 27 threads to create database
# Save to srlab/yaamini/blastdbs
${diamond} makedb \
--in /gscratch/srlab/blastdbs/uniprot_sprot_20200123/uniprot_sprot.fasta \
-dbtype prot \
--threads 27 \
-d /gscratch/srlab/yaaminiv/blastdbs/20210601-uniprot-sprot.dmnd
```

That failed immediately because it's not a valid argument for `DIAMOND makedb`. [Sam suggested I use `-long-reads`](https://github.com/RobertsLab/resources/discussions/1226#discussioncomment-813918), since `DIAMOND blastx` is developed for shorter reads. This started a series of me trying `--long-reads` along with different RAM usages, explicit `--range-culling`, `-F 15` and `--top 10`, and changing `--block-size`. None of those modifications worked! So this is when I give up.

### Going forward

1. Perform enrichment
2. Conduct randomization test with `DSS`
2. Try [EpiDiverse/snp](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
1. Run `methylKit` randomization test on `mox`
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)
6. Update methods
7. Update results
8. Create figures

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
