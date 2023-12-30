---
layout: post
comments: true
title: Green Crab Experiment Part 21
tags: green-crab metabolomics mixOmics
---

## Multivariate metabolomics analysis

After many moments pondering how to do this and scrolling through workflows, I've finally analyzing my metabolomics data! Shoutout to Ariana for [her scripts](https://github.com/AHuffmyer/EarlyLifeHistory_Energetics/tree/master/Mcap2020/Scripts/Metabolomics) and answering some questions for me when I was working through them.

The overall workflow I'm using:

1. Conduct an initial PCA with all of the metabolite data. This analysis will tell me if temperature, day, or both temperature and day have any bearing on the metabolite patterns.
2. Run a PLSDA analysis with all of the metabolite data. This is a more robust, supervised multivariate analysis method to understand which metabolites are associated with which treatments.
3. Run a PLSDA analysis with only known metabolites. Within my data, not all of the metabolites were identified! Ariana suggested running the PLSDA with all data to look at overall trends and maybe get a feel for what some of those unknown compounds are doing, but then repeating the analysis with only known metabolites so I can look at pathways.

I ran the analysis in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/05-metabolomics-analysis.Rmd).

### Data transformation

But first, I went down a rabbit hole trying to figure out if I needed to transform my data. It seemed obvious that I needed to, but I wasn't sure to what extent. The West Coast Metabolomics Center has already normalized data using the average mTIC (sum of all peak heights for *identified* metabolites) of the samples. Reviewing Ariana's manuscript and code and then Shelly's manuscript, it seemed like I needed to at least conduct a log transformation for the data prior to the PLSDA analysis.

### PCA

First things first, a PCA. I ran the PCA with all the data and using just treatment, just day, or treatment x day to visualize the separation:

```{}
pcaPlot3 <- autoplot(scaled.pca, data = transMetabData, x = 1, y = 2,
                     size = 4, alpha = 0.8, colour = "treatment", shape = "day",
                     frame = TRUE, frame.colour = "treatment", loadings = FALSE) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)") +
  scale_fill_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                    name = "Temperature (ºC)") +
  scale_shape_manual(values = c(20, 18),
                     name = "Day") +
  theme_classic(base_size = 15) + theme(legend.position = "right",
                                        plot.background = element_blank()) #Use autoplot to plot the prcomp object. Color points and confidence ellipses by treatment, and have different shapes for each day. Do not include loadings. Manually define color, fill, and shapes.
ggsave("PCA/figures/all-data-PCA-day-treatment.pdf", height = 8.5, width = 11)
```

<img width="1191" alt="Screenshot 2023-12-29 at 3 15 23 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/3c23e5f4-5642-4eb9-b1e2-651febcab6f6">

**Figure 1**. PCA with all metabolite data

Looking at the PCA with day and treatment, the largest differentiation is between treatments. It also seems like the metabolite data is less variable at day 3 than day 22. I ran a PERMANOVA using day and treatment:

```
permanova.metab <- adonis2(scale(transMetabData[c(5:601)]) ~ treatment*day, data = transMetabData, method = 'eu') #Conduct global PERMANOVA to assess significance of trends in PCA. Scale metabolomics data and assess influence of treatment, day, and treatment x day on metabolite profiles
```

There was a significant influence on both day and treatment, and their interaction, on the metabolite profiles. The PERMANOVA output is saved [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/PCA/all-data-PERMANOVA-results.csv).

Just for fun, I did a PCA on only the known metabolites! All factors were still significant, so that makes interpretation easier. The output from that analysis can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/PCA/known-metabolites-PERMANOVA-results.csv).

### PLSDA

I returned to my favorite package, [`mixOmics`](https://mixomicsteam.github.io/mixOmics-Vignette/id_05.html) to conduct the PLSDA analysis. They used to have a really nice online book to walk through the analysis, but it seems like that book now has a price. Instead, I used the online vignette and Ariana's code as a guide.

I first ran the PLSDA using all metabolite data from the sequencer using log-transformed metabolite data:

```
X <- log(transMetabData[c(5:601)]) #Assign only metabolite data to the X dataframe after log transformation
Y <- as.factor(transMetabData$treatment_day) #Assign treatment data to the Y dataframe

treatmentday.plsda <- plsda(X,Y, ncomp = 5) #ncomp = # treatments - 1
```

I then plotted the PLSDA results:

```
plotIndiv(treatmentday.plsda, comp = c(1,2), ind.names = FALSE,
          cex = 3, col = c("lightblue", "#2171B5", "grey80", "#525252", "salmon", "#CB181D"), pch = c(12:17), point.lwd = 0.7,
          ellipse = TRUE, star = TRUE, centroid = FALSE,
          legend = TRUE, legend.title = "Temperature (ºC) x Day",
          title = "PLS-DA by Temperature and Day", X.label = "Component 1: 14% Explained Variance", size.xlabel = 15, Y.label = "Component 2: 6% Explained Variance", size.ylabel = 15, size.axis = 13,
          style = "ggplot2") #Visualize first two components of PLS-DA with no individual sample names. Alter point size, color, shape, and line width. Include 95% confidence ellipses and use star format to visualize sample distance from centroids. Include a legend and specify the legend title. Specify the plot title and text label sizes. Use ggplot style.
```

I repeated this process for the known metabolites only:

```
X <- log(transMetabData[c(5:156)]) #Assign known metabolite data to the X dataframe after log transformation
Y <- as.factor(transMetabData$treatment_day) #Assign treatment data to the Y dataframe

treatmentday.known.plsda <- plsda(X,Y, ncomp = 5) #ncomp = # treatments - 1
```

plotIndiv(treatmentday.known.plsda, comp = c(1,2), ind.names = FALSE,
          cex = 3, col = c("lightblue", "#2171B5", "grey80", "#525252", "salmon", "#CB181D"), pch = c(12:17), point.lwd = 0.7,
          ellipse = TRUE, star = TRUE, centroid = FALSE,
          legend = TRUE, legend.title = "Temperature (ºC) x Day",
          title = "PLS-DA by Temperature and Day", X.label = "Component 1: 12% Explained Variance", size.xlabel = 15, Y.label = "Component 2: 12% Explained Variance", size.ylabel = 15, size.axis = 13,
          style = "ggplot2") #Visualize first two components of PLS-DA with no individual sample names. Alter point size, color, shape, and line width. Include 95% confidence ellipses and use star format to visualize sample distance from centroids. Include a legend and specify the legend title. Specify the plot title and text label sizes. Use ggplot style.

<img width="1353" alt="Screenshot 2023-12-29 at 3 19 11 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/140aaca6-c4a5-4762-8bdb-3386a3b5f3fc">

**Figure 2**. PLSDA by treatment and day for known metabolties.

I plotted component 1 versus component 2 and component 3. Looking at both plots, I saw clearer separation by temperature treatment as opposed to day. Interestingly, day 3 data for 5ºC and 13ºC is less variable than day 22 data, but the day 22 data for 30ºC is more variable. These trends are consistent for all metabolites and known metabolites only. It could tie back to the high mortality and variability in respiration data for 30ºC crabs.

### PLSDA validation

An important step with the PLSDA is to validate the model. To do this, I have to tune parameters, then run a sparse PLSDA (sPLSDA). The patterns from the sPLSDA will either confirm or deny the patterns I see in the PLSDA.

First, I ran several permutation tests to determine how many components to include in the PLSDA:

```
MyPerf.plsda <- perf(treatmentday.known.plsda, validation = "Mfold", folds = 6,  
                     progressBar = TRUE, nrepeat = 50)
```

Increasing the number of components decreases the error rate. Next, I tuned the components and number of elements to include in the PLSDA simultaneously:

```
list.keepX <- c(5:10,  seq(20, 300, 10)) #list.keepX # to output the grid of values tested
X <- log(transMetabData[c(5:156)]) #Assign known metabolite data to the X dataframe after log transformation
Y <- as.factor(transMetabData$treatment_day) #Assign treatment data to the Y dataframe

tune.splsda.srbct <- tune.splsda(X, Y, ncomp = 5,
                                 validation = 'Mfold', folds = 6,
                                 measure = "BER", test.keepX = list.keepX, nrepeat = 50) #More extensive classification performance
```

I found that the ideal number of components to include was 5, which is how many I was including to begin with! For 5 components, the sPLSDA should be run with 40 features according to the tuning:

```
myResult.splsda.final_T <- splsda(X, Y, ncomp = ncomp, keepX = select.keepX) #Use the number of components and meatbolites to keep based on optimization from above  
```

Plotting the sPLSDA, I still see separation by treatment with only slight separation by day. The 5ºC and 13ºC treatments have less separation than they do in the PLSDA. Overall I'd consider the PLSDA validated.

### VIP

After plotting and validating the PLSDA, I wanted to understand which metabolites were most responsible for separating the treatments and days — the VIP. I extracted the VIP scores and saved the output [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/PLSDA/known-metabolites-VIP-day-treatment.csv):

```
treatmentday.known.VIP <- as.data.frame(PLSDA.VIP(treatmentday.known.plsda)[["tab"]]) %>%
  rownames_to_column(., var = "metabolite") #Extract VIP information and save as a dataframe. Convert metabolite rownames to a column.
```

I filtered out the VIP with scores greater than 1, since these are the most important metabolites. I then arranged these metabolites by VIP score and plotted them:

```
treatmentday.known.VIP %>%
  filter(VIP >= 1) %>%
  arrange(VIP) %>%
  ggplot(aes(x = VIP, y = reorder(metabolite, VIP, sum))) +
  geom_point() +
  xlab("VIP Score") + ylab("Metabolite") +
  theme_classic() + theme(axis.text.y = element_text(size = 8)) #Using only metabolites with VIP > 1, arrange by metabolite name and reorder by cumulative VIP score. Plot points and change x and y axis labels.
```

<img width="1353" alt="Screenshot 2023-12-29 at 3 19 36 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/3dd60cb2-1400-471c-bc41-4e7cb6678a29">

**Figure 3**. VIP from known metabolite PLSDA.

Even with the filtering threshold, there are 73 metabolites that are VIP! Additionally, just because a metabolite is a VIP, it doesn't tell me anything about that treatment x day combination that VIP is associated with. I followed Ariana's code to conduct pairwise tests that identify which metabolies are important for which groups.

I identified seven pairwise contrasts:

1. Day 3: 13ºC vs. 30ºC
2. Day 3: 13ºC vs. 5ºC
3. Day 22: 13ºC vs. 30ºC
4. Day 22: 13ºC vs. 5ºC
5. 5ºC: Day 3 vs. Day 22
6. 13ºC: Day 3 vs. Day 22
7. 30ºC: Day 3 vs. Day 22

For each contrast, I ran a PLSDA on a subset of data then conducted pairwise t-tests:

```
list <- c("13_22", "30_22") #Assign treatments to examine
```

```
X <- transMetabData %>%
  dplyr::filter(., treatment_day %in% list) %>%
  droplevels() #Filter transMetabData for desired contrasts
Y <- as.factor(X$treatment_day) #Treatments for Y
X <- X[, 5:156] #Retain data only
```

```
known.plsda_13v30_day22 <- plsda(X, Y, ncomp = 5) #Run PLSDA for desired contrast
VIP_13v30_day22 <- as.data.frame(PLSDA.VIP(known.plsda_13v30_day22)[["tab"]]) %>%
  rownames_to_column(., var = "metabolite") %>%
  filter(., VIP >= 1) #Extract VIP list and filter for VIP > 1
```

```
clean_13v30_day22 <- transMetabData %>%
  dplyr::filter(., treatment_day %in% list) %>%
  droplevels() #Filter transMetabData for desired contrasts.
VIP_select_13v30_day22 <- cbind(clean_13v30_day22[, 1:4],
                                log(clean_13v30_day22[, names(clean_13v30_day22) %in% VIP_13v30_day22$metabolite])) %>%
  pivot_longer(., cols = 5:55, values_to = "log_norm_counts", names_to = "metabolite") %>%
  group_by(., metabolite) #cbind metadata columns (1:4) and log-transformed "clean" data for VIP. Pivot data longer and group by metabolites.
```

```
t.test_13v30_day22 <-do(VIP_select_13v30_day22, tidy(t.test(.$log_norm_counts ~ .$treatment_day,
                                                            alternative = "two.sided",
                                                            mu = 0,
                                                            paired = FALSE,
                                                            var.equal = FALSE,
                                                            conf.level = 0.95
))) #Looped t-test for all metabolites with VIP > 1
t.test_13v30_day22$p.adj <- p.adjust(t.test_13v30_day22$p.value, method = c("fdr"), n = length(VIP_13v30_day22$metabolite)) #Adjust p value for the number of comparisons
```

Only three contrasts had VIP with significant p-values.

**Table 1**. Contrasts and number of VIP

| **Condition 1** | **Condition 2** | **Total VIP** | **VIP Higher in Condition 1** | **VIP Higher in Condition 2** |
|:---------------:|:---------------:|:-------------:|:-----------------------------:|:-----------------------------:|
|  13ºC at Day 22 |  30ºC at Day 22 |       29      |               16              |               13              |
|  13ºC at Day 22 |  5ºC at Day 22  |       31      |               29              |               2               |
|   Day 3 at 5ºC  |  Day 22 at 5ºC  |       6       |               0               |               6               |

I plotted the VIP scores for the significant VIP for each contrast:

```
t.test_13v30_day22 %>%
  ungroup(.) %>%
  filter(., p.adj < 0.05) %>%
  left_join(., VIP_13v30_day22, by = "metabolite") %>%
  mutate(., direction = case_when(estimate > 1 ~ "13",
                                  estimate < 1 ~ "30")) %>%
  mutate(., score = case_when(estimate > 1 ~ VIP,
                              estimate < 1 ~ -VIP)) %>%
  arrange(score) %>%
  ggplot(aes(x = score, y = reorder(metabolite, score, sum))) +
  geom_bar(aes(fill = direction), stat = 'identity') +
  xlab("VIP Score") + ylab("Metabolite") +
  scale_x_continuous(limits = c(-2, 2), breaks = seq(-2, 2, 1)) +
  scale_fill_manual(values = c(plotColors[2], plotColors[1]),
                    name = "Temperature (ºC)",
                    breaks = c("13", "30"),
                    labels = c("13", "30")) +
  theme_classic(base_size = 15) + theme() #Take t-test results and filter out significant metabolites. Create two new columns using estimate values: one for the direction of increase, and another for positive or negative VIP. Arrange by metabolite name and reorder by cumulative VIP score. Plot bars and change x and y axis labels. Add scale information.
```

<img width="1353" alt="Screenshot 2023-12-29 at 3 20 18 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/a0ff50e7-df01-454a-8399-07f67a53f2d3">

<img width="1353" alt="Screenshot 2023-12-29 at 3 20 03 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/1cbe4f82-7942-431d-ad2d-518b3f9dc216">

<img width="1353" alt="Screenshot 2023-12-29 at 3 19 50 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/85c5a76a-6f63-40e7-a721-a32afa611001">

**Figures 4-6**. VIP scores for significant pairwise VIP for 13ºC vs. 30ºC at Day 22, 13ºC vs. 5ºC at Day 22, and Day 3 vs. Day 22 at 5ºC.

### Going forward

1. Complete WCNA for metabolomics data
2. Run metabolomics data through MetaboAnalyst
2. Analyze lipidomics data using the same workflow
3. Make SICB talk
4. Update methods
5. Update results
6. Integrate metabolomics and lipidomics data

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
