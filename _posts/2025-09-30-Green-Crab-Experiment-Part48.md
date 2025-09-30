---
layout: post
comments: true
title: Green Crab Experiment Part 48
tags: green-crab metabolomics lipidomics DIABLO
---

## Metabolite-lipid relationships with DIABLO

Since I'm trying out a bunch of different analytical methods, next on my list is to use DIABLO from `mixOmics.` The output of this supervised analysis will give me individual metabolite-lipid correlations, which would build upon the module eigengenes I got from my [combined WCNA analysis](https://yaaminiv.github.io/Green-Crab-Experiment-Part47/).

All my work was done in [this script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/07-integration-analysis.Rmd). Helpful resources:

- [`mixOmics` vignette](https://mixomicsteam.github.io/mixOmics-Vignette/id_06.html)
- [DIABLO case study](https://mixomics.org/mixdiablo/diablo-tcga-case-study/)
- [Ariana's lab notebook](https://ahuffmyer.github.io/ASH_Putnam_Lab_Notebook/Multi-Omic-Integration-Analysis-Part-1/)

### Preliminary sPLS

The first step of any `mixOmics` analysis is to parameterize the supervised analysis. First, I investigated the correlations between the metabolomics and lipidomics datasets with the sPLS so I could use that information in designing the DIABLO analysis. Visually inspecting the sPLS, I saw that there were some lipids correlated with metabolites but there weren't any clear patterns along wither component 1 or component 2. To get a correlation value, I ran an sPLS with only one component:

```
res1.pls.tcga <- pls(X, Y, ncomp = 1)
cor(res1.pls.tcga$variates$X, res1.pls.tcga$variates$Y)
```

My correlation value was ~0.78, so that's decently correlated for biological variables.

### Tune individual variables

For the DIABLO analysis, my variable of interest was a lists that had both metabolomics and lipidomics data. The example had an "outcome" of breast cancer classification. For me, my outcome was temperature. I then had to specify a DIABLO design using the correlation information I obtained.

```
designML <- matrix(cor(res1.pls.tcga$variates$X, res1.pls.tcga$variates$Y),
                   ncol = length(dataML), nrow = length(dataML),
                   dimnames = list(names(dataML), names(dataML))) #Create a matrix with the design of the DIABLO experiment. Lower weights increase predictive capability. I am using a correlation based on previous PLS
diag(designML) <- 0
```

For the design, if I set weights at 0.1, I'd increase the predictive capability of the DIABLO. However, my goal is to pull out biologically-informed correlations, which is why I used a pre-determined correlation value from the sPLS.

Next, I had to figure out how many components to include in the analysis:

```
diablo.tcga.ML <- block.plsda(X = dataML,
                              Y = outcomeML,
                              ncomp = 5,
                              design = designML) #Run a DIABLO analysis to identify the number of components that should be included

perf.diablo.tcga.ML = perf(diablo.tcga.ML, validation = 'Mfold', folds = 10, nrepeat = 10)
```

Plotting the error rates showed me that the lowest error rate came from using 4 components. However, I only have three temperature conditions, so I am sticking with a 3 components maximum. When looking at the error rates for 3 components, using mahalanobis distance reduced the error rate.

Using the component and error rate information, I needed to determine the number of metabolites and lipids to use in the analysis. I initially went about this by taking the tuning information from the PLS-DA I did. However, those were done with each variable separately, and tuning for the DIABLO requires both variables together. So, I figured better safe than sorry to tune using DIABLO-specific parameters:

```
set.seed(123) # for reproducibility
test.keep.ML <- list(metabolomics = c(5:9, seq(10, 100, 5)),
                   lipidomics = c(5:9, seq(10, 100, 5)))

tune.diablo.ML <- tune.block.splsda(X = dataML, Y = outcomeML, ncomp = 3,
                              test.keepX = test.keep.ML, design = designML,
                              validation = 'Mfold', folds = 10, nrepeat = 1,
                              BPPARAM = BiocParallel::SnowParam(workers = 2),
                              dist = "mahalanobis.dist")
```

### Run DIABLO

At last, I could run my DIABLO!

```
diablo.tcga.ML <- block.splsda(X = dataML, Y = outcomeML,
                               ncomp = 3,
                               keepX = list.keep.ML,
                               design = designML) #Run DIABLO with all variables tested above
diablo.tcga.ML$design #Outcomes now included in the design so that the covariance between each block’s component and the outcome is maximised
```

I made a handful of visualizations, but the one I wanted to make the most was the circos plot:

```
circosPlot(diablo.tcga.ML, cutoff = 0.70, line = TRUE,
           color.blocks = c('plum1', 'lightgreen'),
           color.cor = c("chocolate3","grey20"),
           size.labels = 1,
           legend.title = "Temperature (ºC)", color.Y = rev(plotColors)) #Use circos plot to visualise correlations between individual metabolites and lipids
```

<img width="841" height="756" alt="Image" src="https://github.com/user-attachments/assets/4b860d5d-557f-43b8-b02d-ab68d53942ee" />

**Figure 1**. Circos plot with correlations between metabolites and lipids

The plot shows that there are very few correlations between compounds. When I messed around with the correlations, I could only get this plot when corr < 0.75, so these aren't very strong correlations to begin with. That isn't surprising to me given the fact that I had several module eigengenes in my combined WCNA that were comprised of only metabolites or only lipids.

In her code, Ariana pulled out a correlation matrix from the DIABLO for significant correlations. When I went to do this, I noticed two things:

1. Correlations on the diagonal weren't 1?!
2. A lot of my correlations were not significant according to the 0.7 cutoff

Some searching showed me that the current functionality (`cor_matML <- circosPlot(diablo.tcga.ML, cutoff = 0.70)`) pulls out correlations projected onto the components, which isn't helpful for understanding individual compounds that are correlated with eachother (see [this link](https://mixomics-users.discourse.group/t/diablo-circosplot-export-correlation-matrix/22). That lack of functionality really reduces the power of this analysis for interpreting data.

Out of curiosity, I also plotted the figure with correlations between two metabolites or two lipids:

<img width="841" height="756" alt="Image" src="https://github.com/user-attachments/assets/e5e267bb-cd7e-4d44-bbd9-e02886c006a6" />

**Figure 2.** All correlations

Again, there were many more correlations between two metabolites or between two lipids, which is exactly what I was expecting.

### Repeat with only VIP

When looking at the different compounds around the circos plot, I wondered what would happen if I used just VIP compounds for the plot. So obviously, I tried that:

```
X.VIP <- X %>%
  dplyr::select(any_of(metaboliteVIP$metabolite)) #Metabolomics data but only VIP

Y.VIP <- Y %>%
  dplyr::select(any_of(lipidVIP$lipid)) #Lipid data but only VIP
```

Once I formatted my VIP lists, the only thing I had to tune was the number of compounds used, since I was still going to use 3 components and the same design.

<img width="841" height="756" alt="Image" src="https://github.com/user-attachments/assets/060688e7-aa77-47c7-a44a-c06f1c3ff048" />

**Figure 3**. Circos plot for just VIP

Honestly? It doesn't look that much different than what I had previously. But if gives me some options depending on how I massage out the interpretation.

### Going forward

1. ASCA analysis
2. Update methods
3. Update results
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
