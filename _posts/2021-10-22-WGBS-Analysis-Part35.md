---
layout: post
comments: true
title: WGBS Analysis Part 35
tags: manchester topGO
---

## Gene librarian for genes with DML associated with enriched GOterms

[Last week](https://yaaminiv.github.io/WGBS-Analysis-Part34/) I finally matched my list of enriched GOterms with gene IDs and methylation difference information. This week, I spent a lot of time playing gene librarian and understanding the functions of each of these genes in the gonad or during embryogenesis, probable responses to low pH stress, and overlaps between other studies.

### Methods

To separate my manual annotations and searches from tables I produced in R, I created [this Excel spreadsheet](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/output/12-functional-enrichment/manual-annotations/all-BP-EnrichedGO-DML-ManAnn.xlsx). I searched for each of the gene products in publicly available DML lists from the four other ocean acidification and methylation in oyster studies: Downey-Wall et al. 2020, Venkataraman et al. 2020, Chandra Rajan et al. 2021, and Lim et al. 2020. I also looked at the *C. gigas*-specific reproduction and gene expression papers: Dheilly et al. 2012 and Broquard et al. 2021. Important note with these papers: they use the oyster_v9 genome, while I used the Roslin genome. Finally, I searched for papers on Google Scholar examining these genes in marine invertebrates, but preferably bivalves. For these searches, I looked for low pH responses and/or egg-specific studies. I went through this process for biological process and cellular component lists separately, but there was some overlap between these gene lists.

### Biological processes: General functions and interesting notes

Overall, genes are mainly involved in developmental progression and regulation.

- Dynein heavy chain 5, axonemal: Associated with sperm (flagellar and cillar) motility). Ddynein heavy chain 1, 8, 12, cytoplasmic dynein 1 heavy chain 1-like, dynein heavy chain 1, and cytoplasmic dynein 2 heavy chain 1-like contained DML in *C. virginica* gonad (Venkataraman et al. 2020). Higher expression of dynein proteins over the course of spermatogeneisis in *C. gigas* (Dheilly et al. 2012). Dynein does not have strict functions relating to sperm motility. Differential abundance of dynein proteins was found in shotgun proteomic characterization of *C. gigas* ctenidia (Timmins-Schiffman et al. 2014), and can help move materials for calcification in response to low pH stress in mantle tissue (De Wit et al. 2018). Unfertilized sea urchin eggs contain several dynein proteins (Pratt 1980, Porter 1988). With new genome, it's possible that this annotation may be a cytoplasmic homologue, as we did not sequence any male DNA. Cytoplasmic dynein would be involved in organelle transport.
- Unconventional myosin-VI: Motor protein. Decreased expression during gametogenesis, with higher expression in immature gonad (Dheilly et al. 2012). The paramyosin homologue is highly expressed in females over males, with declining expression as gametes mature (Broquard et al. 2021). Unconventional myosin-XVIIIa-like contained a DML in *C. virginica* gonad (Venkataraman et al. 2020). Unconventional myosins were also found to be important for embryonic development in urchins, specifically to regulate the onset of blastulation and gastrulation stages (Sirotkin 2000). Changes in methylation of this gene could impact progression of gametogenesis or embryogenesis.
- Serine/threonine-protein kinase 36: Involved in signaling pathways. Higher expression of serine/threonine-protein kinases in mature female gonads (Dheilly et al. 2012), especially during sex determination processes (Broquard et al. 2021). Examination of the *C. gigas* kinome found several serine/threonine-protein kinases in eggs and embryos, with some gene expression changes in response to abiotic stress (Epelboin et al. 2016). Also found to be related to oocyte maturation in the king scallop (Pauletto et al. 2017). Several serine/threonine-protein kinases contained DML in *C. virginica* (Venkataraman et al. 2020), and one was differentially methylated in *C. hongkongensis* larvae. Methyalation of this gene could promote homeostasis by regulating gonad development processes.
- Helicase domino: Exchanges phosphorylated histone for acetylated form, with chromatin remodeling impacting gene expression. Potentially involved in oogenesis. Outlier SNP loci were found in this gene in *O. lurida*, potentailly related to immune or stress response (Silliman 2019). Potential methylation regulation of chromatin structure.
- Protein neuralized (neuralized-like protein): Involved in cell fate decisions. Part of the ubiquitin ligase family and broadly involved in protein ubiquitination processes (Hu 2005, Jiang 2011). Changes in protein abundance in *Strongylocentrotus purpuratus* egg and embryo related to early cleavage and initiation of gastrulation. Increased expression of this gene in *Artemia sinica* can suppress cell division and macromolecule synthesis (Jiang 2011), so methylation may be regulating gonad development.
- Cytoplasmic aconitate hydratase (aconitase): Catalyzes part of the TCA cycle. Found in *C. virginica* eggs and trocophores, with enzyme activity increasing as development progressed (Black 1962). Associated with energy metabolism during gametogenesis in *Pecten maximus* (Boonmee 2016), and similarly associated with energy production in *C. gigas* during spermatogenesis (Kingtong et al. 2013). Higher expression in *S. purpuratus* populations more exposed to low pH conditions after one day of low pH exposure (embryos), but lower expression after seven days of low pH exposure (larvae) (Evans et al. 2017). Methylation may "prime" embryos for low pH exposure and dictate energy metabolism.
- Serine-protein kinase ATM: Cell cycle checkpoint kinase, regulates downstream proteins, involved in cellular response to DNA damage. Gene contained DML in *C. virginica* gonad (Venkataraman et al. 2020). Associated with female gamete generation and development in *Sinonovacula constricta*. Mobilized under embroygenesis and during environmental stress, regulating cell-cycle progression (Epelboin et al. 2016). Involved in cellular stress response when *C. gigas* was exposed to low pH and As (Moreira et al. 2018).
- Cation-independent mannose-6-phosphate receptor: Transport lysosomal enzymes to lysosome. Potential involvement in *D. polymorpha* contaminant (Leprêtre et al. 2020) and *Marsupenaus japonicus* bacteria immune responses.

### Cellular components

- Lots of similar genes to BP! Dynein heavy chain 5, unconventional myosin, and helicase domino had enriched CC GOterms. Helicase domino was associated with histone and protein acetyltransferase complexes, and dynein and myosin were associated with microtubule complexes and the cytoskeleton.
- Cytoplasmic dynein 2 light intermediate chain 1-like: Forms a motor protein complex to transport items along microtubules in cilia and flagella. Similar findings to dynein heavy chain 5 (see above)
- Kinesin-like protein KIF23: Helps with organelle transport and required for cytokinesis. Increased expression over the course of spermatogenesis, and higher expression in mature male and female gonads (Dheilly et al. 2012). Differential expression between mature gonad and stripped oocytes (Dheilly et al. 2012). Kinesin-like protein KIF12 and KIF15 contained DML in *C. virginica* gonad (Venkataraman et al. 2020). Changes in kinesin-like protein gene expression have been seen in larval *S. purpuratus* exposed to low pH (Padilla-Gamiño et al. 2013) and *Pocillopora acuta* exposed to heat stress (Poquita-Du et al. 2020), and these changes could be associated with reduced calcification. Also associated with immune response in *C. gigas* (Lorgeril et al. 2011).

### Going forward

1. Look into unannotated GOterms and potential nesting
3. Determine if there's a bias towards hyper- or hypomethylated genic DML with enriched functions
2. Report `mc.cores` issue to [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html)
2. Perform randomization test
2. Update `mox` handbook with R information
3. Determine if larval DNA/RNA should be extracted

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
