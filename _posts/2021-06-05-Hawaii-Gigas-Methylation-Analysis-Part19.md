---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 19
tags: hawaii gigas-ploidy blast uniprot
---

## Restarting functional annotation with `blastx`

TL;DR, I decided use standard `blastx` instead of `DIAMOND blastx` to annotate the new genome. More details on why I made that decision below.

### A struggle recap

I initially started using `DIAMOND blastx` to annotate the new *C. gigas* genome, but ran into [several issues](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part18/). Since I wasn't getting any useful output, Sam suggested I use the existing genome annotation in [this discussion](https://github.com/RobertsLab/resources/discussions/1226). I looked at [the annotation release](https://www.ncbi.nlm.nih.gov/genome/annotation_euk/Crassostrea_gigas/102/) and [FTP site](https://www.ncbi.nlm.nih.gov/genome/annotation_euk/Crassostrea_gigas/102/) and found the [annotation provided by the NCBI](https://ftp.ncbi.nlm.nih.gov/genomes/all/annotation_releases/29159/102/GCF_902806645.1_cgigas_uk_roslin_v1/GCF_902806645.1_cgigas_uk_roslin_v1_protein.faa.gz). While the annotation has protein ID and product information, I have no Uniprot Accession codes, which means I can't obtain GOterms.

At this point, Steven suggested I retrieve accession information through Uniprot mapping. Sam had a script I could modify to retrieve Uniprot Accession codes from the genomic GFF file. I modified [my Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/08-GOterm-Annotation.ipynb) to go through these steps based on [Sam's code](https://nbviewer.jupyter.org/github/RobertsLab/code/blob/master/notebooks/sam/20210601_ssal_gff-annotations.ipynb). I ran into a few perl module installation issues, which I detailed in [this discussion](https://github.com/RobertsLab/resources/discussions/1227). Once I troubleshooted the perl issues, I still wasn't getting any matches! Sam spot-checked a few gene IDs on the Uniprot website and didn't get matches either. Turns out the genome is new enough that the information is not on the Uniprot website!

I spot-checked the protein IDs from the protein annotation, and saw those were returning results. I then pivoted my strategy: use protein IDs to get Uniprot information, then find a different way to map protein IDs to gene IDs. To do this programmatically, I tried using the RefSeq annotation but encountered the same no-match result, which I detailed in [a new discussion](https://github.com/RobertsLab/resources/discussions/1228). At this point, Steven suggested I just use the web interface for Uniprot mapping, since my spot-check worked. When I tried uploading all of the protein IDs to the GUI, the interface was no longer available! Same when I tried just copying and pasting instead of uploading a file. Steven tried this as well, and ended up only getting a ~1000 of the ~60,000 entries mapping. Even then, they only mapped to themselves! So, `blastx` it is.

### Moving on to standard `blastx`

I reworked [this script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/11-blastx.sh) for `blastx` instead of `DIAMOND blastx`. It wasn't too difficult, as the syntax is mostly similar between the two programs.

# Exit script if any command fails
set -e

# Program paths
blast=/gscratch/srlab/programs/ncbi-blast-2.10.1+/bin/

# Create database from Uniprot-SwissProt information
# Speciy protein database
# Save to srlab/yaamini/blastdbs

${blast}makeblastdb \
-in /gscratch/srlab/yaaminiv/blastdbs/20210601-uniprot_sprot.fasta \
-dbtype prot \
-out /gscratch/srlab/yaaminiv/blastdbs/20210601-uniprot-sprot.blastdb

# Run blastx
# Output format 6 produces a standard BLAST tab-delimited file
${blast}blastx \
-db /gscratch/srlab/yaaminiv/blastdbs/20210601-uniprot-sprot.blastdb \
-query /gscratch/scrubbed/yaaminiv/data/cgigas_uk_roslin_v1_genomic-mito.fa \
-out 20210605-cgigas-roslin-mito-blastx.outfmt6 \
-outfmt 6 \
-evalue 1e-4 \
-max_target_seqs 1 \
-num_threads 28


### Going forward

1. Perform enrichment
6. Update methods
7. Update results
8. Create figures
5. Outline discussion
6. Write discussion
7. Write introduction
2. Conduct randomization test with `DSS`
2. Try [EpiDiverse/snp](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
1. Run `methylKit` or randomization test on `mox`
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)


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
