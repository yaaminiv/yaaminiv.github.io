---
layout: post
comments: true
title: WGBS Analysis Part 20
tags: manchester gigas-broodstock methylKit mox
---

## Running R scripts on `mox`

The moment we've all been waiting for: moving over to `mox`! A week ago I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1176) to get more information about the actual process, but I didn't get any feedback. Turns out this kind of workflow is new for the lab, so I'll be figuring more stuff out on my own. Thankfully I think I have enough information from `mox` documentation, so we'll see what happens.

### Installing the latest R version

First things first, installations. I started using [these instructions](https://wiki.cac.washington.edu/display/hyakusers/Hyak+R+programming) to install `R-4.0.5`. The first step is making sure you're not using anaconda python, but I skipped this step to see how far I could get without doing this. When I reached step 5, the UC Berkeley CRAN mirror was not accessible, so I [used OSU's instead](https://ftp.osuosl.org/pub/cran/). Interestingly, I wasn't able to download `R` to the `gscratch/srlab/yaaminiv/Rstuff` folder, but I could download the files to my user directory.

I ran `./configure --prefix=/gscratch/srlab/yaaminiv/Rinstall` from the `R-4.0.5` directory and got the following error:

`configure: error: PCRE2 library and headers are required, or use --with-pcre1 and PCRE >= 8.32 with UTF-8 support`

A quick search lead me to [this page](https://unix.stackexchange.com/questions/635758/local-installation-of-pcre2-not-detected-while-installing-r-4-0-4-from-source), so perhaps I needed to install the package. However, this issue could be linked to me using anaconda python. I posted in [this discussion](https://github.com/RobertsLab/resources/discussions/1178) to ask how to remove anaconda python from my `~/.bashrc`.

<img width="749" alt="Screen Shot 2021-04-13 at 1 27 46 PM" src="https://user-images.githubusercontent.com/22335838/114616716-0a613880-9c5c-11eb-94f3-ed7847dd7528.png">

Sam suggested I 1) use `conda deactivate` since I was in an anaconda environment (indicated by `(base)`), and 2) try `./configure --prefix=/gscratch/srlab/yaaminiv/Rinstall --with-prce1` based on the error message suggestion. I got the same error even though I wasn't in an anaconda environment. Following Sam's instructions, I commented out the `conda initialize` portion of my `~/.bashrc`, logged out of `mox`, and requested another build node. When I typed `which python`, I was directed to `/usr/bin/python`, which meant I was probably good to start again with the installation.

I ran `./configure --prefix=/gscratch/srlab/yaaminiv/Rinstall` and got the same error. I tried again with `./configure --prefix=/gscratch/srlab/yaaminiv/Rinstall --with-prce1` and was able to move forward with `make`! I couldn't run `make install` since the `/gscratch/srlab` quota was exceeded. Sam removed some files and I waited a bit before successfully continuing with `make install`. I added the newest R version to my path with `export PATH=/gscratch/srlab/yaaminiv/Rinstall/bin:$PATH`!

<img width="582" alt="Screen Shot 2021-04-13 at 3 38 00 PM" src="https://user-images.githubusercontent.com/22335838/114629894-3f768680-9c6e-11eb-897e-bee03a891e55.png">

### Installing `methylKit`

Now that I had the latest version of R, I needed to install `methylKit`. I started by trying to install `devtools`, since I needed to use `install_github` to install `methylKit`. When I tried installing `devtools` on the latest version of R, it says it wasn't compatible with the newest version! Totally could have loaded an older R version module but I was annoyed that I potentially installed the newest version of R for nothing and decided to call it quits for the day XD

### Going forward

1. Install `methylKit` on `mox`
3. Run R script on `mox`
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
