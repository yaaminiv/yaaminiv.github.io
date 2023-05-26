---
layout: post
comments: true
title: Green Crab Experiment Part 17
tags: green-crab
---

## Finishing the demographic data analysis

[Sara started analyzing the demographic data from the experiment](https://yaaminiv.github.io/Green-Crab-Experiment-Part14/). I've been putting off finishing up her analysis, so obviously I decided to finish it since starting the respirometry analysis felt too difficult for my post-vacation brain. I would have finished it quickly, except I kept diving down analysis rabbit holes instead of focusing on the goals of the analysis. So let's lay them out.

My goal with the demographic data analysis was initially to demonstrate that there were no differences in carapace width, length, sex ratios, or integument colors between the tanks. As I went through the data analysis steps, I realized a secondary goal was to determine if significant differences in the demographic variables could be attributed to temperature or time. This would allow me to justify including these variables in the [time-to-right](https://yaaminiv.github.io/Green-Crab-Experiment-Part16/) and respirometry analyses, or provide reason for me to not include them if they weren't significantly different between treatments.

### PCA: Visual inspection of data

I used a two-fold approach for my analysis. The first was to build upon the PCA Sara started to understand multivariate differences between groups. The initial PCA had carapace width and length as two separate variables, but these variables are correlated since crabs carapaces have a standard ratio between width and length (for the most part). I created an index variable by dividing width and length. Rerunning the PCA with the index variable changed the significance of the PCs:

```
Importance of components:
                             PC1       PC2       PC3       PC4       PC5       PC6
Variance(eigenvalue)   1.6680014 1.3040093 1.1000417 0.9466031 0.5161098 0.4652346
Proportion of Variance 0.2780002 0.2173349 0.1833403 0.1577672 0.0860183 0.0775391
Cumulative Proportion  0.2780002 0.4953351 0.6786754 0.8364426 0.9224609 1.0000000
Broken-stick value     2.4500000 1.4500000 0.9500000 0.6166667 0.3666667 0.1666667
```

```
             PC1   PC2   PC3   PC4   PC5   PC6
Eigenvalue 1.668 1.304 1.100 0.947 0.516 0.465
P-value    0.000 0.000 0.002 0.781 1.000 1.000
```

PC1 and PC2 did not explain more variation than the broken-stick model, but PC3 did. A permutation test with 1000 iterations showed that PC1-3 were significant. Even though PC1 and PC2 did not explain more variation than the broken-stick model, together they explained almost 50% of the variation in the data, which is why I chose to keep them.

I examined the structure coefficients to see which loadings were associated with each significant PC. It seems like temperature is strongly associated with the third PC, so I figured I would retain three PCs from the analysis.

```
pca.structure(pca_res_crab_wl, dat_wl, dim = 3, cutoff = 0.4)
```

```
                    PC1    PC2    PC3
weight            0.735              
sex               0.706 -0.501       
integument_color -0.602 -0.566       
day                     -0.585   0.57
temperature                    -0.697
size                    -0.447 -0.483
```

Finally, I looked to see if all of the loadings were significant using a permutation with 1000 iterations.

```
envfit(scores(pca_res_crab_wl), dat_wl, choices = c(1:3), perm = 1000)
```

```
                     PC1      PC2      PC3     r2   Pr(>r)    
weight            0.85897 -0.43852  0.26434 0.6836 0.000999 ***
sex               0.77348 -0.62164 -0.12366 0.7576 0.000999 ***
integument_color -0.67632 -0.71905  0.15986 0.6968 0.000999 ***
day              -0.34058 -0.64499  0.68410 0.7887 0.000999 ***
temperature      -0.36893 -0.32528 -0.87068 0.6991 0.000999 ***
size              0.14508 -0.64019 -0.75439 0.4463 0.000999 ***
```

Since all of the loadings are significant, I created a multipanel PCA plot with all of the loadings. I colored the data points by temperature and used different shapes for temperature as well.

```
demPCA12 <- autoplot(pca_res_crab_wl, data = dat_wl, x = 1, y = 2, colour = "temperature", shape = "temperature", size = 2, loadings = TRUE, loadings.colour = 'black', loadings.label = TRUE, loadings.label.size = 5, loadings.label.colour = 'black') +
  scale_color_manual(values = c(plotColors[2], plotColors[1], plotColors[3]),
                     name = "Temperature (ºC)",
                     breaks = c("13", "30", "5"),
                     labels = c("13", "30", "5"),
                     guide = "none") +
  stat_ellipse(geom = "polygon", aes(color = temperature, fill = temperature), linewidth = 0.3, alpha = 0.15) +
  scale_fill_manual(values = c(plotColors[2], plotColors[1], plotColors[3]),
                    name = "Temperature (ºC)",
                    breaks = c("13", "30", "5"),
                    labels = c("13", "30", "5"),
                    guide = "none") +
  scale_shape_manual(values = c(19, 17, 15),
                     name = "Temperature (ºC)",
                     breaks = c("13", "30", "5"),
                     labels = c("13", "30", "5"),
                     guide = "none") +
  theme_classic(base_size = 15) #Base plot. Don't include legend

demPCA13 <- autoplot(pca_res_crab_wl, data = dat_wl, x = 1, y = 3, colour = "temperature", shape = "temperature", size = 2, loadings = TRUE, loadings.colour = 'black', loadings.label = TRUE, loadings.label.size = 5, loadings.label.colour = 'black') +
  scale_color_manual(values = c(plotColors[2], plotColors[1], plotColors[3]),
                     name = "Temperature (ºC)",
                     breaks = c("13", "30", "5"),
                     labels = c("13", "30", "5")) +
  stat_ellipse(geom = "polygon", aes(color = temperature, fill = temperature), linewidth = 0.3, alpha = 0.15) +
  scale_fill_manual(values = c(plotColors[2], plotColors[1], plotColors[3]),
                    name = "Temperature (ºC)",
                    breaks = c("13", "30", "5"),
                    labels = c("13", "30", "5")) +
  scale_shape_manual(values = c(19, 17, 15),
                     name = "Temperature (ºC)",
                     breaks = c("13", "30", "5"),
                     labels = c("13", "30", "5")) +
  theme_classic(base_size = 15) + theme(legend.justification = c(0,0), legend.position = c(0,0),
                                        legend.background = element_rect(color = "black", linewidth = 0.5)) #Base plot. Include legends. Anchor legend in the bottom left corner and add a line around the legend.

demPCA12 | demPCA13 #Create patchwork plot
```

![Screenshot 2023-05-26 at 11 43 00 AM](https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/21a1ca2c-f9d2-47c9-80ba-60a63c0badf3)

**Figure 1**. PCA of crab demographic data.

### MANOVA: Statistically significant differences between treatments

My next step involved determining an appropriate statistical test to complement the PCA. At the end of the statistical testing, I wanted to say which variables were influenced by temperature or experimental day. I could then use the [plots Sara initially created](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/02-demographic-data/All_figs.pdf) to say if the control or a temperature treatment had a higher or lower weight, etc.

Initially, I thought I would use a perMANOVA to look at group differences, similar to what I did for the MethCompare paper. However, I wanted a statistical test that looked at differences between group means, not dispersions. I settled on a Multivariate Analysis of Variance (MANOVA) to look at overall patterns, then ANOVA with Bonferroni corrections post-hoc. Of course, ChatGPT fed me all the information I needed to learn about the difference in these statistical tests.

I first used a MANOVA to explore the influence of temperature, day, and sex on weight, size, and integument color. At first, I had sex as a response variable. After thinking about it some more, I realized that sex needed to be an independent variable since temperature and time may influence survivorship and the sex ratio, but that's not what I'm testing for (and I have too small of a sample size to do so).

```
demMANOVA <- manova(cbind(weight, size, integument_color) ~ as.factor(temperature)*day*as.factor(sex), data = dat_wl) #Conduct a MANOVA to examine the influence of temperature, day, and sex on weight, size, and integument color.
summary(demMANOVA) #Significant impact of temperature, day, and sex on demographic variables. Interactions between day and both temperature and sex are significant as well.
```

```
                                          Df     Pillai   approx F num Df den Df       Pr(>F)
as.factor(temperature)                      2 0.16463726  9.9271147      6    664 1.655494e-10
day                                         1 0.17063407 22.7000236      3    331 2.175552e-13
as.factor(sex)                              1 0.28389829 43.7416149      3    331 7.780097e-24
as.factor(temperature):day                  2 0.05164096  2.9332032      6    664 7.850702e-03
as.factor(temperature):as.factor(sex)       2 0.03745919  2.1123047      6    664 4.999046e-02
day:as.factor(sex)                          1 0.06399118  7.5430488      3    331 6.771483e-05
as.factor(temperature):day:as.factor(sex)   2 0.01448400  0.8072942      6    664 5.644015e-01
Residuals                                 333         NA         NA     NA     NA           NA
```

Based on the MANOVA, temperature, day, and sex had significant impacts on the multivariate demographic variables. The interaction with day and both temperature and sex were also significant. To dissect this further, I used ANOVA for single variables and Bonferroni corrections to deal with multiple comparisons.

```
#Example code for Bonferroni correction and ANOVA
summary(aov(weight ~ as.factor(temperature)*day*as.factor(sex), data = dat_wl))[[1]] %>%
  as.data.frame(.) %>%
  mutate(., padj = p.adjust(p = `Pr(>F)`, method = "bonferroni"))
```

```
#weight
                                           Df      Sum Sq     Mean Sq      F value       Pr(>F)         padj
as.factor(temperature)                      2  1119.86261  559.931306  12.35410011 6.674268e-06 4.671988e-05
day                                         1    28.78130   28.781301   0.63501910 4.260880e-01 1.000000e+00
as.factor(sex)                              1  4913.87445 4913.874453 108.41775812 3.637849e-22 2.546494e-21
as.factor(temperature):day                  2    58.42172   29.210859   0.64449670 5.255802e-01 1.000000e+00
as.factor(temperature):as.factor(sex)       2   144.82489   72.412446   1.59767922 2.039129e-01 1.000000e+00
day:as.factor(sex)                          1    47.56935   47.569349   1.04955108 3.063541e-01 1.000000e+00
as.factor(temperature):day:as.factor(sex)   2     2.38465    1.192325   0.02630698 9.740381e-01 1.000000e+00
Residuals                                 333 15092.73224   45.323520           NA           NA           NA
```
```
#size

                                           Df      Sum Sq     Mean Sq   F value       Pr(>F)        padj
as.factor(temperature)                      2 0.011942780 0.005971390  2.121545 0.1214633343 0.850243340
day                                         1 0.003867339 0.003867339  1.374008 0.2419624184 1.000000000
as.factor(sex)                              1 0.037875673 0.037875673 13.456660 0.0002841409 0.001988986
as.factor(temperature):day                  2 0.015355490 0.007677745  2.727788 0.0668245846 0.467772092
as.factor(temperature):as.factor(sex)       2 0.007179304 0.003589652  1.275350 0.2806940722 1.000000000
day:as.factor(sex)                          1 0.012753698 0.012753698  4.531198 0.0340159658 0.238111761
as.factor(temperature):day:as.factor(sex)   2 0.008655863 0.004327931  1.537649 0.2164074251 1.000000000
Residuals                                 333 0.937275618 0.002814641        NA           NA          NA
```

```
#integument_color

                                           Df      Sum Sq    Mean Sq    F value       Pr(>F)         padj
as.factor(temperature)                      2  12.1806677  6.0903339 17.8407712 4.360211e-08 3.052148e-07
day                                         1  22.0155658 22.0155658 64.4914845 1.689393e-14 1.182575e-13
as.factor(sex)                              1   1.0528279  1.0528279  3.0841105 7.997972e-02 5.598581e-01
as.factor(temperature):day                  2   3.5175635  1.7587818  5.1521023 6.257358e-03 4.380150e-02
as.factor(temperature):as.factor(sex)       2   2.2367089  1.1183545  3.2760612 3.899789e-02 2.729852e-01
day:as.factor(sex)                          1   5.4435963  5.4435963 15.9462450 8.022732e-05 5.615913e-04
as.factor(temperature):day:as.factor(sex)   2   0.4197818  0.2098909  0.6148457 5.413370e-01 1.000000e+00
Residuals                                 333 113.6767663  0.3413717         NA           NA           NA
```

Temperature and sex has a significant impact on weight. This makes sense because males tend to be larger than females. Even though I fed the crabs ad libidum, I did see the crabs in the 30ºC treatment not eat as much as the rest of the crabs. They also have much higher metabolisms at that temperature that they may be burning resources. Only sex had a significant impact on size, which again, is because males are larger than females. The MANOVA significance was driven by crab integument color. Integument color significantly differed between temperature treatments and over time, with the interactions between temperature and day and sex and day also being significant. Sex on it's own was not significant. Anecdotally, we saw the crabs in the 30ºC treatment have redder integuments. Since integument color is a proxy for intermolt status, the redder the integument the closer the crab is to molting. We also know that temperature does play a role in molt-readiness (again, temperature speeds up metabolism).

Overall, the analysis shows me that there were significant differences in gross physiological metrics between temperature treatments and over time. This justifies the use of these variables when exploring differences in crab time-to-right or respiration.

Oh side note: all of the statistical output from each test was saved in [this folder](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/02-demographic-data).

### Going forward

1. Test formatting and analysis for one set of respirometry data
8. Respirometry analysis
3. Update methods
4. Update results

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
