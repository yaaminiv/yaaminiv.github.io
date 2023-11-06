---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 25
tags: green-crab-wc sequencher genotype
---

## Genotyping respirometry crabs

I got my Sanger sequencing data back! Before I sent out my samples, Carolyn showed me how to use Sequencher to clean up the data and call genotypes. I followed her protocol to get genotype data for 25/27 respirometry crabs.

### Methods

1. Open Sequencher. Create a New Project and add Sequences (`.abi` files, with chromatograms)
2. Double click the sequences for the forward strand of a sample, then click "show chromatogram."
3. With the chromatogram showing, remove the sequence information for poor quality chromatograms. Repeat for the reverse strand.
4. After cleaning the chromatograms, select both files, then click "Assemble Automatically." This will create a new contig. Rename that contig with the sample name.
5. Double click on the newly assembled contig, then click "Bases." This will bring you to a view of the forward and reverse sequences in blue colors on top, with the reference sequence on the bottom. There will be dots indicate disagreements.
6. Click "Show Chromatograms." To fix disagreements manually, click on the dot under the sequences in blue lettering. This will bring you to the place in the chromatograms with the disagreement. At each disagreement, evaluate if a nucleotide needs to be added, deleted, or changed. After fixing each disagreement, click "ReAligner" to ensure proper alignment at each step.
7. Repeat for all sequences.
8. Once all sequences have been cleaned and assembled into individual contigs, highlight all contigs and click "Assemble Automatically." This will create a "master contig". Rename with the genotyping date. Include reads that were not assembled into contigs in the event that one strand was poorer quality than another. The higher quality strands will be assembled while the lower quality strads will not be included in the master contig.
9. Once the master contig is assembled, rearrange the F and R strands for each sample so they are on top of eachother. The default order once assembled is to group all R together and all F strands together, which is not conducive for genotyping individual samples. Click "Show Chromatograms" and confirm that the chromatograms are in the correct order.
10. In the "Bases" view, click on the nucleotide with the ambiguity. This will bring all chromatograms to the locus used to genotype samples. Sometimes, sequence quality will show to loci with ambiguities, other times just one.
11. Go through chromatograms for each sample and confirm that the bases are correctly called at the locus with the ambiguity. Record the genotype at this locus. If sequence quality allows for genotype to be called at two loci, the genotypes at these loci should match.

### Notes

- 5-005: No readable sequence for F strand, unable to assemble contig
- 25-052: Garbage F read, unable to assemble contig
- 15-097: Poor qualilty F and R reads but still able clean data and assemble contig
- Assembled master contig with all individual sample contigs and R reads from 5-005 and 25-052
- Only had genotype data at one locus. Unsure where the second locus of data is.

### Results

**Table 1**. Number of CC, CT, and TT genotypes per treatment and tank

| **Treatment** | **Tank** | **CC** | **CT** | **TT** |
|:-------------:|:--------:|:------:|:------:|:------:|
|      5ºC      |     1    |    0   |    2   |    1   |
|      5ºC      |     4    |    0   |    3   |    0   |
|      5ºC      |     7    |    0   |    1   |    2   |
|      15ºC     |     2    |    2   |    0   |    0   |
|      15ºC     |     5    |    0   |    1   |    2   |
|      15ºC     |     8    |    1   |    1   |    0   |
|      25ºC     |     3    |    2   |    0   |    1   |
|      25ºC     |     6    |    0   |    1   |    2   |
|      25ºC     |     9    |    0   |    3   |    0   |

I went into [this R Markdown script for demographic data](https://github.com/yaaminiv/wc-green-crab/blob/main/code/02-demographic-data.Rmd) to pull together a few helpful pie charts for the genotype data. Turns out making pie charts in R is really annoying. I was able to put together an overall genotype pie chart easily:

```{r}
genotypeData %>%
  group_by(., final.genotype) %>%
  summarise(., count = n()) %>%
  ggplot(., aes(x= "", y = count, fill = final.genotype)) +
  geom_bar(stat = "identity", width = 1) +
  coord_polar("y", start = 0) +
  geom_text(aes(label = paste0(round((count/sum(count)*100), digits = 0), "%")), position = position_stack(vjust = 0.5), size = 6) +
  scale_fill_manual(name = "Genotype",
                    values = c("#BAE4B3", "#74C476", "#006D2C")) +
  theme_void(base_size = 15) #Group genotype data by genotype, then count the number of crabs in each genotype. Create a pie chart for genotype distribution, irrespective of treatment by using geom_bar then setting polar coordinates. Add text labels for % crabs in each genotype. Create a manual color scale.
ggsave("figures/genotype.pdf", height = 8.5, width = 11)
```

<img width="743" alt="Screenshot 2023-11-06 at 3 32 21 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/28d14d97-99ea-40c4-b324-c071d05cf058">

**Figure 1**. Distribution of genotypes for all crabs

I tried to do something similar and facet by treatment so I could get the percent of crabs for each genotype within a treatment. However, when I tried to do that, either the labels were completely off OR the pies themselves would shrink until they matched whatever pie fraction was present in the first pie chart. Obviously I spent way too much time on Stack Overflow trying to figure this out before giving up because it wasn't important.

```{r}
facet_labels <- c("15C" = "15ºC",
                  "25C" = "25ºC",
                  "5C" = "5ºC") #Assign labels for facets
```

```{r}
genotypeData %>%
  group_by(., treatment, final.genotype) %>%
  summarise(., count = n()) %>%
  ggplot(., aes(x= "", y = count, fill = final.genotype)) +
  geom_bar(stat = "identity", width = 1, position = "fill") +
  coord_polar("y", start = 0) +
  facet_wrap(vars(treatment), ncol = 3, labeller = labeller(treatment = facet_labels)) +
  scale_fill_manual(name = "Genotype",
                    values = c("#BAE4B3", "#74C476", "#006D2C")) +
  theme_void(base_size = 15)  + theme(strip.text.x = element_text(size = 15)) #Group genotype data by genotype, then count the number of crabs in each genotype. Create a pie chart for genotype distribution, irrespective of treatment by using geom_bar then setting polar coordinates. Create a manual color scale and adjust text of facet labels.
ggsave("figures/genotype-by-treatment.pdf", height = 8.5, width = 11)
```

<img width="868" alt="Screenshot 2023-11-06 at 3 32 40 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/9b550077-fe44-4e41-acc9-41d744aac26d">

**Figure 2**. Distribution of genotypes within each treatment

Looks like I have a few crabs per genotype in each treatment for my respirometry crabs, with the exception of missing CC from the 5ºC treatment. I need to confirm which genoytpe is connected to warm tolerance vs. cold tolerance.

Now that I have this data, I can incorporate it into my TTR and respirometry analyses for the subset of crabs that were used for both!

### Going forward

1. Incorporate genotype into TTR and respirometry analyses
2. Prepare talk for PICES
3. Q10 analysis for 2023 experiment
4. Finish extracting respirometry samples
5. Continue with Chelex extractions, PCRs, and gels for TTR crabs
6. Update methods and results of 2022 paper
7. Examine HOBO data from 2023 experiment
8. Demographic data analysis for 2023 paper
9. Update methods and results of 2023 paper
10. Revisit Julia's genotypes

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
