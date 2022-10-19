---
layout: post
comments: true
title: CEABiGR Part 6
tags: virginica CEABiGR mixOmics mox
---

## Correlating gene methylation and expression

Time to return to `mixOmics`! Since we don't have any differentially expressed genes or transcripts associated with pH treatment for each sex, we cannot match methylation and expression information easily. After discussing with Steven and Sam, our goal is to use `mixOmics` to run a SPLS with gene methylation and FPKM to determine if there is any correlation between the two datasets. I did my work in [this R Markdown script](https://github.com/sr320/ceabigr/blob/main/code/gene-methylation-expression-mixomics.Rmd).

### `mixOmics` filtering and versions

I first ran an SPLS on all data combined to demonstrate a need to consider male and female data separately. As expected, the plots showed clear separation between sexes! After separating data by sex and refiltering to remove genes with a range of FPKM < 10 across all samples, I tried to tune a female-specific SPLS. When I tried to determine the number of components that should be included for the analysis, I got the following error:

```{r}
genePerf.plsFem <- perf(geneResult.plsFem, validation = "Mfold", folds = 2,
                     progressBar = FALSE, nrepeat = 10) #Use perf for repeated k-fold cross-validation to determine the number of components that should be included
```

> Error in Ypred[omit, , h] <- Y.hat[, , 1] :
number of items to replace is not a multiple of replacement length

I googled the error message and found [this post on the `mixOmics` discourse forum](https://mixomics-users.discourse.group/t/pls-and-diablo-tuning/742/4). There was some discussion of the error resulting from dataframes with features with very low variance. I didn't really understand the nuances of the issue, so I looked at the [associated Github issue](https://github.com/mixOmicsTeam/mixOmics/issues/196). It seems like `mixOmics` was not functioning as expected when the input data had features with near-zero variance. The devleoper suggested a workaround for this issue by downloaded a developer version of the package and proceeding. Before downloading a new `mixOmics` version, I decided to try updating my filtering. I used `rowVars` to calculate variance across samples for each gene, then retained genes with variance > 5:

```{r}
#Transpose expression data
#Remove rows with missing data
#Rowwise range operation
#Remove rows with near zero variance
geneExpressionFilt <- geneExpressionMod %>%
  t(.) %>%
  as.data.frame(.) %>%
  drop_na(.) %>%
  mutate(var = matrixStats::rowVars(as.matrix(.))) %>%
  filter(., var > 5) %>%
  select(., !var) %>%
  t(.) %>%
  as.data.frame(.)
head(geneExpressionFilt) #Confirm formatting
sum(is.na(geneExpressionFilt)) #Confirm there are no NAs
ncol(geneExpressionFilt) #Number of genes remaining
```

Filtering on variance alone did not prevent me from encountering the same error, so I installed the developer version of the package:

```{r}
require(devtools)
install_github("mixOmicsTeam/mixOmics", ref = github_pull("197"), force = TRUE) #Install dev version: https://github.com/mixOmicsTeam/mixOmics/issues/196
require(mixOmics)
```

When I tried running `perf` with five iterations, I didn't get the error message! I did, however, freeze R Studio so I decided to move to the R Studio server on `mox` to keep testing.

### Setting up R Studio server

An interlude to do something I really didn't like the first time. Following the instructions in the [Roberts Lab Handbook](https://robertslab.github.io/resources/mox_RStudio-Server/), the first thing I did was modify the parameters of my singularity container. I found the `singularity` definition file I used the last time I needed the R Studio server in my home directory. I modified my `r_packages_installs.R` to install `devtools`, `tidyverse` and `plotly`:

```
# Update base packages
update.packages(ask = FALSE)

# Install BioConductor package manager
if (!requireNamespace("BiocManager", quietly = TRUE))
install.packages("BiocManager")

# Install devtools
install.packages("devtools")

#Install mixOmics
install_github("mixOmicsTeam/mixOmics", ref = github_pull("197"), force = TRUE) #Install dev version: https://github.com/mixOmicsTeam/mixOmics/issues/196

# Install tidyverse
install.packages("tidyverse")

# Install plotly
install.packages("plotly")
```

Then I logged into a build note, loaded the `singularity` module, and built the container. I initially included the `install_github` command to install `mixOmics`. However, I kept getting errors stating that the `singularity` container failed to build because it could not find the command `install_github`:

> Error in install_github("mixOmicsTeam/mixOmics", ref = github_pull("197"),  :
  could not find function "install_github"

I tried  adding `require(devtools)` after installation to see if I could get the package to already load. It worked!

```
Loading required package: devtools
Loading required package: usethis
Warning message:
package 'devtools' was built under R version 4.0.3
```

Then I tried just installing `devtools`, loading it, and using `install_github`. It worked up until the very last second:

```
Installing package into '/usr/local/lib/R/site-library'
(as 'lib' is unspecified)
* installing *source* package 'mixOmics' ...
** using staged installation
It is recommended to use 'given' instead of 'middle'.
** R
** data
** inst
** byte-compile and prepare package for lazy loading
Error in dyn.load(file, DLLpath = DLLpath, ...) :
  unable to load shared object '/usr/local/lib/R/site-library/igraph/libs/igraph.so':
  libglpk.so.40: cannot open shared object file: No such file or directory
Calls: <Anonymous> ... namespaceImport -> loadNamespace -> library.dynam -> dyn.load
Execution halted
ERROR: lazy loading failed for package 'mixOmics'
* removing '/usr/local/lib/R/site-library/mixOmics'
Error: Failed to install 'mixOmics' from GitHub:
  (converted from warning) installation of package '/tmp/RtmpumGdlP/file1bd4945374c/mixOmics_6.19.4.tar.gz' had non-zero exit status
Execution halted
```

I'm not sure what this means, so I decided to try `install_github` in the R Studio server itself instead of building the container with it. Once I had the container, I input container path and name into a SLURM script to create a tunnel to the R Studio server and get log in credentials. This worked smoothly! When I logged into the R Studio server, I realized I didn't have a good way to clone the CEABiGR `git` repository onto the server. I could just upload and download files with `rsync`, but I wanted to be able to push directly from the server to my repository.

I added the following code to the `singularity` definition file based on [this example](https://gist.github.com/WeipengMO/e1549c9f024a4d2f27d5db53b853c383):

```
%post
    # Install additinoal system packages in container
    # Most are needed for R/RStudio dependencies
    apt -y update
    apt -y install libxml2 libz-dev libbz2-dev liblzma-dev libxtst6 git
    git	clone --recursive https://github.com/sr320/ceabigr.git
```

That...did something! I then created the tunnel and logged into the R Studio server. Even though I saw the repository was cloned, it was unclear where it went. I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1525) to get help.

### Going forward

1. Tune sPLS parameters
3. Run sex-specific SPLS
4. Identify drivers
1. Update methods and results
2. Read gene expression variability papers
2. Continue with gene expression variability analysis

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
