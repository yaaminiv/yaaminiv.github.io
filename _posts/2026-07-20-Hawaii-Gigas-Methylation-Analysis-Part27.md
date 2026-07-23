---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 27
tags: hawaii gigas-ploidy bedtools
---

## Cleaning up methylation landscape analysis

While reviewing the method and code for this paper, I realized that I never removed C->T SNPs from the methylation landscape. The paper said I did, but the scripts did not reflect this! I did, however, remove C->T SNPs from the DML, so that's good. Anyways, while Steven was running my parameterization script, I figured I could update this analysis.

### Removing C->T SNPs

I added the following code chunk in [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/06-General-Methylation-Landscape.ipynb) to remove C->T SNPs from the 5x bedgraphs:

```
%%bash

for f in /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644*5x.bedgraph
do
/opt/homebrew/bin/subtractBed \
-a ${f} \
-b /Volumes/web/spartina/project-oyster-oa/Haws/BS-Snper/unique-CT-SNPs.bed \
> ${f}.NO-SNPs
done
```

I got this error when I checked the notebook again:

> bash: line 5: /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_union-averages_5x.bedgraph.NO-SNPs: No such file or directory
---------------------------------------------------------------------------
CalledProcessError                        Traceback (most recent call last)
Cell In[8], line 1
----> 1 get_ipython().run_cell_magic('bash', '', '\nfor f in /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644*5x.bedgraph\ndo\n/opt/homebrew/bin/subtractBed \\\n-a ${f} \\\n-b /Volumes/web/spartina/project-oyster-oa/Haws/BS-Snper/unique-CT-SNPs.bed \\\n> ${f}.NO-SNPs\ndone\n')
CalledProcessError: Command 'b'\nfor f in /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644*5x.bedgraph\ndo\n/opt/homebrew/bin/subtractBed \\\n-a ${f} \\\n-b /Volumes/web/spartina/project-oyster-oa/Haws/BS-Snper/unique-CT-SNPs.bed \\\n> ${f}.NO-SNPs\ndone\n'' returned non-zero exit status 1.

I also saw that my computer was asleep and the UW VPN was no longer connected! So I think the issue is that the loop did not complete for the final file (the union average bedgraph) due to a dropped remote connection. I logged into my VPN again and confirmed that there were SNP-free files for all of my other smaples. I then ran `subtractBed` on the union bedgraph:

```
!/opt/homebrew/bin/subtractBed \
-a /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_union-averages_5x.bedgraph \
-b /Volumes/web/spartina/project-oyster-oa/Haws/BS-Snper/unique-CT-SNPs.bed \
> /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_union-averages_5x.bedgraph.NO-SNPs
```

I confirmed that there were SNP-free files for all of my samples:

```
!find /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/*5x.bedgraph.NO-SNPs
```

> /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_1_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_10_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_11_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_12_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_13_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_14_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_15_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_16_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_17_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_18_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_19_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_2_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_20_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_21_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_22_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_23_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_24_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_3_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_4_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_5_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_6_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_7_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_8_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_9_R1_val_1_val_1_val_1_bismark_bt2_pe._5x.bedgraph.NO-SNPs
/Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/zr3644_union-averages_5x.bedgraph.NO-SNPs

Once the C->T SNPs were removed, I proceeded to identify methylated, sparsely methylated, and unmethylated CpGs in the 5x bedgraphs and counted the number of CpGs in each file:

```
%%bash
for f in /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/*5x.bedgraph.NO-SNPs
do
    awk '{if ($4 >= 50) { print $1, $2, $3, $4 }}' ${f} \
    > ${f}-Meth
done

%%bash
for f in /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/*5x.bedgraph.NO-SNPs
do
    awk '{if ($4 < 50) { print $1, $2, $3, $4}}' ${f} \
    | awk '{if ($4 > 10) { print $1, $2, $3, $4 }}' \
    > ${f}-sparseMeth
done

%%bash
for f in /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/*5x.bedgraph.NO-SNPs
do
    awk '{if ($4 <= 10) { print $1, $2, $3, $4 }}' ${f} \
    > ${f}-unMeth
done
```

```
#Get line counts for each fine
# Remove 26th line (total entries)
#Ensure output is tab-delimited
#Save output
!wc -l /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/*5x.bedgraph.NO-SNPs-Meth \
| sed '26,$ d' \
| awk '{print $1"\t"$2}' \
> zr3644_5x-NO-SNPs-Meth-counts.txt

#Get line counts for each fine
# Remove 26th line (total entries)
#Ensure output is tab-delimited
#Save output
!wc -l /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/*5x.bedgraph.NO-SNPs-sparseMeth \
| sed '26,$ d' \
| awk '{print $1"\t"$2}' \
> zr3644_5x-NO-SNPs-sparseMeth-counts.txt

#Get line counts for each fine
# Remove 26th line (total entries)
#Ensure output is tab-delimited
#Save output
!wc -l /Volumes/web/spartina/project-oyster-oa/Haws/methylation-landscape/*5x.bedgraph.NO-SNPs-unMeth \
| sed '26,$ d' \
| awk '{print $1"\t"$2}' \
> zr3644_5x-NO-SNPs-unMeth-counts.txt
```

The revised summary statistics can be found [here](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_06-methylation-landscape). I added the tag "NO-SNPs" so I knew which files were created before and after SNP removal. On `gannet`, I  put all files that included SNPs into a separate subdirectory to reduce any confusion.

My next step was to characterize overlap between the various CpG categories and genome feature files. When I started to run the code, I realized that the path to these genome feature files was no longer valid! I had to redownload the GFFs into a separate folder on `gannet` prior to my analysis:

```
wget -r --no-check-certificate --no-directories --no-parent --reject "index.html*" -P . -A "cgigas_uk_roslin_v1*" http://owl.fish.washington.edu/halfshell/genomic-databank/
```

Once the files were downloaded, I could begin the extremely slow process of determining how many highly methylated, moderately methylated, or lowly methyated CpGs were found in various genome feature files. All of the count information can be found in [here](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_06-methylation-landscape). I added the tag "NO-SNPs" and removed any files that did not have this tag to reduce confusion.

### Revising figures and statistics

The next step was to remake the stacked bargraph and statistical analysis in [this R Markdown script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/06-General-Methylation-Landscape.Rmd). The revised contingency test output can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_06-methylation-landscape/CpG-location-statResults.txt). This code ran without a hitch, but at the end I decided to remake the figure using `ggplot` instead of base R:

```
cpgFeatureOverlapsPercents %>% #Take dataframe
  rownames_to_column("genomeFeature") %>% #Make rownames into a genomeFeature column
  mutate(genomeFeatureOrdered = case_when(genomeFeature == "exonUTR" ~ "1exonUTR",
                                          genomeFeature == "CDS" ~ "2CDS",
                                          genomeFeature == "intron" ~ "3intron",
                                          genomeFeature == "upstream" ~ "4upstream",
                                          genomeFeature == "downstream" ~ "5downstream",
                                          genomeFeature ==  "lncRNA" ~ "6lncRNA",
                                          genomeFeature == "TE" ~ "7TE",
                                          genomeFeature == "intergenic" ~ "8intergenic")) %>% #Add numbers to the genomeFeature names so they are ordered correctly in the legend
  pivot_longer(., cols = 2:5, names_to = "category") %>% #Pivot CpG percent columns so that the dataframe is in long format
  ggplot(., aes(x = category, y = value, fill = genomeFeatureOrdered)) + #Create a plot with methylation category on the x-axis, percent CpGs on the y-axis, and color indicated by genome feature
  geom_bar(stat = "identity", color = "grey10") + #Stacked barplot with black outlines around the bars
  scale_x_discrete(name = "",
                   labels = c("All CpGs", "High", "Moderate", "Low")) + #Modiy x-axis
  scale_y_continuous(name = "% CpGs",
                     breaks = seq(0, 100, 10)) + #Modify y-axis
  scale_fill_manual(name = "Genome Feature",
                    values = plotColors,
                    labels = c("Exon UTR",
                               "CDS",
                               "Intron",
                               "Upstream Flanks",
                               "Downstream Flanks",
                               "lncRNA",
                               "TE",
                               "Intergenic")) + #Modify legend
  theme_classic(base_size = 15) #Classic theme with minimum font size specified
ggsave("figures/CpG-feature-overlaps.pdf", width = 11, height = 8.5)
```

<img width="1788" height="1343" alt="Image" src="https://github.com/user-attachments/assets/4d418086-dd3b-4ef5-b9e1-e10055e64138" />

**Figure 1**. Stacked barplot of CpG locations

One important modification to the stacked barplot is that I revised the color scheme to use grayscale instead of...purplescale? In any case, color didn't make sense for the previous iteration. I think using color only for the DML plots or any other plots that need treatment information coded in will make more sense.

### Going forward

1. Continue parameter testing for DML identification
2. Revise `methylKit` methods and results
4. `methylKIt` randomization test
1. ATAC-Seq data integration
2. `KOG-MWU` for *Crassostrea* methylation comparison
5. Revise discussion
7. Revise introduction
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
