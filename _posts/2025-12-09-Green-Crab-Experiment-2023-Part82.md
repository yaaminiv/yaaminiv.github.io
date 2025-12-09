---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 82
tags: green-crab-wc metabolomics metaboanalyst
---

## Clearing up metabolomics discrepancies

I started writing the methods of the 2023 paper (yay!!!!!) and in this process I noticed a few discrepancies:

- Samples were taken (according to my spreadsheet) on day 21, but the metabolomics metadata says day 26
- I forgot that I only have 87 samples for RNA-Seq, so I have an extra one for metabolomics!

So here I am dealing with those discrepancies.

### Sampling day

I checked my [big project spreadsheet](https://docs.google.com/spreadsheets/d/1R5K1n6z4c0cr9n6310FkdLUbuUyYp_WdrMxzGDMAdZo/edit?gid=0#gid=0) and confirmed that day 26 was a Monday. I only sampled on a Wednesday/Thursday, so I must have sampled on day 21. Looks like the issue came up in the [crab genotype spreadsheet](https://docs.google.com/spreadsheets/d/1B1tyeCI7F_T-l41144m6k_MEVhhU-XCHAEkr6PHoTpw/edit?gid=35121745#gid=35121745). I fixed the error there, and in the [metabolomics metadata spreadsheet](https://github.com/yaaminiv/wc-green-crab/blob/main/metadata/metabolomics-sample-list.csv). I then went back and changed day 26 to day 21 in my [code](https://github.com/yaaminiv/wc-green-crab/blob/main/code/05-metabolomics-analysis.Rmd).

### Removing the extra sample

I checked the [crab genotype spreadsheet](https://docs.google.com/spreadsheets/d/1B1tyeCI7F_T-l41144m6k_MEVhhU-XCHAEkr6PHoTpw/edit?gid=35121745#gid=35121745) and confirmed that while 5-064 was extracted, I never used it for library prep since I dropped the sample and it got lost in the abyss of the RNA room. I removed this sample from the metabolomics dataframe:

```
transMetabData <- metabolomicsMetadata %>%
  left_join(x = ., y = genotypeData, by = "crab.ID") %>%
  dplyr::rename(treatment = treatment.x,
                original.treatment = treatment.y) %>%
  mutate(.,
         treatment_genotype = paste(treatment, final.genotype, sep = "_"),
         treatment_presenceC = paste(treatment, presenceC, sep = "_"),
         treatment_presenceT = paste(treatment, presenceT, sep = "_"),
         treatment_het = paste(treatment, het, sep = "_")) %>%
  left_join(x = .,
            y = rawMetabolomicsData %>%
              dplyr::select(., -c(2:8)) %>%
              column_to_rownames(., var = "BinBase.name") %>%
              t(.) %>% as.data.frame(.) %>%
              rownames_to_column(., var = "crab.ID") %>%
              mutate(crab.ID = gsub(x = crab.ID, pattern = "\\.", replacement = "-")),
            by = "crab.ID") %>%
  filter(., crab.ID != "5-064")
#Remove columns with MS information, convert BinBase.name to rownames, and transform dataframe. Convert rownames to crab.ID column. Replace . with - in IDs. Join with genotype data and metadata. Since some samples were taken from 25 and 5 treatments before temperatures were changed (day 0), the "treatment" information used for metabolomics is slightly different than the original treatment assignment. Modify different treatment column names to reflect this. Remove sample 5-064 since it doesn't have an RNA-Seq pair.
```

5-064 was sampled on day 21, which means I needed to redo any analysis that included samples from day 21. This would be the temperature only initial comparison, the temperature x experimental day comparison, and the temperature x timepoint comparison. Redoing the analyses didn't change much. I redid the ASCA and ASCA enrichment analyses just in case, which did not change any of the results. Then I got to the VIP lists and Metaboanalyst. After removing this sample, there were more VIP identified in all contrasts except early vs. late at 5ºC. I'm not sure why the 25ºC contrasts were strongly impacted by this. Perhaps removing the sample changed the overall PLS-DA structure? There's also the much more likely scenario: I didn't do my due diligence when adjusting some column numbers so I cutoff a bunch of VIP.

**Table 1**. Number of VIP for each contrast

|       **Contrast**       | **Total VIP** | **Higher in Condition 1** | **Higher in Condition 2** |
|:------------------------:|:-------------:|:-------------------------:|:-------------------------:|
| Early 5ºC vs. Early 25ºC |       69      |             1             |             68            |
|  Late 5ºC vs. Late 25ºC  |       60      |             14            |             46            |
|  Early 5ºC vs. Late 5ºC  |       42      |             37            |             5             |
| Early 25ºC vs. Late 25ºC |       69      |             63            |             6             |

I redid Metaboanalyst for the VIP lists and didn't see any change in significant pathways for the first three contrasts listed. Where I did see a difference was in the Early 25ºC vs. Late 25ºC, which makes sense since there are so many more identified VIP. In addition to starch and sucrose metabolism, arginine biosynthesis and alanine, aspartate, and glutamate metabolism were also enriched. These latter two pathways have quite a bit of overlap in the molecules represented, so that will be something interesting to look at when contexualizing these results.

### Going forward

1. Performance assay results
2. Metabolomics results
2. Index, get advanced transcriptome statistics, and pseudoalign with `salmon`
4. Clean transcriptome with `EnTap` and `blastn`
4. Identify differentially expressed genes
5. Additional strand-specific analysis in the supergene region
2. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper

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
