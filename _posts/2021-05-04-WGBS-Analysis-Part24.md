---
layout: post
comments: true
title: WGBS Analysis Part 24
tags: manchester gigas-broodstock methylKit mox
---

## Troubleshooting R Studio server session timeouts

[Last week](https://yaaminiv.github.io/WGBS-Analysis-Part23/), I was able to run [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html) on the `mox` R Studio server! It worked great for identifying DML, but I ran into some issues with my randomization tests. I set up my randomization tests to run 1000 iterations of `calculateDiffMeth` and `getMethylDiff` on my samples with randomized treatment information. The purpose of the test was to determine if the number of DML I identified was greater than what would be expected by random chance. If the number of DML identified is significantly greater than random chance, it would provide more confidence in the fact that pH treatment was influencing methylation.

When I first started running the randomization test, I noticed that it took two days to make it through six iterations! Since I was only running the script on one core, I asked Sam how many cores I could use on `mox`. He suggested I use 40 cores. We pay for 28 but sometimes have access to 40, but there was no harm in listing 40. I modified my code as follows, and used `rsync` to move my updated script off of `mox` and [into my repository](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.R):

```
for (i in 1:1000) {
  pHRandTreat <- sample(sampleMetadataFem$pHTreatment,
                        size = length(sampleMetadataFem$pHTreatment),
                        replace = FALSE)
  pHRandDML <- methylKit::reorganize(methylationInformationFilteredCov5Fem,
                                     sample.ids = sampleMetadataFem$sampleID,
                                     treatment = pHRandTreat) %>%
    methylKit::calculateDiffMeth(., covariates = covariateMetadataFem,
                                 overdispersion = "MN",
                                 test = "Chisq", mc.cores = 40) %>%
    methylKit::getMethylDiff(., difference = 25, qvalue = 0.01) %>%
    nrow() -> pHRandTest25Fem[i]
}
```

I ran this test overnight and returned to check back on it. Since R Studio sessions time out after 60 minutes, I needed to log back in. However, the login window wasn't loading for me! I posted [this dicussion](https://github.com/RobertsLab/resources/discussions/1196) to see if there was anything I could do besides kill my R Studio session. Sam suggested I close out the session occupying the port with `lsof -ti:8787 | xargs kill -9`, then try establishing the tunnel and logging in. However, that didn't work. I tried closing my tunnel and restarting it several times, but no luck. I even let the log in window sit overnight to see if I could get it to load! I ended up cancelling my SLURM job and as per Sam's suggestion, added the following code to my [R Studio server SLURM script]() to prevent sessions from timing out:

```
module load singularity #Existing line to load session
# Do not suspend idle sessions.
# Alternative to setting session-timeout-minutes=0 in /etc/rstudio/rsession.conf
# https://github.com/rstudio/rstudio/blob/v1.4.1106/src/cpp/server/ServerSessionManager.cpp#L126
export SINGULARITYENV_RSTUDIO_SESSION_TIMEOUT=0
```

I loaded the R Studio session again and started running the randomization test again. The session used to time out after an hour, so I left the session running and checked back after an hour! Turns out, I still got logged out, since R Studio logs you out after 60 minutes of inactivity. I put in my username and password in, but when I pressed the "sign in" button I wasn't taken to my R Studio session! I let it load overnight, and when I came back in the morning I saw this screen:

![Screen Shot 2021-05-05 at 9 21 08 AM](https://user-images.githubusercontent.com/22335838/117175208-6f074300-ad83-11eb-90d3-fa09e7b5547a.png)

I'm not entirely sure what "Safe Mode" is, but I clicked on it anyways. After ~5 hours of waiting for Safe Mode, I opted to terminate R. And of course, R didn't actually terminate! So then I killed my SLURM script.

Moving forward, I think I will move my randomization tests to a separate R script, and call that R script from a SLURM script like I've done previously. Now that I know these are no typos in my script (*knocks on wood*), I think I can run the randomization test separately for both datasets after I obtain DML. I'll use [additional code from Sam](https://github.com/RobertsLab/resources/discussions/1196#discussioncomment-696240) next time I run the R Studio server for something else!

### Going forward

1. Finish running `methylKit` randomization tests on `mox`
2. Write methods
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
