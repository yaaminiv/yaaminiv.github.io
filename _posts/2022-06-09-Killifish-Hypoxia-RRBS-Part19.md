---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 19
tags: killifish RRBS poseidon BAT
---

## Wrapping up the initial methylation analysis

I have a few loose ends to tie up before finishing my preliminary methylation analysis: methylation landscape information and an overview of the closest annotated genes.

### Methylation landscape analysis

I tried looking through the output to figure out if this information was listed in a file anywhere. I didn't have the log files anymore, but I don't think it was listed there. In any case, if someone else isn't able to access the information I used, then it wasn't reproducible! So I returned to [this Jupyter notebook](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-methylation-landscape-analysis.ipynb) to revise the methylation landscape analysis. Neel said that I should recalculate my statistics using 0% for the cutoff for methylation (i.e. a CpG was either methylated or unmethylated, and it didn't matter how intensely it was methylated).

**Table 1. Methylation landscape information calculated using all common CpGs for each specific contrast.

| **Contrast** | **Methylated CpGs (%)** | **Unmethylated CpGs (%)** | **Average Methylation** |
|:------------:|:-----------------------:|:-------------------------:|:-----------------------:|
|  All samples |       7275 (48.8%)      |        7620 (51.2%)       |          20.7%          |
|    All NBH   |      12216 (82.0%)      |        2679 (18.0%)       |          20.6%          |
|    All SC    |      12536 (84.2%)      |        2359 (15.8%)       |          20.7%          |
|  Hypoxic NBH |      96857 (54.9%)      |       79429 (45.1%)       |          22.1%          |
| Normoxic NBH |      93895 (53.3%)      |       82391 (46.7%)       |          20.4%          |
|  Hypoxic SC  |      22694 (27.6%)      |       59611 (72.4%)       |          16.4%          |
|  Normoxic SC |      45661 (55.5%)      |       36644 (44.5%)       |          16.2%          |

Compared to what I have seen with other fishes, general methylation seems to be low. Interestingly, the tolerant NBH population doesn't have much of a difference between the methylated:unmethylated ratios after hypoxia exposure, but the sensitive SC population does (~70% unmethylated in hypoxia vs. ~45% in normoxia). Average percent methylation is also lower in the SC population. Seeing population differences makes me think more about identifying SNPs between these populations and trying to identify mQTLs.

### Closest genes to DMR

The next thing I wanted to do was dig into the functions of the closest genes to DMR. I used the ENSEMBL and NCBI databases to find this informaiton.

Overlapping genes:

- [iqgap1 (IQ motif containing GTPase activating protein 1)](https://www.ncbi.nlm.nih.gov/gene/105938828): enable GTPase activator activity, actin filament binding activity, and calmodulin binding activity, regulate cytoskeletal organization
- [aspa (aspartoacylase)](https://www.ncbi.nlm.nih.gov/gene/105932199): enable hydrolase activity
- [SHISA6 (protein shisa-6 homolog)](https://www.ncbi.nlm.nih.gov/gene/388336): enable ionotropic glutamate receptor activity, part of AMPA glutamate receptor complex, involved in synaptic density and membrane

Closest genes:

- [ENSFHEG00000011518 (kcnma1a; -22,011 bp away)](https://www.ncbi.nlm.nih.gov/gene/568554/): enable calcium-activated potassium channel activity, acts within auditory stiumlus response, active in postsynaptic membrane
- [ENSFHEG00000021357 (no gene name; -24,519 bp away)](https://www.ncbi.nlm.nih.gov/gene/105922866): major histocompatibility complex class I-related gene protein-like (not predicted in the latest *F. heteroclitus* annotation)
- [znf423 (-55,168 bp away)](https://www.ncbi.nlm.nih.gov/gtr/genes/23090/): zinc finger protein 423, DNA-binding transcription factor, roles in signal transduction

The fact that one of the DMR was close to a gene not present in the latest mummichog annotation supports the idea of re-doing RRBS and RNA-Seq alignment with the new genome! Something to consider after doing an initial pass of the data.

### Going forward

1. Revise methylation landscape information
1. Update methods and results
3. Match DMR with RNA-Seq information
2. Start mapping with new genome
2. Try DMR identification with `bismark` and `methylKit`
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
