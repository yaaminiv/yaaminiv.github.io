---
layout: post
comments: true
title: Green Crab Experiment Part 32
tags: green-crab metabolomics mixOmics
---

## Reworking lipidomics analysis

It's time to finally tackle the lipidomics data! Now that I had a [method established with the metabolomics data](https://yaaminiv.github.io/Green-Crab-Experiment-Part30/), I felt better about trying to piece the harder dataset together in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/06-lipidomics-analysis.Rmd).

### Formatting data

To start, I knew I needed to reformat the data. I emailed the UC Davis West Coast Metabolomics Center to ask about their lipid naming conventions. Scott sent me [this link](https://systemsomicslab.github.io/compms/msdial/lipidnomenclature.html) on MS-DIAL lipidomics naming conventions. It was helpful to read through and understand which codes corresponded to which lipid classes! I also asked about duplicate entries in my dataset, since there were a few lipids with the same name, but different abundances:

> I have confirmed with our staff that in this case, the entries are the same lipid in different adduct forms (M+Na and M+K). Normally our staff would combine these into one annotation, but occasionally adducts slip through our curation procedures unnoticed. You can simply take the sum of the values and combine the adducts into one annotation.

Now I had a plan to handle duplicates: I need to add them together! To do this, I used my new favorite function, `summarize_all`:

```
transLipidData <- rawLipidomicsData %>%
  mutate(lipid = case_when(lipid == "" ~ identifier,
                           lipid != "" ~ lipid)) %>%
  dplyr::select(-c(identifier, Ion.species:ESI.mode)) %>%
  group_by(lipid) %>%
  summarise(across(where(is.numeric), ~ sum(.x, na.rm = TRUE))) %>%
  t(.) %>% as.data.frame(.) %>%
  purrr::set_names(as.character(dplyr::slice(., 1))) %>%
  dplyr::slice(., -1) %>%
  rownames_to_column(., var = "crab.ID") %>%
  left_join(lipidomicsMetadata, ., by = "crab.ID") #Take raw data and modify lipid column such that if there is no lipid name, the identifier is used instead. Add lipid abundance across numeric columns to deal with any duplicate entries (same lipid detected in different adduct forms). Transpose data, convert the first row (lipid) to column names, and remove this row. Convert rownames to crab ID column. Join with lipidomics metadata.
transLipidData[, c(5:2407)] <- transLipidData[, c(5:2407)] %>% mutate_if(is.character, as.numeric) #Convert data columns to numeric
```

. By using the lipid and unique identifier information from the sequencing facility, I circumvented the need to create my own unique identifier. I still created a dataframe with every identifier possible to use in my script:

```
lipidIdentifiers <- rawLipidomicsData %>%
  right_join(inChiKeyNamePubmed, ., by = "INCHIKEY") %>%
  arrange(., "PubChem.ID") %>%
  dplyr::select(4:5, 3:1) #Save dataframe with all possible identifiers for downstream analysis
```

### PCA and perMANOVA

The next step was to redo the PCA and PLS-DA with the newly formatted data. I log-transformed the data for the PCA, being sure to convert all `-Inf` and `NA` to 0. I used [`mutate_if` to convert the `-Inf` to `NA`](https://stackoverflow.com/questions/12188509/cleaning-inf-values-from-an-r-dataframe), then `replace` to convert `NA` to 0.

```
scaled.pca <- prcomp(transLipidData[c(5:2407)] %>%
                       log() %>%
                       mutate_if(is.numeric, list(~na_if(., -Inf))) %>%
                       replace(is.na(.), 0),
                     scale = FALSE, center = TRUE) #Use prcomp to perform a centered and scaled PCA calculation. Replace all -Inf with NA, and then with 0 in the dataframe and log transform prior to performing the PCA calculation
```

When I plotted the PCA, I saw a clearer treatment separation than a separation by day:

![Screenshot 2024-09-04 at 2 44 41 PM](https://github.com/user-attachments/assets/f0eb6255-8c0b-49f5-a1d8-53524ab39ad3)

**Figure 1**. PCA of lipidomics data colored by treatment or day

This was confirmed by a perMANOVA:

```
permanova.lipid <- adonis2(scale((transLipidData[c(5:2407)] %>%
                                    log() %>%
                                    mutate_if(is.numeric, list(~na_if(., -Inf))) %>%
                                    replace(is.na(.), 0))) ~ treatment*day, data = transLipidData, method = 'eu') #Conduct global PERMANOVA to assess significance of trends in PCA. Scale lipidomics data and assess influence of treatment, day, and treatment x day on metabolite profiles
```

> Permutation test for adonis under reduced model
Terms added sequentially (first to last)
Permutation: free
Number of permutations: 999
adonis2(formula = scale((transLipidData[c(5:2407)] %>% log() %>% mutate_if(is.numeric, list(~na_if(., -Inf))) %>% replace(is.na(.), 0))) ~ treatment * day, data = transLipidData, method = "eu")
              Df SumOfSqs      R2      F Pr(>F)    
treatment      2    21062 0.12890 4.9948  0.001 ***
day            1     4108 0.02514 1.9483  0.054 .  
treatment:day  2     5402 0.03306 1.2811  0.178    
Residual      63   132831 0.81290                  
Total         68   163404 1.00000                  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

There was a significant effect of treatment on lipid profiles, but a marginal effect of day on lipid profiles. There was no impact of an interaction on lipid profiles. Based on this information, I decided to proceed with temperature treatment as the only effect in subsequent analyses.

### PLS-DA for all and known lipids

The next step as a PLS-DA using only temperature for all lipid data:

```
X <- (transLipidData[c(5:2407)] %>%
        log() %>%
        mutate_if(is.numeric, list(~na_if(., -Inf))) %>%
        replace(is.na(.), 0)) #Assign only metabolite data to the X dataframe after log transformation.
Y <- as.factor(transLipidData$treatment) #Assign treatment data to the Y dataframe

treatment.plsda <- plsda(X,Y, ncomp = 2) #ncomp = # treatments - 1
```

![Screenshot 2024-09-04 at 3 17 32 PM](https://github.com/user-attachments/assets/0c4d96cf-7bb1-422b-b90a-acf61426a494)

![Screenshot 2024-09-04 at 3 18 52 PM](https://github.com/user-attachments/assets/c7c2df8f-b70d-4aab-9e90-a484e1078331)

**Figures 2-3**. PLS-DA and CIM plot for all lipids

The separation between 30ºC and the other two treatments is really clear in the PLS-DA with all features. The row clustering for the CIM plot has all of the 30ºC samples together (with a 5ºC or 13ºC interloper here and here), while the rest of the rows are an even mix of the other two treatments. This makes me feel comfortable in my decision to only look at temperature, and not temperature and day effects.

I then repeated the PLS-DA with known features only. I first created a new dataframe only for known lipids. My `lipidIdentifiers` dataframe is arranged by PubChem ID. If a lipid does not have a PubChem ID, I do not consider it a known lipid, even if there is a lipid name with structural information:

```
knownTransLipidData <- transLipidData %>%
  dplyr::select(1:4, which(colnames(transLipidData) %in% lipidIdentifiers$lipid[1:418])) #Take transposed lipid data and select columns associated with known lipids.
```

Then I ran a PLS-DA with the known lipid data:

```{r}
X <- log(knownTransLipidData[-c(1:4)]) #Assign known metabolite data to the X dataframe after log transformation
Y <- as.factor(knownTransLipidData$treatment) #Assign treatment data to the Y dataframe

treatment.known.plsda <- plsda(X,Y, ncomp = 3) #ncomp = # treatments - 1
```

![Screenshot 2024-09-04 at 3 22 15 PM](https://github.com/user-attachments/assets/4c8bf686-9a77-4295-9d9c-6f67fa869b57)

![Screenshot 2024-09-04 at 3 22 35 PM](https://github.com/user-attachments/assets/53cd3cdf-0a55-496a-9278-9bd3d10867f7)

![Screenshot 2024-09-04 at 3 24 04 PM](https://github.com/user-attachments/assets/9b567aba-2c3c-40a5-944e-4b483b9678cf)

**Figures 4-6**. PLS-DA and CIM plot for known lipids

Similar to the PLS-DA with all features, it's clear that there's a big separation between 30ºC and the other two treatments. I think the separation between 5ºC and 13ºC is clearer in the plot with components 2 and 3.

### Overall and pairwise VIP

I extracted overall VIP (score ≥ 1) and saved the table [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/06-lipidomics-analysis/PLSDA/known-lipids-VIP-treatment.csv). There are 188 overall VIP lipids.

![Screenshot 2024-09-04 at 3 28 48 PM](https://github.com/user-attachments/assets/24cfe15b-0c02-4edc-ace6-ec5a3f3dc2e2)

**Figure 7**. Cumulative VIP score for overall VIP lipids

I then went through to identify pairwise VIP between 13ºC and 30ºC, or 13ºC and 5ºC:

```
list <- c("13", "30") #Assign treatments to examine

X <- knownTransLipidData %>%
  filter(., treatment %in% list) %>%
  droplevels() #Filter knownTransLipidData for desired contrasts
Y <- as.factor(X$treatment) #Treatments for Y
X <- X[, 5:419] #Retain data only

known.plsda_13v30 <- plsda(X, Y, ncomp = 3) #Run PLSDA for desired contrast
VIP_13v30 <- as.data.frame(PLSDA.VIP(known.plsda_13v30)[["tab"]]) %>%
  rownames_to_column(., var = "lipid") %>%
  filter(., VIP >= 1) #Extract VIP list and filter for VIP > 1

clean_13v30 <- knownTransLipidData %>%
  dplyr::filter(., treatment %in% list) %>%
  droplevels() #Filter knownTransLipidData for desired contrasts.
VIP_select_13v30 <- cbind(clean_13v30[, 1:4],
                          (log(clean_13v30[, names(clean_13v30) %in% VIP_13v30$lipid]) %>%
                             mutate_if(is.numeric, list(~na_if(., -Inf))) %>%
                             replace(is.na(.), 0))) %>%
  pivot_longer(., cols = 5:147, values_to = "log_norm_counts", names_to = "lipid") %>%
  group_by(., lipid) #cbind metadata columns (1:4) and log-transformed "clean" data for VIP. Ensure -Inf from log transformation is converted to 0. Pivot data longer and group by lipids.

t.test_13v30 <-do(VIP_select_13v30, tidy(t.test(.$log_norm_counts ~ .$treatment,
                                                alternative = "two.sided",
                                                mu = 0,
                                                var.equal = FALSE,
                                                conf.level = 0.95
))) #Looped t-test for all lipids with VIP > 1
t.test_13v30$p.adj <- p.adjust(t.test_13v30$p.value, method = c("fdr"), n = length(VIP_13v30$lipid)) #Adjust p value for the number of comparisons
t.test_13v30
```

When comparing 13ºC and 30ºC, there were 97 pairwise VIP with higher abundances in 13ºC, and 33 pairwise VIP with higher abundances in 30ºC. There were 61 pairwise VIP between 13ºC and 5ºC, with 55 having higher abundance in 13ºC and six having higher abundance in 5ºC. I then plotted the pairwise VIP scores:

```
t.test_13v5 %>%
  ungroup(.) %>%
  filter(., p.adj < 0.05) %>%
  left_join(., VIP_13v5, by = "lipid") %>%
  mutate(., direction = case_when(estimate > 0 ~ "13",
                                  estimate < 0 ~ "5")) %>%
  mutate(., score = case_when(estimate > 0 ~ VIP,
                              estimate < 0 ~ -VIP)) %>%
  arrange(score) %>%
  ggplot(aes(x = score, y = reorder(lipid, score, sum))) +
  geom_bar(aes(fill = direction), stat = 'identity') +
  xlab("VIP Score") + ylab("Lipid") +
  scale_fill_manual(values = c(plotColors[2], plotColors[3]),
                    name = "Temperature (ºC)",
                    breaks = c("13", "5"),
                    labels = c("13", "5")) +
  theme_classic(base_size = 15) #Take t-test results and filter out significant lipids. Create two new columns using estimate values: one for the direction of increase, and another for positive or negative VIP. Arrange by identifier name and reorder by cumulative VIP score. Plot bars and change x and y axis labels. Add scale information.
```

![Screenshot 2024-09-04 at 3 32 36 PM](https://github.com/user-attachments/assets/f973f47b-051b-434a-a060-f3c495f3373e)

![Screenshot 2024-09-04 at 3 34 24 PM](https://github.com/user-attachments/assets/1924b977-882b-46a6-a5f4-6a1774698203)

**Figures 8-9**. Pairwise VIP scores

When going through this code, I realized I had estimate > or < 1 instead of 0! This lead to an incorrect visualization. I fixed this code and the corresponding code in the [metabolomics code](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/05-metabolomics-analysis.Rmd). The updated metabolomics figure can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/PLSDA/figures).

### PLS-DA validation

Finally, I needed to validate my PLS-DA with a sPLS-DA. This step is a sanity check for:

1) ensuring I didn't use too many components or variables in my analysis
2) ensuring the patterns in the sPLS-DA are similar to the patters in the PLS-DA

Things looked good here, so that means it's time to move onto the WCNA.

### Going forward

1. Repeat lipidomics WCNA with temperature only
2. Lipidomics pathway enrichment
7. Try lipid-specific enrichment with LION
1. Update lipidomics methods
2. Update lipidomics results
8. Determine if I can add physiology data to `mixOmics` analyses
9. Integrate metabolomics and lipidomics data

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
