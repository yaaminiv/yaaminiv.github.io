---
layout: post
comments: true
title: CEABiGR Part 13
tags: virginica CEABiGR gene-expression alt-splicing
---

## Counting DML in genes with a predominant isoform shift

At what I thought was the end of my predominant isoform analysis, I [realized I should count DML in genes with a predominant isoform shift](https://yaaminiv.github.io/CEABiGR-Part12/). I then [created this issue](https://github.com/sr320/ceabigr/issues/99) to keep track of my revelation. So today, let's tackle my revelation.

### Creating input files

In the [same issue](https://yaaminiv.github.io/CEABiGR-Part12/), I pinged Steven to ask if there were already files with DML and gene annotations available. I poked around the repo and couldn't find anything, so I decided to make my own. I returned to [this Jupyter notebook](https://github.com/sr320/ceabigr/blob/main/code/Genomic-Location-of-DML.ipynb) to use `bedtools` for annotations:

```
#Find overlaps between DML and gene list with annotations
#Save overlaps with annotations

!{bedtoolsDirectory}intersectBed \
-wb \
-a {femaleDML} \
-b ../../genome-features/C_virginica-3.0-gene.gff \
> female_dml-Gene-wb.bed

!{bedtoolsDirectory}intersectBed \
-wb \
-a {maleDML} \
-b ../../genome-features/C_virginica-3.0-gene.gff \
> male_dml-Gene-wb.bed
!head male_dml-Gene-wb.bed
```

The output files are in [this folder](https://github.com/sr320/ceabigr/tree/main/output/DML-characterization).

### DML presence

I counted the number of DML in relevant genes in [this R Markdown file](https://github.com/sr320/ceabigr/blob/main/code/42-predominant-isoform.Rmd). First, I imported the DML data:

```
DMLLocationsFemale <- read.delim("../DML-characterization/female_dml-Gene-wb.bed", sep = "\t", header = FALSE, col.names = c("chr", "start", "end", "f_DML", "meth.diff", "gene.chr", "Gnomon", "gene", "gene.start", "gene.end", "V11", "strand", "V13", "V14")) %>%
  dplyr::select(., "chr", "start", "end", "meth.diff", "gene.start", "gene.end", "V14") %>%
  separate(., col = V14, into = c("gene_name", "misc"), sep = ";Dbxref=GeneID:") %>%
  dplyr::select(., -misc) %>%
  mutate(., gene_name = gsub(pattern = "ID=gene-", replacement = "", x = gene_name)) #Import bedfile with DML locations in genes with gene ID annotations. Rename columns as file is imported then select columns of interest. Separate gene ID annotation information from misc information, and remove misc column. Remove extra information before gene ID

DMLLocationsMale <- read.delim("../DML-characterization/male_dml-Gene-wb.bed", sep = "\t", header = FALSE, col.names = c("chr", "start", "end", "f_DML", "meth.diff", "gene.chr", "Gnomon", "gene", "gene.start", "gene.end", "V11", "strand", "V13", "V14")) %>%
  dplyr::select(., "chr", "start", "end", "meth.diff", "gene.start", "gene.end", "V14") %>%
  separate(., col = V14, into = c("gene_name", "misc"), sep = ";Dbxref=GeneID:") %>%
  dplyr::select(., -misc) %>%
  mutate(., gene_name = gsub(pattern = "ID=gene-", replacement = "", x = gene_name)) #Import bedfile with DML locations in genes with gene ID annotations. Rename columns as file is imported then select columns of interest. Separate gene ID annotation information from misc information, and remove misc column. Remove extra information before gene ID
```

I then joined the data with the wide-format data:

```
predIsoFemWideDML <- left_join(predIsoFemWide, DMLLocationsFemale, by = "gene_name") %>%
  dplyr::select(., -c(chr, gene.start, gene.end)) %>%
  group_by(., gene_name) %>%
  mutate(., DMLcount = n()) %>%
  mutate(., DMLcount = case_when(is.na(meth.diff) == TRUE ~ 0,
                                 is.na(meth.diff) == FALSE ~ DMLcount)) #Join pred iso data with DML annotations and remove superfluous columns. Group by gene name and count the number of DML per gene. When there is no DML in the gene (meth.diff == NA), change the DML count to 0.
```

I saved the dataframe [here](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/fem-pred-isoform-DML-annotations.csv). Then, I counted how many genes had a particular number of DML:

```
predIsoFemWideDML %>%
  group_by(., DMLcount, change) %>%
  summarise(., count = n()) #Group data by DML count and change, then count the number of genes with or without predominant isoform shifts with various DML counts
```

**Table 1**. Female genes with DML counts

| **DML count** | **Shift?** | **Gene Count** |
|:-------------:|:----------:|:--------------:|
|       0       |      0     |      30253     |
|       0       |      1     |      2065      |
|       1       |      0     |       53       |
|       1       |      1     |        2       |
|       2       |      0     |       14       |
|       2       |      1     |        2       |
|       3       |      0     |        3       |

Majority of genes did not contain DML, irrespective of whether or not the predominant isoform shifted.

I did the same thing for the male data, which can be found [here](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/male-pred-isoform-DML-annotations.csv).

**Table 2**. Male genes with DML counts

| **DML Count** | **Shift?** | **Gene Count** |
|:-------------:|:----------:|:--------------:|
|       0       |      0     |      7692      |
|       0       |      1     |      1541      |
|       1       |      0     |       429      |
|       1       |      1     |       71       |
|       2       |      0     |       212      |
|       2       |      1     |       46       |
|       3       |      0     |       84       |
|       3       |      1     |       27       |
|       4       |      0     |       76       |
|       4       |      1     |        8       |
|       5       |      0     |       35       |
|       5       |      1     |        5       |
|       6       |      0     |       24       |
|       7       |      0     |       14       |
|       8       |      0     |       16       |
|       9       |      1     |       18       |
|       13      |      0     |       13       |
|       14      |      1     |       14       |
|       15      |      1     |       15       |
|       17      |      0     |       17       |
|       23      |      0     |       23       |
|       25      |      1     |       25       |

My next move was to plot this very unexciting data.

```
predIsoFemWideDML %>%
  dplyr::select(., -c(start, end, meth.diff)) %>%
  unique(.) %>%
  ggplot(aes(x = DMLcount, fill = as.factor(change))) +
  geom_histogram() +
  facet_wrap(.~change, scales = "free_x", labeller = labeller(change = facet_labels)) +
  labs(x = "Number of DML", y = "Number of Genes") +
  scale_fill_manual(values = c(plotColors[5], plotColors[8]),
                    labels = c("No Shift", "Shift"),
                    name = "") +
  theme_classic(base_size = 15)
```

![Screenshot 2024-03-05 at 12 11 11 PM](https://github.com/sr320/ceabigr/assets/22335838/32f45dbf-d884-4f00-b9ec-ebdbe2398e16)

![Screenshot 2024-03-05 at 12 10 59 PM](https://github.com/sr320/ceabigr/assets/22335838/39912002-ab59-4e3b-ac74-5164e8684034)

**Figures 1-2**. Distributions of DML counts for genes with and without a shift in the predominant isoform for females (top) and males (bottom)

Although I don't think that DML count had any bearing on the shift in the predominant isoform, I needed to do my due diligence and add DML count into the model:

```
predIsoFemModel <- glm(I(change == 1) ~ change_meth + DMLcount +
                         log10(length) + log2(change_exp + 1),
                       family = binomial(),
                       data = (predIsoFemWideDML %>% dplyr::select(., -c(start, end, meth.diff)) %>% unique(.) %>% ungroup(.))) #Create a binomial model, where 1 = the predominant isoform shifted when comparing low and control pH conditions. Use change in methylation, gene length, and change in gene expression as explanatory variables
```

This variable was retained in the final model for both [females](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/fem-predo-isoform-model-output.csv) and [males](https://github.com/sr320/ceabigr/blob/main/output/42-predominant-isoform/male-predo-isoform-model-output.csv). Guess I have some tables, methods, and results to update.

### Going forward

1. Update methods
2. Update results
2. Finish maximum transcript analysis
2. Perform formal DML enrichment analysis
4. Quantify DML in genes with a change in maximum transcript
2. Update methods and results
2. Compare with Aranda and Liew models in discussion section
4. Read recent papers
3. Write discussion
4. Write abstract
5. Outline introduction
6. Write introduction
7. Send to co-authors for approval
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
