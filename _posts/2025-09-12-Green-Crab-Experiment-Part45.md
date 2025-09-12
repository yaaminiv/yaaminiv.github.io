---
layout: post
comments: true
title: Green Crab Experiment Part 45
tags: green-crab metabolomics lipidomics perMANOVA
---

## Adding dispersion analysis to perMANOVA

One of the recommendations Ariana and I included in [our perspective piece](https://doi.org/10.1093/icb/icaf138) was to pair perMANOVA analyses with dispersion analyses to understand if significant differences are due to centroid or variance differences. Obviously I can't make that recommendation without doing it myself........so here's to do it myself!

The purpose of the analysis is to determine if any significant effects from the perMANOVA are due to differences in centroids between groups, or differences in spread. I've done this analysis before, so I found a helpful `perMANOVA` tag in this lab notebook index and got directed to [this post where I did a dispersion analysis for MethCompare](https://yaaminiv.github.io/MethCompare-Part21/). Thanks, notebook! I also found [this helpful link](https://uw.pressbooks.pub/appliedmultivariatestatistics/chapter/permdisp/) I used to refresh my memory on the analysis.

### Metabolomics

The first thing I did in [the metabolomics code](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/05-metabolomics-analysis.Rmd) was try and run a PCA. Turns out various package updates now necessitate loading `ggfortify` before trying to run `autoplot` with a `prcomp` object. So, I explicitly load this package now. Another minor change to the code was to the `adonis2` command. I now have to specify `by = terms` for the test to evalute the significance of each term, and not just the full model:

```
permanova.metab <- adonis2(dissim.metab ~ as.factor(treatment)*as.numeric(day), data = transMetabData, method = 'eu', by = "terms") #Conduct global PERMANOVA to assess significance of trends in PCA. Scale metabolomics data and assess influence of treatment, day, and treatment x day on metabolite profiles. Include "by = "terms"" to ensure test assess significance for each term.
permanova.metab #Significant influence of all factors
```

After confirming I could run a PCA and perMANOVA, I needed to run a beta dispersion test. I ran a test for each specific variable that was found to be significant in the perMANOVA. At first, I tried running the code using a scaled transformed matrix, but quickly learned I needed a dissimilarity matrix. I was making one in `adonis2` using the scaled matrix, so I just used a specific command to do this and make my life easier:

```
dissim.metab <- vegdist(scale(transMetabData[c(5:601)]), "euclidean") #Create euclidean dissimilarity matrix to use for perMANOVA and PERMDISP
```

Once I had my dissimilarity matrix, I could run the dispersion test:

```
disp.metab.temp <- betadisper(dissim.metab, group = as.factor(transMetabData$treatment), type = 'centroid') #Run a beta dispersion model to assess if significant differences are due to differences in group centroid or variance
plot(disp.metab.temp) #Dispersion differences exist
boxplot(disp.metab.temp) #Show distance to group centroids, can clearly see dispersion differences between 30 and the other two
anova(disp.metab.temp) #Variance is different between groups. Temperature significance is due to centroid and dispersion differences!
```

```
disp.metab.day <- betadisper(dissim.metab, group = as.factor(transMetabData$day), type = 'centroid') #Run a beta dispersion model to assess if significant differences are due to differences in group centroid or variance
plot(disp.metab.day) #Similar dispersion, but some clear outliers for day 22
boxplot(disp.metab.day) #Similar dispersion differences, outliers seen more easily
anova(disp.metab.day) #Variance is the same between groups. Day significance is due to centroiddifferences!
```

```
disp.metab.tempDay <- betadisper(dissim.metab, group = as.factor(transMetabData$treatment_day), type = 'centroid') #Run a beta dispersion model to assess if significant differences are due to differences in group centroid or variance
plot(disp.metab.tempDay) #Dispersion differences exist, especially with 30_22
boxplot(disp.metab.tempDay) #Very clear dispersion differences for each unique treatment!
anova(disp.metab.tempDay) #Variance is different between groups. Interaction significance is due to centroid and dispersion differences!
```

Day was significant due to centroid differences alone, while the dispersion test revealed differences in dispersion between temperatures and temperature x day treatments. The output is saved [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/PCA/all-data-PERMDISP-results.csv).

### Lipidomics

I did the same thing [with the lipidomics code](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/06-lipidomics-analysis.Rmd). Of course, the lipids were more of a pain. Turns out past Yaamini did something weird and overwrote the temperature data column, so when I went to do the perMANOVA and included `by = terms`, I still couldn't get output that tested differences by each variable. I then went back into my metadata modifying code to ensure I had a temperature column, day column, and interaction column:

```
lipidomicsMetadata <- read.csv("../../data/metabolomics_metadata.csv", header = FALSE, strip.white = TRUE) %>% 
  dplyr::rename_with(~stringr::str_replace_all(., "V", "")) %>%
  t(.) %>% as.data.frame(.) %>% 
  purrr::set_names(as.character(dplyr::slice(., 1))) %>%
  dplyr::slice(., -1) %>%
  dplyr::rename(., temperature = treatment) %>%
  mutate(., unite(., "treatment", temperature:day, sep = "_")) %>%
  mutate(., treatment = gsub(x = treatment, pattern = "_  3", replacement = "_3")) %>%
  mutate(., treatment = gsub(x = treatment, pattern = "_ 22", replacement = "_22")) %>%
  mutate(., treatment = gsub(x = treatment, pattern = " 13_", replacement = "13_")) %>%
  mutate(., treatment = gsub(x = treatment, pattern = " 30_", replacement = "30_")) %>%
  mutate(., treatment = gsub(x = treatment, pattern = "  5_", replacement = "5_")) %>% 
  mutate(., treatment = gsub(x = treatment, pattern = " ", replacement = "")) %>%
  mutate(., day = gsub(x = day, pattern = " ", replacement = "")) #Import metadata. Do not include any header information. Remove "V" from column names. Transform so crab.ID, treatment, and day are all columns. Rename treatment to temperature. Set the first row in the dataframe as column names, then remove that row. Transform characters in each column for correct formatting
```

Once I had that sorted out, I re-ran the perMANOVA and confirmed that the main effect of temperature was the only significant variable. I then ran a beta dispersion test for this variable only:

```
disp.lipid.temp <- betadisper(dissim.lipid, group = as.factor(transLipidData$temperature), type = 'centroid') #Run a beta dispersion model to assess if significant differences are due to differences in group centroid or variance
plot(disp.lipid.temp) #Dispersion differences exist
boxplot(disp.lipid.temp) #Show distance to group centroids, can clearly see dispersion differences between 30 and the other two
anova(disp.lipid.temp) #Variance is different between groups. Temperature significance is due to centroid and dispersion differences!
```

Similar to the metabolomic data, there were significant centroid and dispersion differences for temperature. I think this is because day 30 crabs had much more variable profiles than their counterparts in the control or 5ºC treatments. The output can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/06-lipidomics-analysis/PCA/all-data-PERMDISP-results.csv).

### Going forward

1. Integrate physiology and -omics data with WCNA
3. Metabolomics and lipidomics DIABLO analysis
4. Outline discussion
5. Write discussion
5. Write abstract

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
