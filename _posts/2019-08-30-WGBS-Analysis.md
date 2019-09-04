---
layout: post
comments: true
title: WGBS Analysis
tags: manchester gigas-broodstock WGBS mox
---

## Testing `bismark` parameters for WGBS data

We're 18 days out from PCSGA, which means I should start analyzing my data! In February, [I pooled *C. gigas* samples from ambient and low pH treatments for Whole Genome Bisulfite Sequencing (WGBS)](https://yaaminiv.github.io/WGBS-Samples-Part2/). I'm planning on processing the data to look at differential methylation between treatments so I can justify future sequencing.

### Determining parameters

The first step in this process is aligning samples to a bisulfite genome with `bismark`. I started [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2018-08-30-Bismark-Parameter-Testing.ipynb) to test different alignment stringencies with a subset of the data before running the full `bismark` pipeline. Sam already [trimmed my files](https://robertslab.github.io/sams-notebook/2019/04/15/FastQC-WGBS-Sequencing-Data-from-Genewiz-Received-20190408.html) and [prepared the bisulfite genome](https://robertslab.github.io/sams-notebook/2019/02/21/Data-Management-Create-C.gigas-Bisulfite-Genome-with-Bismark-on-Mox.html), so that's two things I didn't have to do! I downloaded the bisulfite genome from [this link](http://owl.fish.washington.edu/halfshell/genomic-databank/Crassostrea_gigas.oyster_v9.dna_sm.toplevel_bisulfite.tar.gz) and trimmed samples [from this link (names start with YRV)](http://owl.fish.washington.edu/nightingales/C_gigas/). I would have run `bismark` without downloading first, but `owl` is super slow I'd rather just have them on `genefish`.

Once I had the files dowloaded, I referred to [my *C. virginica* script for parameter testing](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-10-03-Bismark-Parameter-Testing.ipynb). The last thing I wanted to do was confirm my files were paired and non-directional. I was pretty sure they were paired because there were two files for each sample. I wasn't sure if they were non-directional. I posted [this issue](https://github.com/RobertsLab/resources/issues/738) to confirm. After running different alignment stringencies, I found that `score_min = L,0,-0.9` gave me about 61% alignment. Less stringent settings gave me 68% alignment, while more stringent settings gave me 47% alignment. I decided to stick with 61% alignment because it was the biggest jump between settings, and I didn't gain much from less stringent settings.

### Running `bismark` on Mox

Now that I had my alignment settings in order, I could run a full alignment on `mox`. I modified [a previous script](https://github.com/fish546-2018/yaamini-virginica/blob/master/scripts/2018-11-08-Bismark-Revised-Parameters-Samtools.sh) for `bismark` on Mox with my *C. virginica* samples to use with my *C. gigas* samples. I saved the revised script [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/scripts/2019-09-03-Bismark.sh). I used the following commands to move files over to Mox:

```
rsync --archive --progress --verbose yaamini@172.25.149.226:/Users/yaamini/Documents/project-gigas-oa-meth/analyses/2019-08-30-Bismark-Parameter-Testing/*fastq.gz .#Transfer analysis files
```

```
rsync --archive --progress --verbose yaamini@172.25.149.226:/Users/yaamini/Documents/project-gigas-oa-meth/analyses/2019-08-30-Bismark-Parameter-Testing/Crassostrea_gigas.oyster_v9.dna_sm.toplevel . #Transfer bisulfite genome to Mox
```

```
rsync --archive --progress --verbose yaamini@172.25.149.226:/Users/yaamini/Documents/project-gigas-oa-meth/scripts/2019-09-03-Bismark.sh . #Transfer script to USER directory, not scrubbed directory
```

To run the script:

```
sbatch 2019-09-03-Bismark.sh
```

It's been a while since I last used Mox, so I encountered [this error](https://github.com/RobertsLab/resources/issues/740). Turns out `--workdir` is no longer used! I changed it to `--chdir`. I learned a new verison of `bismark` was installed, so I changed version 19 to 21 in the script. Once I made those changes, I reran the script! The job number was 1266351.

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
