---
layout: post
comments: true
title: Green Crab Experiment Part 52
tags: green-crab glmm
editor_options:
  markdown:
    wrap: 72
---

## Revising TTR analysis

I'm in the process of addressing reviewer comments on the manuscript! One comment I got is that because crabs were randomly sampled within a tank but we had no crab IDs, it's very possible that some crabs were measured multiple times over the course of the experiment, so data isn't truly independent.

Valid.

So time to figure out if 1) the way I analyzed the data is the most appropriate and 2) if not, the best analytical approach so I can 3) re-analyze the data.

### Reformatting data

One of the reviewers was concerned about the fact that some crabs were missing legs (including swimmers!) since dropping legs is a sign of extreme stress. A lot of the crabs in this experiment were missing legs when we got them because they were kept in a rough state. But to ensure that I could statistically test the impact that missing legs had on the data, I did a quick [reformat in the data](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/data/time-to-right.csv) itself to actually count the number of legs and number of swimmers for each crab. Dealing with it this way was a lot easier than with R, since I had to go through notes to do so.

### Using a different model

I've poked around for this stuff on the internet before, but have never found anything very helpful. Maybe I wasn't using the right search terms? In any case, I used our institutional Gemini to come up with potential approaches then groundtruth them. The suggestion was to use some group-level aggregation as a random effect, since I can't drill down to an individual crab. Since crabs were re-sampled within the same tank over the course of the experiment, this meant that observations within the same tank would be correlated. Therefore, I used tank as a random effect. I then used a GLMM with a gamma distribution and a log-link function (wow reminds me of Skalski's class) to model average righting response. Since the log-link function works better for right-skewed data, I didn't need to log-transform average righting response prior to fitting the model. I decided to only use the number of swimmers in the model, as opposed to total number of legs, as the swimmers have the largest role in initiating the righting response:

```
TTRmodelGamma <- glmer( #Generalized linear mixed effect model
  TTRavg ~ as.factor(treatment) + day.sc + as.factor(treatment):day.sc + #Treatment, day, and their interaction
    as.factor(sex) + integument.cont + carapace.width + number.swimmers + #Demographic variables: sex, integument color, carapace width, and number of swimmers
    (1 | treatment.tank), #Use tank as a random effect to account for potential dependence between data
  data = modTTR,
  family = Gamma(link = "log") #Gamma model with a log link
)
```

Immediately, I ran into a gradient warning indicating convergence issues!

> Warning in checkConv(attr(opt, "derivs"), opt$par, ctrl = control$checkConv, :
Model failed to converge with max|grad| = 0.0153676 (tol = 0.002, component 1)

I scaled the numerical variables to mean = 0 and SD = 1 to counteract this:

```
#Scale continuous variables for the GLMM

modTTR$carapace.width.sc <- scale(modTTR$carapace.width)
modTTR$day.sc <- scale(modTTR$day)
modTTR$integument.sc <- scale(modTTR$integument.cont)
modTTR$number.swimmers.sc <- scale(modTTR$number.swimmers)
```

Scaling the parameters modifies the measurement unit to standard deviations instead of days, number of legs, mm, etc. Scaling won't prevent me from interpreting model output afterwards, since the information can always be extracted for the original raw parameters. Once my parameters were scaled, I was able to run my model without any convergence issues. I used `drop1` to perform model selection by backwards deletion and obtained the most parsimonious model:

```
TTRmodelGamma4 <- glmer( #Generalized linear mixed effect model
  TTRavg ~ as.factor(treatment) + day.sc + as.factor(treatment):day.sc + #Treatment, day, and their interaction
    as.factor(sex) + #Demographic variables: sex
    (1 | treatment.tank), #Use tank as a random effect to account for potential dependence between data
  data = modTTR,
  family = Gamma(link = "log") #Gamma model with a log link
)
```

### A detour in assumption checking and model tweaking

Now that I had a model, I could check assumptions! The following assumptions needed to be checked:

1. Multicollinearity: Avoid correlations between different predictors in the model
2. Uniform distribution of model residuals
3. Residual dispersion
4. Normality of random effects

My model passed the multicollinearity assumption check! I used the [`DHARMa` package to simulate residuals for my model](https://cran.r-project.org/web/packages/DHARMa/vignettes/DHARMa.html), which is standard for GLMMs:

```
sim_residuals <- simulateResiduals(fittedModel = TTRmodelGamma4, n = 1000, plot = TRUE) #Simulate residuals for the final model
```

Once I had my simulated residuals, it spit out a QQ plot and quantile deviation plot:

<img width="868" height="544" alt="Image" src="https://github.com/user-attachments/assets/a0ea38c6-10fe-4049-8fc4-0261c0f1b97a" />

**Figure 1**. Diagnostic plots for simulated residuals from `glmer` model

Immediately, I could see that there were residual uniformity, dispersion, and quantile deviation issues! I went through the `DHARMa` manual to understand these results. My first step was to run the Kolmogorov-Smirnov uniformity and dispersion tests separately to understand the flags:

```
testUniformity(sim_residuals) #Kolmogorov-Smirnov test
testDispersion(sim_residuals) #Test dispersion
```

Based on the output and similar test results in the `DHARMa` manual, I had an underdispersion issue. The manual suggested that underdispersion can come from overfitting the model and wasn't as big of an issue compared to overdispersion, but it could be fixed by tweaking the model itself. Gemini suggested that the underdispersion didn't come from overfitting (given that I didn't have many predictors), but instead from an estimation limmitation in `glmer` that could be remedied by switching to `glmmTMB`. I installed `glmmTMB` from source (`install.packages("glmmTMB", type = "source"`) to deal with some `TMB` compatibility issues (side note: VERY helpful error message and help pages allowed me to resolve this issue seamlessly), and then reran my model using `glmmTMB`:

```
TTRmodelGamma4 <- glmmTMB( #Generalized linear mixed effect model
  TTRavg ~ as.factor(treatment) + day.sc + as.factor(treatment):day.sc + #Treatment, day, and their interaction
    as.factor(sex) + #Demographic variables: sex
    (1 | treatment.tank), #Use tank as a random effect to account for potential dependence between data
  data = modTTR,
  family = Gamma(link = "log") #Gamma model with a log link
)
```

I went through all of the `drop1` model selection steps to confirm that changing from `glmer` to `glmmTMB` didn't change the most parsimonious model. I then went back and simulated residuals to understand if this improved the model.

<img width="860" height="533" alt="Image" src="https://github.com/user-attachments/assets/39e6e49a-160f-4814-b069-44d3fbc4d67c" />

**Figure 2**. Diagnostic plots for simulated residuals from `glmmTMB` model

The underdispersion has been fixed! However, the residuals still are distributed non-uniformly and there are quartile deviations. According to the manual, the suggested solution to remove the pattern in the residuals is "to make the dispersion parameter dependent on a predictor (e.g. in JAGS), or apply a transformation on the data." I've already ruled out data transformation based on the Gamma distribution. I wasn't sure how to make the dispersion parameter dependent on a predictor, so I queried Gemini. Based on the suggestion, I added `dispformula` to model dispersion directly as a function of treatment temperature. The goal of this tweak was to see if adding different dispersion parameters for 5ºC, 13ºC, and 30ºC improved the residual diagnostic plots by removing heteroscedasticity. This......did not happen:

<img width="512" height="319" alt="Image" src="https://github.com/user-attachments/assets/efaa0afc-8588-479f-94ea-bca42c6a84a6" />

**Figure 3**. Residual plots for gamma model with dispersion based on temperature condition.

Adding a different dispersion for each temperature has now led to an overdispersion issue in the model, in addition to non-uniform residuals, outliers, and quantile curvature issues! I removed this from the model immediately. The next Gemini suggestion was to try different model families: log-normal, inverse gaussian, and tweedie. I tried those model families but the AIC of the most parsimonious model was higher than the AIC for the most parsimonious gamma model, and using those model families led to more flags in the simulated residual plots. Additionally, I couldn't get the tweedie model to converge! Another Gemini suggestion was to try adding a polynomial term for day as suggested by the curvature. When going through model selection, this term was one of the ones I needed to drop to improve AIC! So, I was still left with my gamma model. I dug through the `DHARMa` manual more and found a function to look at the residual distribution against temperature, a categorical predictor variable:

```
testCategorical(sim_residuals, catPred = ~ as.factor(treatment)) #Test quantile distributions against a specific categorial predictor variable
```

<img width="836" height="520" alt="Image" src="https://github.com/user-attachments/assets/34afe411-120c-4175-96f1-cda1660f5fb1" />

**Figure 4**. Quantile distributions by temperature treatment

Based on this figure, there is much higher variance in the 5ºC treatment. I think this makes sense biologically: cold temperature can lead to more variation in righting response because some crabs are going to be a lot slower than "normal."

So, I have a model with imperfect residual results. In the `DHARMa` manual, there's a section assuring users that sometimes a model that is significant for some of these tests is still a usable model. Given that my QQ plot is so close to being linear, I decided to keep this model. Since my model used `glmmTMB` instead of `glmer`, I couldn't use the method for multicollinearity checks previously, and had to modify my code and use `check_collinearity` within the `performance` package:

```
check_collinearity(TTRmodelGamma4) # Check multicollinearity assumption.
```

All of the assumptions were met! My adjusted VIF values (which are important for a model with random effects) were all less than 2, and the test itself spit out a green "Low Collinearity" message. I also used the `performance` package to chekc for normality of the random effect:

```
re_normality <- check_normality(TTRmodelGamma4, effects = "random") # Check random effect normality
re_normality #Random effects are normally distributed (p = 0.762)
```

<img width="1750" height="1081" alt="Image" src="https://github.com/user-attachments/assets/c7629a21-8a95-4370-8d98-a5340170dbb2" />

**Figure 5**. QQ plot of random effects

Look at that beautiful QQ plot. Based on a Gemini suggestion, I also ran a boostrap test on my model coefficients to confirm that my output remained stable:

```
boot_params <- bootstrap_parameters(TTRmodelGamma4, iterations = 1000) #Calculate bootstrapped p-values and 95% CI to on model coefficients
print(boot_params) #See results of boostrapping
```

When comparing the boostrapping results with my original model output, I saw that all the coefficient inferences were stable! That's a good sign. I then combined the boostrapping output with the model output:

```
summary(TTRmodelGamma4)[[6]][[1]] %>% #Original model output
  as.data.frame(.) %>% #Convert to a dataframe
  rownames_to_column(., var = "Parameter") %>% #Convert rownames to Parameter column
  left_join(x = .,
            y = boot_params %>%
              as.data.frame(.), #Join with bootstrap output converted to a dataframe
            by = "Parameter") %>% #Join by the Parameter column
  write.csv(., "TTRmodelGamma4-output.csv", quote = FALSE, row.names = FALSE) #Save table version of GLMM output with boostrapping results
```

The table of all model and bootstrapping output can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/03-TTR-analysis/TTRmodelGamma4-output.csv).

At this point, it's time to pause and update the methods and results sections of the manuscript with what I've done so far.

### Going forward

1.  Interpret new TTR model
1.  Run post-hoc tests on new TTR model
1.  Address remaining methods comments
2.  Address results comments
3.  Address figure comments
4.  Address discussion comments
5.  Send revised manuscript to co-authors

{% if page.comments %}

::: {#disqus_thread}
:::

```{=html}
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
```

<noscript>Please enable JavaScript to view the
<a href="https://disqus.com/?ref_noscript">comments powered by
Disqus.</a></noscript>

{% endif %}

```{=html}
<script id="dsq-count-scr" src="//the-responsible-grad-student.disqus.com/count.js" async></script>
```
