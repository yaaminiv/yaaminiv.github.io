---
layout: post
comments: true
title: WGBS Analysis Part 21
tags: manchester gigas-broodstock methylKit mox
---

## Running R scripts on `mox`

Alright, I have [R installed](https://yaaminiv.github.io/WGBS-Analysis-Part20/), which is maybe a moot point but I couldn't get [`methylKit`](https://bioconductor.org/packages/release/bioc/vignettes/methylKit/inst/doc/methylKit.html) installed. Let's see if I can actually run an R SLURM script today.

### Installing packages (round 2)

To install `methylKit`, I decided to use an older version of R. I first loaded the module:

```
module load r_3.6.0 #Load R version 3.6.0
R #Start running R
```

Once I had the older R version, I was able to run install `devtools`!:

```
 install.packages("devtools", lib = "/gscratch/srlab/rpackages") #Install devtools to the specified folder
 require(devtools) #Load devtools
```

 My next step was installing Bioconductor. I followed the installation instructions from the [Bioconductor website](https://www.bioconductor.org/install/):

```
install.packages("BiocManager", lib = "/gscratch/srlab/rpackages") #Install BiocManager to the specified folder
BiocManager::install(version = "3.10") #Install the correct version of BiocManager for the R version used
```

Turns out there are specific `BiocManager` versions for each R version! I used [this Bioconductor release guide](https://bioconductor.org/about/release-announcements/) to determine which `BiocManager` version I needed to install. Since I was using R.3.6.0, I could use `BiocManager` versions 3.9 or 3.10. I figured I'd use 3.10.

Finally, I installed `methylKit`:

```
BiocManager::install("methylKit") #Install methylKit
```

The package started installing! However, I got a warning that I was using too much of the CPU. That's when I realized I wasn't on a build node! I stopped the package installation, quit R, and interrupted my `mox` session. I then started a build node:

```
srun -p build --time=4:00:00 --mem=10G --pty /bin/bash #Request a build node for four hours
```

I loaded the R module again, then installed `methylKit`:

```
require(BiocManager) #Load package
BiocManager::install("methylKit") #Install methylKit
require(methylKit) #Load package
```

It worked! The last package I needed (and almost forgot about) was `dplyr`.  I ran `require(dplyr)` just to see what happened:

![Screen Shot 2021-04-21 at 10 34 00 AM](https://user-images.githubusercontent.com/22335838/115596581-1cab2a00-a28d-11eb-941a-c60de6061297.png)

The package was already installed! I closed the Terminal window, logged in and requested another build node, and ran `require(methylKit)` to ensure I wouldn't have to install the package again in my SLURM script:

![Screen Shot 2021-04-21 at 10 36 27 AM](https://user-images.githubusercontent.com/22335838/115596862-6eec4b00-a28d-11eb-8b0d-bf0322cbb2dd.png)

Since that worked too, I tried running `sessionInfo()`. Hopefully this information would be saved into my slurm-out file.

![Screen Shot 2021-04-21 at 10 37 28 AM](https://user-images.githubusercontent.com/22335838/115596964-93482780-a28d-11eb-80e2-e610abc1c991.png)

I exited R and my build node to finish up my preparation.

### File paths on `mox`

When working in R Studio, it's a lot easier for me to save files to various places, or source the data from a different folder since I can set the working directory in a chunk. For the purpose of the R SLURM script, I think it's easier to have all the data and output files in the same folder. I created a `/gscratch/scrubbed/yaaminiv/Manchester/analyses/methylKit` folder to house all relevant files. Then, I navigated to that folder and copied the merged CpG coverage files from `gannet` to `mox`:

```
rsync --archive --progress --verbose yaamini@172.25.149.226:/Volumes/web/spartina/project-gigas-oa-meth/output/bismark-roslin/*merged_CpG_evidence.cov .
```

The next thing I wanted to do was create a subdirectory structure that mirrored where I saved output files in [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit.Rmd). I usually do this within the script itself since I can switch between bash and R, but I will not be able to do that in a SLURM script. I created:

- `/gscratch/scrubbed/yamainiv/Manchester/analyses/methylKit/general-stats` for individual-sample and comparative analysis plots
- `/gscratch/scrubbed/yamainiv/Manchester/analyses/methylKit/DML` for DML lists
- `/gscratch/scrubbed/yamainiv/Manchester/analyses/methylKit/rand-test` for randomization test output

### Running the R SLURM script

All that's left to do was create the SLURM script! I copied my R Markdown script into [this SLURM script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/06-methylKit-mox.sh). Then, I ran the script. When I checked the queue (`squeue | grep "srlab"`), I found that my script wasn't running! When I looked at the SLURM information at the top of the script, I saw `SBATCH --mem=500G`. I changed it to `SBATCH --mem=100G`, and ran the script again. Unfortunately, it timed out immediately!

When I looked at the slurm.out file, I saw the following error:

<img width="589" alt="Screen Shot 2021-04-21 at 9 27 29 PM" src="https://user-images.githubusercontent.com/22335838/115655566-637a3d80-a2e8-11eb-8db1-35097e82e56d.png">

I then posted [in this discussion](https://github.com/RobertsLab/resources/discussions/1176#discussioncomment-643377) to see where I should specify `--save`, `--no-save`, or `--vanilla`. Sam responded and said my shebang should be `#!/gscratch/srlab/programs/R-3.6.2/bin/Rscript`, and not `#!/gscratch/srlab/programs/R-3.6.2/bin/R`! I changed the shebang and ran the script again.

Obviously, my script timed out again. Looking through the slurm.out, I confirmed a few things. One, any `head()` command does print to the slurm.out. Second, I got an error that `dplyr` was not available when I ran `require(dplyr)`. Additionally, there were some packages attached to `methylKit` that didn't load. I opened another build node to install `dplyr`:

```
install.packages("dplyr", lib = "/gscratch/srlab/rpackages") #Install dplyr
require(BiocManager) #Load BiocManager
install_github("al2na/methylKit", build_vignettes = FALSE, repos = BiocManager::repositories(), dependencies = TRUE) #Install more methylKit options
require(methylKit) #Check that all associated packages load
```

I then modified the script to load several packages at the top:

```
# Load packages

require(devtools)
require(BiocManager)
require(methylKit)
require(dplyr)
sessionInfo()
```

![Screen Shot 2021-04-22 at 9 43 51 AM](https://user-images.githubusercontent.com/22335838/115752740-4036a900-a34f-11eb-92e0-d96f1a30c4b7.png)

![Screen Shot 2021-04-22 at 9 42 41 AM](https://user-images.githubusercontent.com/22335838/115752575-15e4eb80-a34f-11eb-9fa9-a1b1afaa7c31.png)

Once I ran this revised script, I ran into the same error! Based on the error messages, I think R was unable to find my specified packages. ![Screen Shot 2021-04-22 at 9 43 51 AM](https://user-images.githubusercontent.com/22335838/115752740-4036a900-a34f-11eb-92e0-d96f1a30c4b7.png)

![Screen Shot 2021-04-22 at 9 42 41 AM](https://user-images.githubusercontent.com/22335838/115752575-15e4eb80-a34f-11eb-9fa9-a1b1afaa7c31.png)

I know I installed these packages, so I think they're not being installed from their actual location. `BiocManager`, `devtools`, and `dplyr` are in the `/gscratch/srlab/rpackages/` directory:

![Screen Shot 2021-04-22 at 9 47 59 AM](https://user-images.githubusercontent.com/22335838/115753238-d4087500-a34f-11eb-81f2-6a0cd9ce2cea.png)

`methylKit` is installed in `/gscratch/home/yaaminiv/R/x86_64-pc-linux-gnu-library/3.6/`:

![Screen Shot 2021-04-22 at 9 50 32 AM](https://user-images.githubusercontent.com/22335838/115753561-2ea1d100-a350-11eb-8705-07727c122f6f.png)

I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1187) to see if there was a way to reference library locations in `require()`. Why I posted this discussion before actually Googling I don't know, but Sam and I arrived at the same conclusion: include `lib.loc` in require to specify the library location. This is important especially because I have packages installed in two separate locations! I modified my script and ran it again and encountered a new error:

<img width="944" alt="Screen Shot 2021-04-23 at 10 51 03 AM" src="https://user-images.githubusercontent.com/22335838/116120419-5c4e8900-a674-11eb-9efe-2db0cfba6d27.png">

<img width="944" alt="Screen Shot 2021-04-23 at 10 51 17 AM" src="https://user-images.githubusercontent.com/22335838/116120425-5d7fb600-a674-11eb-9695-8695b5598d72.png">

Interestingly, when I loaded packages in the SLURM script, R was unable to find dependencies, even when they were installed (like `usethis`). I confirmed that these errors were precluding me from loading packages by running `sessionInfo`:

<img width="521" alt="Screen Shot 2021-04-23 at 10 58 34 AM" src="https://user-images.githubusercontent.com/22335838/116120621-9455cc00-a674-11eb-93d5-91c6faf4f2ca.png">

This began a series of installing packages, running my R script, and finding out I needed to explicitly install another dependency:

<img width="635" alt="Screen Shot 2021-04-23 at 11 31 20 AM" src="https://user-images.githubusercontent.com/22335838/116121321-5907cd00-a675-11eb-8768-eccc41b55f6b.png">

<img width="635" alt="Screen Shot 2021-04-23 at 11 32 12 AM" src="https://user-images.githubusercontent.com/22335838/116121329-59a06380-a675-11eb-936d-528ffcd4e3a8.png">

![Screen Shot 2021-04-26 at 9 30 36 AM](https://user-images.githubusercontent.com/22335838/116121363-61600800-a675-11eb-9156-f97fe26e0fe5.png)

![Screen Shot 2021-04-26 at 9 30 59 AM](https://user-images.githubusercontent.com/22335838/116121372-61f89e80-a675-11eb-8983-69583a01f54c.png)

...so this is where I quit for now.

### Going forward

1. Try different methods to run R script on `mox`
1. Write methods
2. Obtain relatedness matrix and SNPs with [EpiDiverse/snp](https://github.com/EpiDiverse/snp)
3. Write results
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
