---
layout: post
comments: true
title: WGBS Analysis Part 23
tags: manchester gigas-broodstock methylKit mox
---

## `methylKit` with R Studio server on `mox`

TL;DR I can run `calculateDiffMeth` and get DML! Please enjoy this saga of troubleshooting and not reading errors properly.

Alas, after running for five hours my R SLURM script failed *cries*. When I looked at the SLURM output, I saw the following error:

![Screen Shot 2021-04-27 at 9 33 51 AM](https://user-images.githubusercontent.com/22335838/116278774-ae0e1680-a73b-11eb-915d-6a16f7173730.png)

`calculateDiffMeth` was giving me problems again! I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1188) to get input from Sam and Steven about what to do next.

### Revisiting the build node

In the meantime, I decided to revisit the build node. I requested a build node and loaded R:

```
srun -p build --time=4:00:00 --mem=10G --pty /bin/bash #Request a build node for four hours
module load r_3.6.0 #Load R version 3.6.0
R #Start R
```

I loaded my R data and ran `calculateDiffMeth` without a covariate matrix or overdispersion correction. It finished running and I ran `warnings()`. They were all `glm.fit` errors, which I've encountered before. I was able to produce a `methylDiff` object, so that's a good sign!

![Screen Shot 2021-04-27 at 1 00 47 PM](https://user-images.githubusercontent.com/22335838/116463549-a9bb2980-a81f-11eb-9c2c-85b420709de6.png)

![Screen Shot 2021-04-27 at 1 01 23 PM](https://user-images.githubusercontent.com/22335838/116464458-d9b6fc80-a820-11eb-99dd-a05ac7ca5b08.png)

I then tried running `calculateDiffMeth` with the covariate matrix and overdispersion correction. Around the four hour mark, my command timed out. I initially thought it was related to `calculateDiffMeth`, but I then I realized it was probably because the four hour reservation ended! In any case, it was ready for me to move onto a different option: R Studio server.

### Troubleshooting R Studio server

When I started running `calculateDiffMeth` on the build node, I also started working through the R Studio server Sam set up on `mox`. I followed the instructions he laid out in [this discussion](https://github.com/RobertsLab/resources/discussions/1180#discussioncomment-651295). To run R Studio server, I needed to change my R library directory information in `~/.Renviron`, then run a SLURM script to get R Studio login credentials. I could then tunnel into `mox` in a separate Terminal window to run R Studio.

When I went to change my `~/.Renviron` library location, I realized I didn't have a `~/.Renviron` file to begin with! Sam said I should make one, so I did.

![Screen Shot 2021-04-28 at 1 28 24 PM](https://user-images.githubusercontent.com/22335838/116468560-da05c680-a825-11eb-9a3e-9d4539e6d8ad.png)

I created [this script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit-RStudio.sh) to get R Studio log in credentials. After running it, I was unable to tunnel into `mox` and access the R Studio server! I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1190) with the specific "connection refused" error I got:

<img width="605" alt="Screen Shot 2021-04-27 at 1 02 18 PM" src="https://user-images.githubusercontent.com/22335838/116305568-cf313000-a758-11eb-81b8-47af5c376f10.png">

Turns out I misinterpreted Sam's original script, and didn't include any of the important things that started the R Studio server session! I added the script sections I needed to, then ran it again and was able to access R Studio server. I found [my R Script]() and started running `calculateDiffMeth`. According to Sam, the session should still run even if I close the window.

Later at night, I decided to test that theory. When I closed the R Studio server browser on my local machine, I got an error in my Terminal window. I logged back into R Studio server on `genefish`, I saw the following error:

<img width="733" alt="Screen Shot 2021-04-27 at 10 34 46 PM" src="https://user-images.githubusercontent.com/22335838/116351578-c6684a80-a7a8-11eb-8655-b981b8db00dd.png">

It seemed like `calculateDiffMeth` finished running, but there was an "error writing to connection" that I didn't understand. I set up `calculateDiffMeth` to run through the night. I saw that I was logged out after 60 minutes due to inactivity. When I logged back in, I saw a slew of error messages:

![Screen Shot 2021-04-28 at 11 26 07 AM](https://user-images.githubusercontent.com/22335838/116454261-a4a4ad00-a814-11eb-8ccb-c89cf5516c4a.png)

1. `There were 50 or more warnings (use warnings() to see the first 50)`: Checked warnings(), they're glm.fit errors (not related to the issue at hand). At least I know `calculateDiffMeth` ran!

![Screen Shot 2021-04-28 at 11 26 30 AM](https://user-images.githubusercontent.com/22335838/116454391-caca4d00-a814-11eb-989c-26071f27073d.png)

2. `Error in h(simpleError(msg, call)) :
  error in evaluating the argument 'x' in selecting a method for function 'head': object 'differentialMethylationStatsTreatment' not found`: My fault, didn't reference the correct data frame. When I referenced the correct `calculateDiffMeth` output, the command worked!

3.  Same `error writing to connection` error! However, it   my `calculateDiffMeth` command DID finished running, and my global environment was loaded again when I logged back into the R Studio server.

Sam investigated it further and saw that my home directory may be full, leading to errors saving any R Studio output, like `~/.rstudio` or `~/.Rdata` in that directory. I was saving my R data in `/gscratch/scrubbed/yaaminiv/Manchester/analyses/methylKit`, but it's possible that the large `methylKit` objects were causing resulting in a `~/.rstudio` file. Interestingly, my global environment was loading each time I logged into the R Studio server. I checked my login node quota and ruled out that it was a storage issue on my end! Since the R Studio server was still working for me and I was able to save output to my scrubbed directory, I kept going.

### The bottom line

The force termination was likely because the build node timed out because I only requested one for four hours, and the `head` command error was because I was incorrectly calling the dataframe. Thanks to R Studio server, I could confirm that this was the case and ensure that the warnings were not devastating! I updated my "connection refused" discussion [with explanations of my error messages](https://github.com/RobertsLab/resources/discussions/1190#discussioncomment-671472), and posted an update in my [original discussion about `calculateDiffMeth`](https://github.com/RobertsLab/resources/discussions/1188#discussioncomment-671554).

**But the best part was that I ran `getMethylDiff` AND I HAVE DML**! I'll post about that in a separate notebook entry.

So really, it was [a typo all along](https://www.youtube.com/watch?v=P8u8md-NiHM&t=3s).

### Going forward

1. Finish running `methylKit` on R Studio server
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
