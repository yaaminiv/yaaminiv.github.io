---
layout: post
comments: true
title: Gonad Methylation Analysis Part 4
tags: virginica bismark MBDSeq
---

## Preparing a genome (and my brain)

I forgot just how time-consuming (and brain pain-inducing) bioinformatics pipelines are! Good thing I set up my monitor. Screensharing on my laptop screen would not be ideal.

I created a new [Jupyter notebook](https://github.com/RobertsLab/project-virginica-oa/blob/master/notebooks/2018-04-27-Gonad-Methylation-Bismark.ipynb) for the Bismark portion of my pipeline. Here's the basic outline of the Bismark pipeline:

1. Genome preparation
2. Alignment
3. Methylation extraction
4. HTML report
5. Summary report

I got started on the genome preparation portion. To do this, I first downloaded the genome from [NCBI](https://www.ncbi.nlm.nih.gov/genome/?term=Crassostrea%20virginica). I learned that when you download FASTA files from NCBI, it specifies what kind of FASTA it is. In my case, I downloaded a .fna, or nucleotide FASTA file for the genome. I had to convert the .fna to an .fa for Bismark using the handy `cp` command.

Obviously I needed Bismark and Bowtie2 on the Mac mini before I could use it. This part was obviously strugglesome (see [this issue](https://github.com/RobertsLab/resources/issues/234) if you don't believe me). I downloaded Bismark from [this website](https://www.bioinformatics.babraham.ac.uk/projects/bismark/), and Bowtie2 using this code:

> conda install -c bioconda bowtie2

It was simple, but Steven suggested downloading the source code in the future to have more control over versioning and the full path.

Finally, I could use the `bismark-genome-preparation` command. Here's what I needed:

1. Path to `bismark-genome-preparation`
2. --path-to-bowtie + the path to the **folder** with bowtie2, and not the path to bowtie2
3. --verbose, which prints detailed status reports
4. Path to the folder with the .fa or .fasta genome file

And it's running! I'll check on it later tonight to see if I can move on to the alignment step.

![untitled](https://user-images.githubusercontent.com/22335838/39389962-e8d0447c-4a42-11e8-9a19-80d910b1761a.png)

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
