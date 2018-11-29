---
layout: post
comments: true
title: MEPS Revisions Part 3
tags: DNR SRM MEPS-Revisions
---

## Final unconstrained and constrained ordinations

I *finally* figured out all of my N/A problems and completed my unconstrained and constrained ordinations!

### Protein abundance

I didn't have any problems with my protein abundance NMDS and [previously](https://yaaminiv.github.io/MEPS-Revisions-Part2/) obtained pairwise ANOSIM R-statistic and p-values. In this [R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-ANOSIM-for-Cluster-Analysis.R), I visualized my ordinations. You can find one version with sample numbers [here](https://render.githubusercontent.com/view/pdf?commit=db25c4adacf82e173c9804df28b3eacc12203e09&enc_url=68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f646232356334616461636638326531373363393830346466323862336561636331323230336530392f616e616c797365732f444e525f53524d5f32303137303930322f323031372d31302d31302d54726f75626c6573686f6f74696e672f323031372d31312d30352d496e74656772617465642d446174617365742f323031382d31312d32382d50726f7465696e2d4162756e64616e63652d4e4d44532d456c6c697073652e706466&nwo=RobertsLab%2Fproject-oyster-oa&path=analyses%2FDNR_SRM_20170902%2F2017-10-10-Troubleshooting%2F2017-11-05-Integrated-Dataset%2F2018-11-28-Protein-Abundance-NMDS-Ellipse.pdf&repository_id=68737231&repository_type=Repository#6eab06ec-4253-43a7-a34f-235fa914060d), and another with site and habitat distinctions [here](https://render.githubusercontent.com/view/pdf?commit=db25c4adacf82e173c9804df28b3eacc12203e09&enc_url=68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f646232356334616461636638326531373363393830346466323862336561636331323230336530392f616e616c797365732f444e525f53524d5f32303137303930322f323031372d31302d31302d54726f75626c6573686f6f74696e672f323031372d31312d30352d496e74656772617465642d446174617365742f323031382d31312d32382d50726f7465696e2d4162756e64616e63652d536974652d486162697461742d4e4d44532d456c6c697073652e706466&nwo=RobertsLab%2Fproject-oyster-oa&path=analyses%2FDNR_SRM_20170902%2F2017-10-10-Troubleshooting%2F2017-11-05-Integrated-Dataset%2F2018-11-28-Protein-Abundance-Site-Habitat-NMDS-Ellipse.pdf&repository_id=68737231&repository_type=Repository#b0d6635e-d9a0-4d7d-bb21-7a6b2ec689bd). Because I exported my files as pdfs and don't feel like changing the code and re-exporting files, I'm also including screenshots of the ordinations.

<img width="808" alt="screen shot 2018-11-29 at 10 58 33 am" src="https://user-images.githubusercontent.com/22335838/49245366-f4be8180-f3c6-11e8-8a8e-55ce601b9d68.png">

<img width="813" alt="screen shot 2018-11-29 at 10 58 49 am" src="https://user-images.githubusercontent.com/22335838/49245367-f4be8180-f3c6-11e8-878d-9d5baf234d7d.png">

**Figures 1 and 2**. Protein abundance ordinations with confidence ellipses.

### Environmental data

My original problem with the environmental data was that I had N/As in my dataframe, but was unable to use `method = "gower"` with the `metaMDS` function in the package `vegan`. Turns out `vegan` uses `vegdist`, which doesn't handle N/As. Instead, I used the `daisy` function in the `cluster` library to calculate a Gower's dissimilarity (distance) matrix. I then inputted that matrix directly into `metaMDS` with `method = "euclidean"`. It worked! My code can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-11-15-Environmental-Data-and-Biomarker-Analyses/2017-12-13-Environmental-Data-Quality-Control/2018-11-18-Environmental-Data-NMDS.Rmd). I then conducted an Analysis of Similarity (ANOSIM) to assess the significance of my results.

**Table 1**. One-way ANOSIM results for environmental data NMDS based on site (Case Inlet vs. Fidalgo Bay vs. Port Gamble Bay vs. Skokomish River Delta vs. Willapa Bay), habitat (bare vs. eelgrass), parameter (mean vs. variance), and environmental variable (pH vs. dissolved oxygen vs. salinity vs. temperature). Significant p-values are bolded.

|     **One-way ANOSIM**     |    **R**    |   **p-value**   |
|:--------------------------:|:-----------:|:---------------:|
|          **Site**          | -0.03428841 |      0.943      |
|         **Habitat**        | -0.02037461 |      0.982      |
|        **Parameter**       |  0.7378174  |    **0.001**    |
| **Environmental Variable** |  0.1796322  |    **0.001**    |

Only the parameter and environmental variable ANOSIMs were significant. My guess is that because I ordinated means and variances in the same space, this is skewing my results. I decided to conduct two-way ANOSIMs to go further.

**Table 2**. Two-way ANOSIM results for environmental data NMDS. Significant p-values are bolded.

|            **Two-way ANOSIM**            |    **R**    | **p-value** |
|:----------------------------------------:|:-----------:|:-----------:|
|           **Site and Habitat**           | -0.09991229 |      1      |
|          **Parameter and Site**          |  0.4778027  |  **0.001**  |
|         **Parameter and Habitat**        |  0.4778027  |  **0.001**  |
|    **Environmental Variable and Site**   |  0.02098092 |    0.315    |
|  **Environmental Variable and Habitat**  |  0.1089352  |  **0.007**  |
| **Environmental Variable and Parameter** |  0.7802082  |  **0.001**  |

When accounting for parameter, site and habitat were significant. When accounting for environmental variable, only habitat was significant. I included environmental variable and parameter, but I already figured that would be significant. I need to double check with Julian, but I belive conducting pairwise ANOSIMs based on the significant two-way ANOSIM results will help me understand what is driving the significant results.

I also visualized my ordination [here](https://render.githubusercontent.com/view/pdf?commit=db25c4adacf82e173c9804df28b3eacc12203e09&enc_url=68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f646232356334616461636638326531373363393830346466323862336561636331323230336530392f616e616c797365732f444e525f53524d5f32303137303930322f323031372d31312d31352d456e7669726f6e6d656e74616c2d446174612d616e642d42696f6d61726b65722d416e616c797365732f323031372d31322d31332d456e7669726f6e6d656e74616c2d446174612d5175616c6974792d436f6e74726f6c2f323031382d31312d32382d456e7669726f6e6d656e74616c2d446174612d4e4d44532e706466&nwo=RobertsLab%2Fproject-oyster-oa&path=analyses%2FDNR_SRM_20170902%2F2017-11-15-Environmental-Data-and-Biomarker-Analyses%2F2017-12-13-Environmental-Data-Quality-Control%2F2018-11-28-Environmental-Data-NMDS.pdf&repository_id=68737231&repository_type=Repository#d4f2da2b-3e35-4545-a102-bde16a74d9d0).

<img width="811" alt="screen shot 2018-11-29 at 11 08 28 am" src="https://user-images.githubusercontent.com/22335838/49245430-20da0280-f3c7-11e8-9b7b-40ba9351bf70.png">

**Figure 3**. Environmental data NMDS. Means are outlined in solid lines, variances are outlined in dashed lines.

### Constrained ordination

In [this R Markdown file](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2018-11-27-Protein-Environment-Constrained-Ordination.Rmd), I conducted a constrained ordination to look at relationships in protein abundance based on my environmental data. To do this, I first created an environmental data matrix with mean and variance values from the entire outplant, and matched them with my objects, oyster sample IDs. This meant I had the same objects in both my protein abundance and environmental data matrices. I calculated the gradient length and found that the underlying model was linear, so I used an RDA. I specified `na.action = na.exclude` in my `rda` function so it could handle my missing values.

**Table 3**. Variance explained by constrained ordination. The RDA explains 29.21% of all variance.

| **Variance Partition** | **Inertia** | **Proportion** |
|:----------------------:|:-----------:|:--------------:|
|        **Total**       |   0.015616  |        1       |
|     **Constrained**    |   0.004561  |     0.2921     |
|    **Unconstrained**   |   0.011055  |     0.7079     |

**Table 4**. ANOVA results for overall RDA analysis. With F(6,19) = 1.3064 and p-value = 0.195, the RDA is not significant.

| **Partition** | **Df** | **Variance** |  **F** | **Pr(>F)** |
|:-------------:|:------:|:------------:|:------:|:----------:|
|     Model     |    6   |   0.0045607  | 1.3064 |    0.195   |
|    Residual   |   19   |   0.0110550  |        |            |

**Table 5**. Significance of each RDA axis. No axis is significant.

| **Axis** | **Df** | **Variance** |  **F** | **Pr(>F)** |
|:--------:|:------:|:------------:|:------:|:----------:|
|   RDA1   |    1   |   0.0023293  | 4.0033 |    0.316   |
|   RDA2   |    1   |   0.0013380  | 2.2995 |    0.603   |
|   RDA3   |    1   |   0.0003888  | 0.6682 |    0.998   |
|   RDA4   |    1   |   0.0003065  | 0.5268 |    0.994   |
|   RDA5   |    1   |   0.0001395  | 0.2397 |    0.999   |
|   RDA6   |    1   |   0.0000586  | 0.1008 |    0.999   |

**Table 6**. Significance of each environmental descriptor in the RDA. Temperature mean and variance are marginally significant predictors.

| **Environmental Variable** | **Df** | **Variance** |  **F** | **Pr(>F)** |
|:--------------------------:|:------:|:------------:|:------:|:----------:|
|      Temperature Mean      |    1   |   0.0013542  | 2.3275 |  **0.065** |
|    Temperature Variance    |    1   |   0.0012584  | 2.1627 |  **0.067** |
|           pH Mean          |    1   |   0.0003063  | 0.5264 |    0.781   |
|         pH Variance        |    1   |   0.0007082  | 1.2171 |    0.301   |
|    Dissolved Oxygen Mean   |    1   |   0.0006024  | 1.0354 |    0.349   |
|  Dissolved Oxygen Variance |    1   |   0.0003312  | 0.5692 |    0.725   |
|          Residual          |   19   |              |        |            |

One weird thing I noticed is that my salinity variables do not show up in Table 6 or my ordinations below. That's something I'll have to figure out with Julian. Other than that, it's interesting to note that my RDA really isn't significant, which fits the loose interpretation we had earlier of environment affecting protein abundance in the manuscript.

To visualized my contrained ordination, I included all proteins and environmental descriptors [here](https://render.githubusercontent.com/view/pdf?commit=db25c4adacf82e173c9804df28b3eacc12203e09&enc_url=68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f646232356334616461636638326531373363393830346466323862336561636331323230336530392f616e616c797365732f444e525f53524d5f32303137303930322f323031372d31302d31302d54726f75626c6573686f6f74696e672f323031372d31312d30352d496e74656772617465642d446174617365742f323031382d31312d32382d5244412d506c6f742e706466&nwo=RobertsLab%2Fproject-oyster-oa&path=analyses%2FDNR_SRM_20170902%2F2017-10-10-Troubleshooting%2F2017-11-05-Integrated-Dataset%2F2018-11-28-RDA-Plot.pdf&repository_id=68737231&repository_type=Repository#54e0a088-4621-42c6-9b78-62fc1bdba08e), and only significant proteins and environmental descriptors [here](https://render.githubusercontent.com/view/pdf?commit=db25c4adacf82e173c9804df28b3eacc12203e09&enc_url=68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f526f62657274734c61622f70726f6a6563742d6f79737465722d6f612f646232356334616461636638326531373363393830346466323862336561636331323230336530392f616e616c797365732f444e525f53524d5f32303137303930322f323031372d31302d31302d54726f75626c6573686f6f74696e672f323031372d31312d30352d496e74656772617465642d446174617365742f323031382d31312d32382d5244412d506c6f742d5369676e69666963616e742d4f6e6c792e706466&nwo=RobertsLab%2Fproject-oyster-oa&path=analyses%2FDNR_SRM_20170902%2F2017-10-10-Troubleshooting%2F2017-11-05-Integrated-Dataset%2F2018-11-28-RDA-Plot-Significant-Only.pdf&repository_id=68737231&repository_type=Repository#696ff0db-bf94-4f88-94f0-87dbbd2e4e9d).

<img width="823" alt="screen shot 2018-11-29 at 11 06 57 am" src="https://user-images.githubusercontent.com/22335838/49245400-07d15180-f3c7-11e8-8c19-319decb1bbae.png">

<img width="820" alt="screen shot 2018-11-29 at 11 07 06 am" src="https://user-images.githubusercontent.com/22335838/49245401-0869e800-f3c7-11e8-9032-4c6bf7076262.png">

**Figures 4-5**. RDA visualization including all proteins and descriptors (top) and only those that are significant (bottom).

### Going forward

1. Discuss all results with Julian
2. Create a better ordination visual
3. Update manuscript with new methods and results
4. Tweak discussion

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
