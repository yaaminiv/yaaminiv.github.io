---
layout: post
comments: true
title: WGBS Analysis Part 22
tags: manchester gigas-broodstock methylKit mox
---

## Running R scripts on `mox` (for real this time)

So, clearly things aren't going well. I [tried running an R script on `mox`](https://yaaminiv.github.io/WGBS-Analysis-Part21/), but landed in a seemingly endless loop of installing dependencies, running the script, having it fail, and trying to install yet another dependency. My theory was that the mechanism of loading R packages in a SLURM script is different than loading packages in an R module. Time to test it out.

### Sanity check with the build node

Since I was able to load packages in the build node, I thought I would see if I could run part of my code interactively as a sanity check. First, I opened a build node for four hours:

```
srun -p build --time=4:00:00 --mem=10G --pty /bin/bash
module load r_3.6.0 #Load R module
R #Open R
```

Then, I loaded the `devtools`, `methylKit`, and `dplyr` packges, confirmed packages were loaded, and set my working directory to folder with my `bismark` output:

```{r}
require(devtools)
require(methylKit)
require(dplyr)
sessionInfo() #Confirm packages are loaded
```

![Screen Shot 2021-04-26 at 11 05 42 AM](https://user-images.githubusercontent.com/22335838/116159731-997f3f00-a6a5-11eb-9ad5-60ddb1e24d3b.png)

```{r}
getwd() #Confirm I am in my home directory
setwd("/gscratch/scrubbed/yaaminiv/Manchester/analyses/methylKit") #Change directory to where bismark output is
```

I then started running code to confirm that `methylKit` and `dplyr` R commands would work as long as the packages were loaded. I was able to quickly read files into R with `methRead`:

![Screen Shot 2021-04-26 at 11 10 09 AM](https://user-images.githubusercontent.com/22335838/116159809-c2073900-a6a5-11eb-9cae-7a4065ca5d4c.png)

I then successfully ran the following code chunk to process `bismark` alignments and normalize coverage between samples!

```{r}
processedFilteredFilesCov5 <- methylKit::filterByCoverage(processedFiles,
                                                          lo.count = 5, lo.perc = NULL,
                                                          high.count = NULL, high.perc = 99.9) %>%
  methylKit::normalizeCoverage(.)
```

![Screen Shot 2021-04-26 at 11 16 03 AM](https://user-images.githubusercontent.com/22335838/116159877-dea37100-a6a5-11eb-9a57-14d56837c5cb.png)

At this point, I saved my R data and knew that as long as I could reference my packages correctly, I could run my code.

### Calling an R Script in a SLURM script

Clearly calling an R module worked better than changing the shebang and running an R script directly on `mox`! I wanted to try another method: calling an R script within a SLURM script. First, I needed to put my R code in a separate script. I copied and pasted my code and created [this R script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.R). Based on the [hyak documentation](https://wiki.cac.washington.edu/display/hyakusers/Hyak+R+programming), I needed to create a SLURM script to call the R script. For [this SLURM script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit-SLURM.sh), I used a 10 day walltime and 100 G memory node. Hopefully I won't need more than that! Within the script, I needed two lines of code:

```
module load r_3.6.0 #Load R version 3.6.0
Rscript > output.txt 2>&1 /gscratch/home/yaaminiv/06-methylKit.R #Specify my standard error file (output.txt) and R script location (/gscratch/home/yaaminiv/06-methylKit.R)
```

I then ran my SLURM script. It ended after 20 minutes (so...a bit longer than the 18 minute run I was used to previously!) due to more package struggles. Thankfully they were different package problems than before! I was unable to load `devtools` or `dplyr` because R could not find the correct versions of dependency packages. However, `methylKit` loaded with no issues:

![Screen Shot 2021-04-26 at 1 09 12 PM](https://user-images.githubusercontent.com/22335838/116161499-b406e780-a6a8-11eb-968d-750cddcdfd8f.png)

![Screen Shot 2021-04-26 at 1 09 29 PM](https://user-images.githubusercontent.com/22335838/116161501-b49f7e00-a6a8-11eb-89ae-9b0dc6e70d62.png)

![Screen Shot 2021-04-26 at 1 09 39 PM](https://user-images.githubusercontent.com/22335838/116161502-b5381480-a6a8-11eb-8ae7-6fa5458bcbec.png)

I finagled with how I loaded packages in my R script and decided to run `require(devtools)` with no `lib.loc` argument. When I would load packages in the build node, I never specified where packages were found and did not encounter any error. I also tried `require(tidyverse, lib.loc = "/gscratch/srlab/rpackages")` to see if loading `tidyverse` would bypass any issues I had loading `dplyr`. I was able to load `devtools` with no problems, but still ran into issues with `dplyr` and `tidyverse`!

![Screen Shot 2021-04-26 at 1 17 01 PM](https://user-images.githubusercontent.com/22335838/116161851-650d8200-a6a9-11eb-917a-8c59d055ec01.png)

![Screen Shot 2021-04-26 at 1 17 12 PM](https://user-images.githubusercontent.com/22335838/116161852-663eaf00-a6a9-11eb-9f42-f84c3eeddde6.png)

![Screen Shot 2021-04-26 at 1 23 45 PM](https://user-images.githubusercontent.com/22335838/116161854-663eaf00-a6a9-11eb-942f-3f6303898a66.png)

Since `devtools` loaded without specifying a library location, I figured I could do the same for `dplyr`. The final configuration of loading R packages that worked went as follows:

```{r}
require(devtools) #Load devtools
require(methylKit, lib.loc = "/gscratch/home/yaaminiv/R/x86_64-pc-linux-gnu-library/3.6/") #Load methylKit. I was able to load with no issues including library location, so I didn't change it
require(dplyr) #Load devtools
sessionInfo() #Confirm packages are loaded
```

At this point, my script truly ran...for 30 minutes! I didn't properly reference my covariate matrix in my `calculateDiffMeth` command, but once I did that the command ran without any issues (so far). Guess we'll wait and see if I can indeed run `calculateDiffMeth` with a covariate matrix and overdispersion correction on `mox`!

![Screen Shot 2021-04-26 at 4 19 39 PM](https://user-images.githubusercontent.com/22335838/116162770-4e682a80-a6ab-11eb-922e-ae1c2eec34bc.png)

![Screen Shot 2021-04-26 at 4 19 58 PM](https://user-images.githubusercontent.com/22335838/116162765-4d36fd80-a6ab-11eb-92f1-3a64c6ffa990.png)

### Going forward

1. Write methods
3. Write results
2. Update `mox` handbook with R information
2. Obtain relatedness matrix and SNPs with [EpiDiverse/snp](https://github.com/EpiDiverse/snp)
4. Identify genomic location of DML
2. Determine if RNA should be extracted
3. Determine if larval DNA/RNA should be extracted
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
