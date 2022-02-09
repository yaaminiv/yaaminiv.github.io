---
layout: post
comments: true
title: Killifish Hypoxia RRBS
tags: killifish RRBS poseidon
---

## Starting a new project!

I'm working with Neel to analyze RRBS and RNA-Seq data 6 month-old killifish liver tissue from 2018. These fish are from two populations: contaminant-susceptible and contaminant-resistant. The hypothesis is that hypoxia tolerance may be linked to contaminant resistance, with the resilient population being unable to respond to a secondary stressor. Neel finished RNA-Seq analysis, but since there's a newer genome assembly now's a great time to reanalyze both datasets.

### Setting up

Before I could begin, I needed to install programs on [WHOI's HPC, Poseidon](https://whoi-it.whoi.edu/creating-scripts/). When I logged in, I tried navigating to my scratch directory, but found it didn't exist and I was unable to create one for myself!

After checking modules with `module avail`, I decided to install `fastqc`, `cutadapt`, `multiqc`, and `trimgalore`. To do this, I figured it would be easiest to install `miniconda` in my home directory, then install the remaining packages. I downloaded the Python 3.9 Linux 64-bit installer locally, then used `rsync` to move the file to Poseidon. I followed [these instructions]( https://docs.conda.io/en/latest/miniconda.html#linux-installers) to install the program, then used `conda activate` to initialize the installation.

I then configured my environment by following [these instructions](https://bioconda.github.io/user/install.html) from `bioconda`:

```
conda config --add channels defaults
conda config --add channels bioconda
conda config --add channels conda-forge
```

I followed [these instructions](https://cutadapt.readthedocs.io/en/stable/installation.html) for `cutadapt` installation:

```
conda create -n cutadaptenv cutadapt #Create a new environment to install cutadapt
conda activate cutadaptenv #Need to be done every time I open a new shell before I use cut adapt
cutadapt --version #Check the installation completed
```

Used [these instructions](https://multiqc.info) for `multiqc` installation:

```
conda install -c bioconda -c conda-forge multiqc
```

After I did this, I realized I could have just done the same thing with `cutadapt` so I didn't need a new module environment every time I wanted to use the program. I used the same method for `fastqc` installation instead of downloading the zip file from the FastQC website and unzipping it for use:

```
conda install -c bioconda -c conda-forge cutadapt #Wanted to not have a separate environment

conda install -c bioconda -c conda-forge fastqc #Worked without the zip file!!
```

Finally, I installed `trimgalore` based on [these Github instructions](https://github.com/FelixKrueger/TrimGalore):

```
curl -fsSL https://github.com/FelixKrueger/TrimGalore/archive/0.6.6.tar.gz -o trim_galore.tar.gz
tar xvzf trim_galore.tar.gz
```

### Creating the script

I modified my script based on [my previous one from my Manchester repository](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/01-fastqc.sh). I reviewed the script and ensured that I had the correct output directory for my work, the directory with the raw data, program paths, and python module. I saved my script [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/01-fastqc.sh) in my new [project repository](https://github.com/yaaminiv/killifish-hypoxia-RRBS).

When I ran the script, I got the following error:

```
sbatch: error: Batch job submission failed: Invalid account or account/partition combination specified
```

The account error made me think that it may be related to my lack of a scratch directory, so I created a ticket with WHOI IS for these issues.

### Running `fastqc` and `multiqc`

Turns out having a Poseidon account isn't enough to run scripts: IS had to activate account permissions for me so I could submit scripts to SLURM. I encountered a few problems with the script when I realized I removed the `fastqc` and `multiqc` program paths. Once I added them back in, I was able to run `fastqc` on all samples. However, the script failed before creating checksums! This meant that `multiqc` didn't run either. I manually created checksums for the `fastqc` output and ran `multiqc` on the samples.

Now came the really annoying part...transferring files off of Poseidon. I tried using `rsync` from my computer to move files off of Poseidon, but I kept getting the error that `zsh` did not recognize `yaamini.venkataraman@poseidon:/path/`. I moved all my files to my home directory, then mounted it on my computer, and used `rsync` to transfer files that way. All output can be found in [this subdirectory](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/01-fastqc).

### Going forward

1. Trim adapters with TrimGalore! and re-run FastQC
3. Start alignment with Bisulfite Analysis Toolkit

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
