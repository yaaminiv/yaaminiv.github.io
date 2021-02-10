---
layout: post
comments: true
title: WGBS Analysis Part 13
tags: manchester gigas-broodstock bismark mox
---

## Starting `bismark` with Manchester data

[After evaluating `trimgalore` output](https://yaaminiv.github.io/WGBS-Analysis-Part12/), I decided it's time to start `[`bismark`](https://github.com/FelixKrueger/Bismark)`! To start, I created [this script](). It's the same as the one I [used for the Hawaii data](), but I updated the paths to all the files. I also moved the bisulfite converted genome from the Hawaii data folder to the Manchester data folder. I transferred the script to `mox` with `rsync` and it started running...

...only to fail two seconds later. Looking at the slurm output, I saw that it was unable to navigate to bisulfite converted genome:

![Screen Shot 2021-02-10 at 1 00 10 PM](https://user-images.githubusercontent.com/22335838/107571872-58948580-6ba0-11eb-9a69-95649294d72d.png)

I navigated to the directory and saw that it was empty! The files must have been on the computer for longer than 30 days before I moved the genome folder from the Hawaii to Manchester subdirectories. I used `wget` (not `curl` because `mox` doesn't like it) to download the [bisulfite converted genome Sam created](https://robertslab.github.io/sams-notebook/2019/02/21/Data-Management-Create-C.gigas-Bisulfite-Genome-with-Bismark-on-Mox.html) from [this link](http://owl.fish.washington.edu/halfshell/genomic-databank/Crassostrea_gigas.oyster_v9.dna_sm.toplevel_bisulfite.tar.gz). I then extracted the files:

```
tar -xvzf Crassostrea_gigas.oyster_v9.dna_sm.toplevel_bisulfite.tar.gz #Extract (x) and decompress (z) files (f) in a verbose (v) manner
```

I navigated into the directory to double check that there was actually a bisulfite converted genome inside `/gscratch/scrubbed/yaaminiv/Manchester/data/Crassostrea_gigas.oyster_v9.dna_sm.toplevel/`. Then, I ran the script. Since I only have eight samples, I'm hoping it'll finish processing in a week!

### Going forward

1. Update the repository README files
2. Write methods
3. Write results
3. Identify DML
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
