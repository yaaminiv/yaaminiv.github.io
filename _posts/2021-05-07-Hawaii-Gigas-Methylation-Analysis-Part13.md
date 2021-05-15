---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 13
tags: hawaii gigas-ploidy methylKit
---

## Comparative analysis with `methylKit`

It's time to (finally) run `methylKit` with the Hawaii samples!

### Setting up R Studio server

I opened [my R Markdown script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit.Rmd) and edited it based on my experiences with the Manchester samples. First, I removed the randomization test code, since I [decided to run it separately without the R Studio server](https://yaaminiv.github.io/WGBS-Analysis-Part24/). I also updated my code to reshuffle treatments based on [my new understanding of `reorganize`](https://yaaminiv.github.io/WGBS-Analysis-Part19/).

Once I had my code updated, I created [this SLURM script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit-RStudio.sh) to generate the R Studio session. Sam suggested I update the code that calls the Singularity container to the following to remove any timeouts, automatic sign-outs, and keep me signed in for 30 days:

```
# This example bind mounts the /gscratch/scrubbed/${USER} directory on the host into the Singularity container.
# By default the only host file systems mounted within the container are $HOME, /tmp, /proc, /sys, and /dev.
singularity exec \
--bind="$TMPDIR/var/lib:/var/lib/rstudio-server" \
--bind="$TMPDIR/var/run:/var/run/rstudio-server" \
--bind="$TMPDIR/tmp:/tmp" \
--bind=/gscratch/scrubbed/${USER} \
${container_path}/${container} \
rserver --www-port ${PORT} --auth-none=0 --auth-pam-helper-path=pam-helper --auth-stay-signed-in-days=30 --auth-timeout-minutes=0
```

I updated that code chunk, copied the coverage files from `gannet` to `mox`, and ran the SLURM script! I immediately encountered an error with the R Studio tunnel:

![Screen Shot 2021-05-07 at 11 24 15 AM](https://user-images.githubusercontent.com/22335838/117492658-c2fe5d00-af26-11eb-95cc-7786cdf4a72c.png)

I realized there was an error in my SLURM output that killed the script!

![Screen Shot 2021-05-07 at 11 23 51 AM](https://user-images.githubusercontent.com/22335838/117492624-b548d780-af26-11eb-8ad2-9b773e7e6528.png)

I commented on this [discussion](https://github.com/RobertsLab/resources/discussions/1196#discussioncomment-711203) to see if Sam could help. When I compared my [Manchester R Studio SLURM script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit-RStudio.sh) with this one, I realized that I was using `{container}`: a variable that did not exist in my script! I replaced `${container_path}/${container}` with my actual container path and successfully established the tunnel.

### Setting root directory

With my R markdown script on the R Studio server, I started with the processing the coverage files. When I tried running `methRead`, I realized my root directory was set to be where my R Markdown file was located: my home directory/login node. Since I was using an R Markdown file, I couldn't just use `setwd()` to change my working directory to `/gscratch/scrubbed/yaaminiv/Hawes/analyses/methylKit`. After a quick Google Search, I learned I could change the root directory in my `knitr` set up chunk:

```
knitr::opts_chunk$set(echo = TRUE)
knitr::opts_knit$set(root.dir = "/gscratch/scrubbed/yaaminiv/Hawes/analyses/methylKit/") #Set root directory
```

Once I had the root directory set, I could process the coverage file with `methRead`. This change also made my downstream code much cleaner, since I didn't need a `/gscratch/scrubbed/yaaminiv/Hawes/analyses/methylKit` prefix before each file path!

### Sample-specific descriptive statistics and comparative analysis

Before uniting the data, I examined percent CpG coverage and methylation in each sample (figures [here](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_04-methylKit/general-stats)). There are more differences in coverage between samples, but I didn't see any samples that looked wildly different. Methylation distribution looked uniform for all samples.

Next, I united the data and created a [correlation plot](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/general-stats/Full-Sample-Pearson-Correlation-Plot-FilteredCov5Destrand.jpeg), [clustering diagram](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/general-stats/Full-Sample-CpG-Methylation-Clustering-FilteredCov5Destrand.jpeg), and [PCA](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_04-methylKit/general-stats/Full-Sample-Methylation-PCA-FilteredCov5Destrand.jpeg).

<img width="683" alt="Screen Shot 2021-05-15 at 1 10 16 PM" src="https://user-images.githubusercontent.com/22335838/118376800-0d16bc80-b57f-11eb-9621-2260bbd52769.png">

**Figure 1**. Correlation plot

<img width="683" alt="Screen Shot 2021-05-15 at 1 10 46 PM" src="https://user-images.githubusercontent.com/22335838/118376804-12740700-b57f-11eb-8def-ed46b3ae97f8.png">

**Figure 2**. Clustering diagram

<img width="683" alt="Screen Shot 2021-05-15 at 1 10 58 PM" src="https://user-images.githubusercontent.com/22335838/118376805-130c9d80-b57f-11eb-85c3-24f0d387cf9e.png">

**Figure 3**. PCA

Correlations between samples were > 86% across the board, even between diploid and triploid oysters, and there wasn't a clear separation of samples by pH treatment or ploidy when looking at the clustering diagram. There was a fair amount of chaining on the clustering diagram, but 3H-2 separated out early from the rest of the samples. When I looked at the PCA, 3H-2 looked like an outlier! 2H-3, 3H-3, and 2H-1 were also farther apart from the tight cluster of other samples. 3H-2 and 3H-3 separated from the main cluster along PC1, while 2H-1 and 2H-3 separated along PC2. I wonder if I can expect more variability in oysters from the high pH (control) treatment because there wasn't a stressor present, but if that variation could be different depending on ploidy status. Each ploidy x pH combination had six samples, so a third of the samples from 2H and 3H seeming like outliers is interesting! I reviewed my [`bismark` output](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part10/) to see if there were any mapping variables that potentially impacted the PCA. I noticed that 2H-1 had the lowest % mCpG, and 3H-2 and 3H-3 had similar % mCpG. However, I couldn't pick out any differences in M C's, % duplicates, or % aligned.

### Obtaining DML (and more R Studio server issues)

I ran the following code chunk to calculate methylation differences at each locus...

```{r}
differentialMethylationStatsPloidy <- methylKit::calculateDiffMeth(methylationInformationFilteredCov5,
                                                                   covariates = covariatepH,
                                                                   overdispersion = "MN", test = "Chisq") #Calculate differential methylation statistics based on treatment indication from methRead. Include pH as a covariate. Use 40 cores.
head(differentialMethylationStatsPloidy) #Look at differential methylation statistics
```

and it worked! But then R Studio froze before I could save the data or extract DML information. The saga continues.

I tried closing the SSH tunnel, reopening it, and loading R Studio. I also tried cancelling my SLURM script, running it again, opening a new SSH tunnel, and opening R Studio. Each time I got the same result! My R Markdown script loaded and after a few seconds of not being able to interact with the session, I got the following error:

![Screen Shot 2021-05-08 at 1 54 33 PM](https://user-images.githubusercontent.com/22335838/117553327-bf8dd300-b005-11eb-9f7a-a2ed2d3964b6.png)

I've tried pressing "Wait", "Exit Page", and doing nothing. No matter what, I'm kicked out of the session:

![Screen Shot 2021-05-08 at 1 54 59 PM](https://user-images.githubusercontent.com/22335838/117553331-c3215a00-b005-11eb-9299-b377a1bb4add.png)

This is similar to something I mentioned with the emu and roadrunner R studio servers in [this discussion](https://github.com/RobertsLab/resources/discussions/1137#discussioncomment-449994). So obviously I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1204) to record yet another struggle for posterity.

I tried again, and immediately closed my R Markdown script. After the script was closed, I was able to interact with the R Studio session! I deleted the R Markdown script and added it again to `mox` with `rsync`, and got the same "Page Unresponsive error". When I created an [analagous R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/04-methylKit.R) and opened it, I was able to interact with the R Studio session! I think I need a way to clear the R Studio server cache associated with the R Markdown file. Steven mentioned in local R Studio, you can change settings to not open prior scripts. I'll investigate that option once I figure out if my code (now running on the R Studio server in my regular R script) runs, if I'm able to save my R data, and if I can interact with the server afterwards!

### Going forward

1. Try [EpiDiverse/snp](https://github.com/EpiDiverse/snp) for SNP extraction from WGBS data
1. Run `methylKit` script on `mox`
5. Investigate comparison mechanisms for samples with different ploidy in oysters and other taxa
4. Test-run [DSS](http://bioconductor.org/packages/release/bioc/vignettes/DSS/inst/doc/DSS.html#34_DMLDMR_detection_from_general_experimental_design) and [ramwas](https://bioconductor.org/packages/release/bioc/html/ramwas.html)
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)
6. Update methods
7. Update results

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
