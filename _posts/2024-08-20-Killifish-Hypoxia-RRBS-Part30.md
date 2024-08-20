---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 30
tags: killifish RRBS
---

## DMR figures

It's everyone's favorite form of productive procrastination: figure making! I worked in [this R Markdown script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/08-annotate-DMR.Rmd) making various plots for the DMR data.

### Heatmaps

The first thing I wanted to do was make heatmaps for the DMR! To do this, I needed a methylation value for the DMR for each sample. Easier said than done. The first thing I did was comb through my various output files and the `BAT` manual to determine what the most useful file was. Unfortunately `BAT` doesn't spit out a file with percent methylation for each sample in a DMR. The closest thing I got were files used by `meteliene` to create DMR. So, I imported those into R and made a series of modifications (which involved me writing a `for` loop in R for the first time in years SUCCESSFULLY on the first try)! I learned how to use `summarize_all` from [this Stack Overflow post](https://stackoverflow.com/questions/25759891/dplyr-summarise-each-with-na-rm).

```
NBH20vOCmetilene <- read.delim("../../05-analysis/new-genome/summarize/missing_1/20_OC_N/N_metilene_NO_OC.txt", header = TRUE) #Import metilene data used to identify DMR. This dataset has the methylation value for each CpG in the DMR.

NBH20vOCpositions <- data.frame() #Create empty dataframe
for (i in 1:nrow(NBH20vOCDMR)) {
  temp <- NBH20vOCmetilene %>%
    filter(., pos >= NBH20vOCDMR$DMR.start[i] & pos <= NBH20vOCDMR$DMR.end[i]) %>%
    mutate(., DMR = NBH20vOCDMR$DMR[i])
  NBH20vOCpositions <- rbind(NBH20vOCpositions, temp)
} #Use a for loop to filter CpG data for each DMR. Filter positions based on start and end positions of each DMR. Add a column for DMR identity. Save as a temporary dataframe. Use rbind to append information to empty dataframe
colnames(NBH20vOCpositions) <- c(colnames(NBH20vOCmetilene), "DMR") #Assign column names based on previous dataframe and add name for the DMR column

NBH20vOCavgMeth <- NBH20vOCpositions %>%
  group_by(., DMR) %>%
  dplyr::select(., -c(chr, pos)) %>%
  summarize_all(funs(sum(., na.rm = TRUE))) %>%
  slice(., 1, 3:10, 2) %>%
  column_to_rownames(var = "DMR") #Group by DMR. Remove chromosome and position columns. Average methylation level across all CpG positions in each DMR for each sample. Reorganize manually with slice. Change DMR column to rownames
```

With average methylation within a sample for each DMR, I could then make heatmaps with `pheatmap`.

<img width="1140" alt="Screenshot 2024-08-20 at 1 40 08 PM" src="https://github.com/user-attachments/assets/dfe33b7f-8826-4977-9383-a976d4c3b29e">

<img width="1140" alt="Screenshot 2024-08-20 at 1 40 17 PM" src="https://github.com/user-attachments/assets/8d390057-eefa-4f74-90ce-bf4a16af294e">

**Figures 1-2**. Heatmap of average methylation across samples for normoxia vs. outside control DMR and hypoxia vs. outside control DMR.

These heatmaps.........are not very convincing. It's clear that there's some within-treatment variation that may be be obscuring trends. Interestingly, there is a strong hypermethylation trend for both sets of DMR.

### Hyper- and hypomethylation plots

Neel made a plot visualizing how hyper- or hypomethylated DMR were. I figured I'd make a very similar plot, but have a multipanel plot with DMR from both contrasts. The trickest thing about this plot was getting the numbering to go in an alphanumeric order (ie. DMR 1, DMR 2, etc. and not DMR 1, DMR 10, DMR 2, etc.). To solve this, I created a new column with DMR number, reordered it, and then converted it to a factor for plotting (h/t this [Stack Overflow post](https://stackoverflow.com/questions/49763863/r-sorting-a-data-frame-by-an-alphanumeric)).

```
NBH20vOCDMR %>%
  mutate(order = as.numeric(gsub(x = DMR, pattern = "DMR_", replacement = ""))) %>%
  arrange(order) %>%
  ggplot(aes(y = factor(order), x = meth.diff)) +
  geom_bar(stat = "identity") +
  scale_x_continuous(name = "Mean Methylation Difference",
                     breaks = seq(-1.0, 1.0, 0.25),
                     limits = c(-1,1)) +
  scale_y_discrete(name = "DMR") +
  theme_classic(base_size = 15)
```

<img width="1140" alt="Screenshot 2024-08-20 at 1 54 56 PM" src="https://github.com/user-attachments/assets/1419cf4f-c6aa-4aeb-831b-26d73378a371">

**Figure 3**. Plot showing mean methylation difference for a) normoxia vs. outside control and b) hypoxia vs. outside control DMR

I find it interesting that most DMR are hypermethylated! I need to review my vertebrate DNA methylation literature to think more about what this might mean.

### `BAT_correlating` plots

I initially thought I would try and make plots akin to the output that `BAT_correlating` is *supposed* to give you (since [I didn't get usable pdf output](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part29/)). Through some Google searches, I learned that these kinds of plots are called scatterplots with marginal boxplots. There are two different ways to make these plots:

- [`scatterplot` command from the `car` package](https://stackoverflow.com/questions/53767057/r-ggplot-how-to-create-a-scatter-plot-with-marginal-box-plots): I tried using this but the plot is just really ugly
- [`ggMarginal` command from the `ggExtra` package](https://www.rdocumentation.org/packages/ggExtra/versions/0.10.1/topics/ggMarginal): This package let me add onto a scatterplot I made in `ggplot2`, but it was still ugly

Ugliness aside, since I had three-four points of data for each treatment, I didn't find that the marginal boxplots actually added any extra information that I couldn't glean from the scatterplot. So I scraped the fancy plot idea and focused on correlation plots.

Before making the plots, I needed to do even more data wrangling! I already had the average methylation values for each DMR, but I needed the expression data across samples for each gene of interest. For each treatment, I imported the data into an `rbind` command and modified the dataframes as needed:

```
NBH20geneExp <- rbind((read.delim("../../../data/RNA-Seq-renamed/htseq-20-N1.txt", header = FALSE,
                                  col.names = c("gene.name", "CPM")) %>%
                         mutate(., sample = "20-N1_exp")),
                      (read.delim("../../../data/RNA-Seq-renamed/htseq-20-N2.txt", header = FALSE,
                                  col.names = c("gene.name", "CPM")) %>%
                         mutate(., sample = "20-N2_exp")),
                      (read.delim("../../../data/RNA-Seq-renamed/htseq-20-N4.txt", header = FALSE,
                                  col.names = c("gene.name", "CPM")) %>%
                         mutate(., sample = "20-N4_exp"))) %>%
  pivot_wider(., names_from = "sample", values_from = "CPM") #Import gene expression data and rename columns. Add a new column for sample ID. Use rbind to combine dataframes. Pivot wider.
```

I then imported and formatted the correlation output from `BAT_correlating`:

```
NBH20vOCBATcorrelating <- read.delim("BAT_correlating/20_OC_N/correlation_output/20_OC_N_correlation.txt", sep = "\t", header = TRUE, skip = 1, na.strings = c("NaN", "NA")) %>%
  dplyr::select(-1) %>%
  filter(is.na(rho.p.val) == FALSE) #Import output from BAT_correlating. Skip the first line to allow column names to be recognized. Input specific NA strings. Remove the first column. Keep rows that have correlation p-values
colnames(NBH20vOCBATcorrelating)[1] <- "gene.name" #Rename column with gene ID
NBH20vOCBATcorrelating <- NBH20vOCBATcorrelating %>%
  left_join(NBH20vOCDMR, ., by = "gene.name") %>%
  dplyr::select(-methylation_region) #Join correlating output with DMR dataframe
```

Finally, I joined the correlation output with methylation and gene expression data, did a series of pivots, and formatted as needed. The highlight here was learning how to use [`rename_all`](https://stackoverflow.com/questions/29948876/adding-prefix-or-suffix-to-most-data-frame-variable-names-in-piped-r-workflow):

```
NBH20vOCcorrOnly <- NBH20vOCBATcorrelating %>%
  filter(., is.na(rho.p.val) == FALSE) %>%
  left_join(., (NBH20vOCavgMeth %>%
                  rename_with(., function(x) paste0(x, sep = "_meth")) %>%
                  rownames_to_column(., var = "DMR")), by = "DMR") %>%
  left_join(., NBH20geneExp, by = "gene.name") %>%
  left_join(., NBHOCgeneExp, by = "gene.name") %>%
  dplyr::select(-c(chr, DMR.start, DMR.end, direction, gene.start, gene.end, strands, "OC-N3_exp")) %>%
  pivot_longer(cols = "NO_20.N4_meth":"OC_OC.N4_meth", names_to = "sample_meth", values_to = "meth") %>%
  pivot_longer(cols = "20-N1_exp":"OC-N5_exp", names_to = "sample_exp", values_to = "exp") %>%
  mutate(sample_meth = gsub(x = sample_meth, pattern = "_meth", replacement = "")) %>%
  mutate(sample_meth = gsub(x = sample_meth, pattern = "NO_20.", replacement = "20-")) %>%
  mutate(sample_meth = gsub(x = sample_meth, pattern = "OC_OC.", replacement = "OC-")) %>%
  mutate(sample_exp = gsub(x = sample_exp, pattern = "_exp", replacement = "")) %>%
  filter(., sample_meth == sample_exp) %>%
  mutate(sample = sample_meth) %>%
  separate(., col = sample_meth, into = c("treatment", "extra"), sep = "-") %>%
  dplyr::select(-c(sample_exp, extra))
#Take BAT_correlating output and retain rows that have a correlation p-value. Join with average methylation data where the columns are renamed to indicate methylation values. Join with gene expression data. Remove unecessary columns. Pivot longer for methylation and expression data separately. Modify column contents and retain rows where the sample ID for methylation and expression match. Create a new sample ID column. Create a new column for treatment specifications by separating defunct sample_meth column. Remove extra columns
```

Initially, I thought that the comparisons without a rho or *P*-value were comparisons for non-overlapping DMR and genes. Looking at the wrangled data, I realized that was incorrect! Some of my significant correlations were from genes that did not overlap with DMR. I decided to save the correlation output with additional annotations for future reference.

- [normoxia vs. outside control](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/08-annotate-DMR/new-genome/BAT_correlating/20_OC_N/correlation_output/20_OC_N_correlation-withAnnot.txt)
- [hypoxia vs. outside control](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/08-annotate-DMR/new-genome/BAT_correlating/5_OC_N/correlation_output/5_OC_N_correlation-withAnnot.txt)

Now that my data was formatted, I could actually create plots! I created [text objects](https://stackoverflow.com/questions/11889625/annotating-text-on-individual-facet-in-ggplot2) for the annotations I wanted to use for each facet (rho and *P*-values) so I could add them onto my plots:

```
NBH20vOCcorrText <- data.frame(gene.name = unique(NBH20vOCcorrOnly$gene.name),
                               label = c("rho = 0.39, p = 0.40",
                                         "rho = -0.63, p = 0.13",
                                         "rho = 0, p = 1.00")) #Create a geom with rho and p-value information to label facets

NBH20vOCcorrOnly %>%
  ggplot(aes(x = meth, y = exp, color = treatment)) +
  geom_point() +
  facet_wrap(~gene.name, scales = "free") +
  labs(x = "Mean Methylation (%)", y = "Expression (CPM)") +
  scale_color_manual(name = "Treatment",
                     values = c(plotColors[2], plotColors[1]),
                     labels = c("Mild", "Control")) +
  geom_smooth(method = lm, se = FALSE,
              linewidth = 0.75, linetype = 2, color = "black") +
  geom_text(NBH20vOCcorrText, inherit.aes = FALSE,
            mapping = aes(x = -Inf, y = Inf, label = label),
            hjust = -0.05, vjust = 5) +
  theme_classic(base_size= 15) #Create a scatterplot with methylation and expression data. Color by treatment and facet by gene. Relabel x- and y-axes. Add linear regression line and text labels.
```

<img width="1140" alt="Screenshot 2024-08-20 at 1 55 11 PM" src="https://github.com/user-attachments/assets/dbc53c98-415a-4b60-971e-4d181f3c0c7d">

**Figure 4**. Correlation plot for normoxia vs. outside control DMR

I did something similar with the hypoxia vs. outside control contrast. Since there were 59 DMR, I decided to make one multipanel plot with facets for each DMR tested, and another for the three significant correlations.

<img width="1148" alt="Screenshot 2024-08-20 at 1 55 26 PM" src="https://github.com/user-attachments/assets/dd915776-51cf-4b47-8884-50de166d986f">

<img width="1148" alt="Screenshot 2024-08-20 at 1 56 09 PM" src="https://github.com/user-attachments/assets/abefc7f3-a04a-4a76-ac1c-7c70faa1d626">

**Figures 5-6**. Hypoxia vs. outside control DMR plots for all correlations and only significant correlations

### Going forward

1. Create DMR figures
1. Update methods and results
5. Identify known SNP/DMR overlaps
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
