---
layout: post
comments: true
title: CEABiGR Part 15
tags: virginica CEABiGR
---

## Some notes

Things I want to keep track of while I'm writing.

### One file to rule them all

I created [this R Markdown script](https://github.com/sr320/ceabigr/blob/main/code/98-enrichment-result-compilation.Rmd) to combine enrichment output from all tests. I got the enrichment output from each test, then matched it with the specific gene background:

```
allRes.maxTransFemPosDataBP <- read.csv("../75-max-transcript-enrichment/fem-maxTransPos-BP-FisherTestResults.csv") %>%
  dplyr::rename(., pvalue = "classic", GO = "GO.ID", GOterm = "Term") %>%
  dplyr::select(., -c(Annotated, Significant, Expected)) %>%
  mutate(., category = "f_maxTransPos") %>%
  left_join(.,
            y = (read_delim("../75-max-transcript-enrichment/geneid2go-fem_maxTrans.tab", delim = "\t", col_names = c("gene_name", "GO")) %>% separate_longer_delim(., cols = GO, delim = ",")),
            by = "GO") %>%
  group_by(., GO) %>%
  unique(.) #Import enrichment results. Rename columns and retain columns of interest. Add a column characterizing enrichment result category. Join with gene background annotated with GOterms. For the gene background, import and separate GO terms into individual rows prior to joining. Group by GO term then retain unique rows
head(allRes.maxTransFemPosDataBP)
```

Then I compiled all output. I created columns with Y/N to indicate if a GOterm was enriched across several tests:

```
allRes.allFem <- rbind(allRes.maxTransFemPosDataBP, allRes.maxTransFemNegDataBP,
                       allRes.predIsoFemdataBP,
                       allRes.femAltSpliceDataBP,
                       allRes.femDMLGeneListDataBP) %>%
  left_join(., mRNAGOGOSlim, by = c("gene_name", "GO")) %>%
  dplyr::select(., -GOterm.x) %>%
  dplyr::rename(., GOterm = "GOterm.y") %>%
  separate(., col = product, into = c("product", "transcriptVar"), sep = "%2C transcript") %>%
  dplyr::select(., gene_name, GO, GOterm, GOslim, pvalue, category) %>%
  unique(.) %>%
  group_by(., gene_name) %>%
  pivot_wider(., names_from = "category", values_from = "pvalue") %>%
  arrange(., gene_name, GO) %>%
  mutate(., f_geneActivity = case_when(f_maxTransPos < 0.01 & f_maxTransNeg < 0.01 & f_predIso < 0.01 & f_altSplice < 0.01 ~ "Y",
                                       f_maxTransPos >= 0.01 | f_maxTransNeg >= 0.01 | f_predIso >= 0.01 | f_altSplice >= 0.01 ~ "N")) %>%
  mutate(., f_geneActivityNoSplice = case_when(f_maxTransPos < 0.01 & f_maxTransNeg < 0.01 & f_predIso < 0.01 ~ "Y",
                                               f_maxTransPos >= 0.01 | f_maxTransNeg >= 0.01 | f_predIso >= 0.01 ~ "N")) %>%
  mutate(., f_geneActivityNoSpliceDML = case_when(f_maxTransPos < 0.01 & f_maxTransNeg < 0.01 & f_predIso < 0.01 & f_DML < 0.01 ~ "Y",
                                                  f_maxTransPos >= 0.01 | f_maxTransNeg >= 0.01 | f_predIso >= 0.01 | f_DML >= 0.01 ~ "N")) %>%
  mutate(., f_allTests = case_when(f_maxTransPos < 0.01 & f_maxTransNeg < 0.01 & f_predIso < 0.01 & f_altSplice < 0.01 & f_DML < 0.01 ~ "Y",
                                   f_maxTransPos >= 0.01 | f_maxTransNeg >= 0.01 | f_predIso >= 0.01 | f_altSplice >= 0.01 | f_DML >= 0.01 ~ "N")) #Combine all separate enrichment results using rbind. Join with annotation information by gene and GOterm. Remove GOterm column from topGO and rename and keep column with GOterm annotations since these are not truncated. Isolate product information and select columns of interest. Retain only unique rows. Group by gene name and pivot p-value information such that each enrichment test has a separate column for p-values. Arrange by gene name, then GO ID. Create summary columns detailing if the GO term was 1) enriched across all gene activity tests (geneActivity), gene activity tests besides alt splicing (geneActivityNoSplice), gene activitiy + DML - splice (geneActivityNoSpliceDML), and all enrichment tests (allTests). NAs will be used if a value is missing for one of the columns.
head(allRes.allFem) #Confirm changes
```

All output is saved in [this folder](https://github.com/sr320/ceabigr/tree/main/output/98-enrichment-result-compilation).

### Paper notes and thoughts

McNally:
- Parental exposure lead to increased shell growth rates and shell size, with a more pronounced effects when larvae were in elevated pCO2 conditions
- No effect on survival

Teng et al. (2024): DNA methylation in oysters is influenced by genetics and sex
- Females had higher levels of hypomethylation than males in muscle (adductor) tissue
- Tissue specificity of methylation exists BUT we also saw lower levels of methylation in female gonad when compared to males
- Can be due to cell type differences: oocytes have higher levels of maternal RNA, while sperm have higher levels of DNA
- Differentially methylated regions in adults were intermediately methylated in offspring, suggesting methylation is inherited

Sun et al. (2024): The role of DNA methylation reprogramming during sex determination and sex reversal in the Pacific oyster Crassostrea gigas
- Global methylation increases when transitioning from female to male → consistent with lower levels of methylation in females
- DNA methylation underlies sex determination → another reason why they were separated for analyses

Sun et al. (2022): DNA methylation differences between male and female gonads of the oyster reveal the role of epigenetics in sex determination
- Males have higher methylation than females in goad tissue

Broquard et al. (2021)
- Specific transcriptome patterns for male and female C. gigas during reproductive maturation

Anastasidadi et al. (2018): Consistent inverse correlation between DNA methylation of the first intron and gene expression across tissues and species
- Conserved across vertebrate species
- More tissue-specific methylation in first intron of gene
- Inverse association of first intron methylation and gene expression

Wan et al. (2022): DNA methylation and gene transcription act cooperatively in driving the adaptation of a marine diatom to global change
- Methylation peaks positively correlated with expression in Phaeodactylum tricornutum adapted for two years to OA and/or warming conditions
- DMR regulated various metabolic and biosynthetic pathways in ~20% of DEG
- Relationship between methylation and gene expression in response to OA is taxa specific

Bogan and Yi (2024): Potential Role of DNA Methylation as a Driver of Plastic Responses to the Environment Across Cells, Organisms, and Populations
- GBM may reduce transcriptional noise by canalizing transcription or preventing spurious transcription as opposed to regulation of specific pathways → can address by looking at enrichment results
- Does methylation precede or follow differential expression? → limited GE but more extensive DM response suggests that DM goes first? OR after 30 days, DM is used to maintain gene expression homeostasis
- Hsp90 and other chromatin modifiers can be differentially regulated by environmental factors, “freeing” cryptic epigenetic variation that influences gene expression
  - Epigenetic capacitor hypothesis

Sarda et al. 2012: The evolution of invertebrate gene body methylation
- Similar to Gavery and Roberts 2012: high methylation in housekeeping genes
- Marine invertebrates have lower levels of methylation in shorter genes → potentially less opportunities for methylation?

Li et al 2018: DNA methylation regulates transcriptional homeostasis of algal symbionts in the coral model Aiptasia
- Spurious transcription: partial transcripts leading to truncated proteins
- Positive correlation between methylation and expression, suggesting either methylation increased gene expression or methylation is increased as a response to expression
- Association of H3K36me3 histone modification with methylation suggests that methylation is a consequence of expression
- DEG and DMG limited overlap

Dixon et al 2016: Evolutionary consequences of DNA methylation in a basal metazoan
- General paradigm from this paper and Gavery and Roberts 2013/Wang 2015: intermediately methylated genes have high transcription, and lowly methylated and strongly methylated genes have lower transcription
- Could fit with the quadratic term used in transcriptional noise modeling

Abbott et al. (2024): Modifications to gene body methylation do not alter gene expression plasticity in a reef-building coral
- Hypothesized that methylation would canalise expression patterns after corals were moved to a different temperature treatment, and that there would be a correlation between GBM and expression plasticity
- Plastic gene expression was not correlated with change in gene body methylation
- TagSeq and mdRAD study → could bias molecular data collected

Peterson et al. (2024): Mixed Patterns of Intergenerational DNA Methylation Inheritance in Acropora
- Mix of heritable and non-heritable sites
- GBM inheritance is higher than intergenic inheritance
- Inheritance is tied to the specific locus, and not necessarily parental identity

Dixon and Matz (2022): Changes in gene body methylation do not correlate with changes in gene expression in Anthozoa or Hexapoda
- Across invertebrate taxa, methylation is associated with increased transcription and decreased transcriptional variation
- Little to no correlations between GBM and DEG

### Going forward

1. Write discussion
5. Outline introduction
6. Write introduction
7. Send to co-authors for approval
8. Write cover letter
8. Submit to journal special issue

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
