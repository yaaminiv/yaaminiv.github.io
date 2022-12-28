---
layout: post
comments: true
title: Green Crab Experiment Part 14
tags: green-crab
---

## Analyzing demographic data

Sara's been [analyzing the demographic data](https://github.com/yaaminiv/green-crab-metabolomics/issues/1) from the experiment (ex. carapace length, carapace width, weight, sex, integument color). We discussed looking at treatment differences at individual time points, as well as looking at differences within a single treatment over time in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/02-demographic-data.Rmd).

## Single-variable analysis

The first thing Sara did was look at differences by treatment or time points at the individual tank level. Within each tank, she found no significant difference of carapace length, width, or weight across time (Kruskal-Wallis test). This makes sense, because crabs carapace width and length change in a stepwise manner when crabs molt. Seeing no change in weight was good because I was worried the crabs weren't eating enough and would be losing weight! When she looked at integument color, she found significant changes in the composition of integument colors over time in tank 6 (chi-squared test). This matches our observations that tank 6 crabs looked redder than others and were progressing faster in their molt cycle. At each time point, she also compared variables between tanks. There were no significant differences in carapace width, carapace length, or weight between tanks at six timepoints. When looking at integument color, tank 6 once again had a different integument color composition at the last time point (7/26/22; chi-squared test).

I suggested using a post-hoc test to understand what was driving the differences in the chi-squared tests. She started performing Tukey HSD post-hoc tests but encountered some errors. She posted [this issue](https://github.com/yaaminiv/green-crab-metabolomics/issues/2) for me to look at. I realized the reasons why her Tukey HSD test was giving her errors were two-fold. One, she was switching the order of the explanatory and response variables. Two, one of the variables she wanted to use wasn't recognized as numerical by R. I [provided a solution in the issue](https://github.com/yaaminiv/green-crab-metabolomics/issues/2#issuecomment-1366843674), but also realized that she may need to use a different post-hoc test specifically for chi-squared analyses! Since she had to run an ANOVA prior to using the Tukey HSD test, the results from the ANOVA and chi-squared tests may not be comparable. I found [this Stack Overflow discussion](https://stackoverflow.com/questions/42096271/post-hoc-tests-for-chi-sq-in-r) with specific R packages for post-hoc testing and included it in the issue.

## Multivariate analysis

When we were going over her initial results, we thought a PCA may be a good approach to examine all demographic variables simultaneously. She had similar data format issues with `prcomp`, where some of her variables weren't recognized as numerical. Looking at the input data, I noticed one observation was missing weight data. This seemed odd to me, so I looked at the original Google Sheet we used to record data. There, I found a data inputting error where weight was shifted into the carapace length colum, and carapace width and length information were squished into the carapace width column! After fixing the error in the Google Sheet and confirming values with those in my notebook, I modified all other associated spreadsheets in the Github repository. I posted [this issue](https://github.com/yaaminiv/green-crab-metabolomics/issues/3) for Sara to recalculate average and SD information by tank found in the spreadsheets, and to point me to the code used to generate these values.

Then, I made sure her integument color data was recoded as numerical. Initially, the values to be used for recoding were in quotes, meaning R was interpreting them as characters:

```{r PCA data import}
all_crab <- read.csv("../../data/all_crab_dat.csv") #created a file with all crab data
all_crab$integument_color <- recode(all_crab$integument.color,"BG"=0.5,"G"=1,"YG"=1.5,"Y"=2,"YO"=2.5,"O"=3,"RO"=3.5) #recoded integument color into values. no quotes around numbers = will be recognized as numeric values
crab_dat <- all_crab[c(1:260),-c(6)] #selected columns of data (do not include integument color before recoding)
```

Since a PCA operates on numerical data, I needed a way to indicate the passage of time without using dates. I used `recode` to assign experimental day values to each date. I did something similar to convert the treatment tank information to experimental temperatures.

```{r PCA data formatting}
dat_all <- crab_dat %>%
  mutate(day = recode(date, "7/8/22" = 4, "7/12/22" = 8, "7/15/22" = 11, "7/19/22" = 15, "7/22/22" = 18, "7/26/22" = 22)) %>%
  mutate(temperature = recode(treatment.tank, "1" = 14, "2" = 6, "3" = 31, "4" = 14, "5" = 6, "6" = 31)) %>%
  select(., -c(treatment.tank, date)) #Make recode dates to days and create a new column for temperature. Drop date and treatment.tank
head(dat_all)
```

I was able to run `prcomp` with no error after making those changes! Following Julian's tutorial from multivariate statistics, I looked at the PCA summary information.

> Importance of components:
                            PC1       PC2       PC3       PC4       PC5       PC6
Variance(eigenvalue)   3.012132 1.2516571 1.0728162 0.5492910 0.0681393 0.0459642
Proportion of Variance 0.502022 0.2086095 0.1788027 0.0915485 0.0113565 0.0076607
Cumulative Proportion  0.502022 0.7106315 0.8894342 0.9809827 0.9923393 1.0000000
Broken-stick value     2.450000 1.4500000 0.9500000 0.6166667 0.3666667 0.1666667

Since PC1's eigenvalue is greater than the broken-stick value, there is support for PC1 being a significant component. I also ran a Monte Carlo randomization, shuffling the data 1000 times to obtain p-values.

> Randomization Test of Eigenvalues:
             PC1   PC2   PC3   PC4   PC5   PC6
Eigenvalue 3.012 1.252 1.073 0.549 0.068 0.046
P-value    0.000 0.000 0.071 1.000 1.000 1.000

Based on these p-values, there is support for PC1 and PC2 being important. When plotting the PCA results, I'll include PC1 and PC2. Finally, I looked at the correlations between individual variables and the PCs, as well as structure coefficients.

>  PC1        PC2          PC3         PC4         PC5         PC6
carapace.width    0.55456902 -0.1757016  0.011916707 -0.05755392 -0.35319521  0.73032927
carapace.length   0.55835526 -0.1387835 -0.004541656  0.05254161 -0.45881631 -0.67504435
weight            0.55314619 -0.1544451  0.043801885  0.04577913  0.81392965 -0.06066461
integument_color -0.21369838 -0.6880887  0.112985210  0.68271475 -0.02716681  0.03555045
day              -0.08987232 -0.2630039  0.855982434 -0.43282926 -0.01427649 -0.05000991
temperature      -0.14483737 -0.6191750 -0.502441216 -0.58171350  0.03629607 -0.05907002

Carapace width, length, and weight were strongly positively associated with PC1, while integument color and temperature were associated with PC2.

![Screenshot 2022-12-28 at 2 51 43 PM](https://user-images.githubusercontent.com/22335838/209882444-70df46f8-22dc-4b62-b0a9-a4d27aaae2b7.png)

**Figure 1**. Demographic data PCA

PC1 explains 50.2% of the variation in the data, while PC2 explains 20.86% variation in the data. Looking at the initial PCA, it seems like ambient and cold data are separated from warm data. I asked Sara to modify the PCA by having different colors for each temperature and adding 95% confidence ellipses around each group so we can understand where they separate. I also asked her to create an index for the carapace length and width data. Since width and length have a fixed ratio and are autocorrelated, it would strengthen the analysis to include an index that summarizes the results, as opposed to including both variables. I also asked her to include sex information so we can see if that is an important variable!

### Going forward

1. Order extraction materials
2. Finalize demographic data analysis
3. Update methods
4. Update results
7. Respirometry analysis
5. Survivorship analysis
6. TTR analysis

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
