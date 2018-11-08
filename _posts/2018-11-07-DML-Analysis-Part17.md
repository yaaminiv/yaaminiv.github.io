---
layout: post
comments: true
title: DML Analysis Part 17
tags: virginica MBDSeq DML 
---

## `methylKit` and `bedtools` with Mox samples

I took my samples off of Mox, identified DML and DMR, and characterized their location! :tada:

### `bismark`

I finished my `bismark` pipeline on Mox! It did, however, take me a bit of tweaking. I ended up needing a few different scripts because I kept making mistakes in my code:

1. I used [this script](https://github.com/fish546-2018/yaamini-virginica/blob/master/scripts/defunct-scripts/2018-10-31-Bismark-Revised-Parameters-Samtools.sh) for my alignment. I could not complete deduplication because I did not include `--samtools_path`.
2. I used [this script](https://github.com/fish546-2018/yaamini-virginica/blob/master/scripts/defunct-scripts/2018-11-06-Bismark-Deduplication-Onwards.sh) for deduplication. I was able to get bismark reports as well, but again, I couldn't complete the methylation extraction because I did not include `--samtools_path`.
3. I used [this script](https://github.com/fish546-2018/yaamini-virginica/blob/master/scripts/defunct-scripts/2018-11-06-Methylation-Extraction.sh) for methylation extraction and report creation. Everything worked!

I moved all of my scripts to [this folder](https://github.com/fish546-2018/yaamini-virginica/blob/master/scripts/defunct-scripts/) for defunct scripts, and created a master script with all revisions that can be found [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/scripts/2018-11-08-Bismark-Revised-Parameters-Samtools.sh).

To move my files off of Mox, I initially thought I should create a checksum file and then use `rsync`. When I tried creating a checksum file in the login node, I got an error message saying I was overloading the CPU. I posted [this issue](https://github.com/RobertsLab/resources/issues/473), and learned that I could either create checksums from the interactive node, or just `rsync` and create the checksum file later since `rsync` already verifies checksums as it is transferring the files. I went with the second option. All of the files from Mox are now on [this gannet folder](http://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/2018-11-07-Bismark-Mox/). I created the checksums with `shasum`.

### `methylKit`

This part was easy since I already had an [R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd) ready. I changed the path for the filenames, and cranked away. Output from DML identification is [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-10-25-MethylKit/2018-10-25-Loci-Analysis), and output from DMR identification can be found [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-10-25-MethylKit/2018-10-25-Tiling-Analysis). Nothing changed between this run and my previous `methylKit` runs using files generated on `genefish`.

### `bedtools`

I characterized the location of DML and DMR in [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb). Based on [this issue](https://github.com/RobertsLab/resources/issues/466), I added sections to find overlaps between DML, DMR, and transposable elements. There are two different transposable element files. According to Sam, "`C_virginica-3.0_TE-all.gff` used all species that exist in the database and `C_virginica-3.0_TE-Cg.gff` only used _Crassostrea virginica_ database" to identify transposable elements in the _C. virginica_ genome. `C_virginica-3.0_TE-all.gff` had more transposable elements, so I got different results for each file when I used `intersectBed`. I also looked at overlaps between transposable elements and either exons, introns, or mRNA coding regions. I generated a lot of data! All of the files with overlap locations can be found [here](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2018-11-01-DML-and-DMR-Analysis).

### Going forward

Steven said to focus on DMR characterization since he found those more interesting in the Olympia oyster data he's working with, so I'll develop all of the code for analyses for DMR first. I can then move on to DML if I want.

1. Conduct flanking analysis for DMR
2. Start gene enrichment for DMR
3. Update methods and results in [my draft manuscript](https://docs.google.com/document/d/1gOMJrnhs4D-jCKWlJK2tm0Z27IrSqMkmc7K1pDBmqi0/edit#)

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
