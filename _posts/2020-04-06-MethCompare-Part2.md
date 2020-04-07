---
layout: post
comments: true
title: MethCompare Part 2
tags: MethCompare IGV
---

## More genome feature tracks

After I [looked over code](https://github.com/hputnam/Meth_Compare/issues/34) and [added analyses, approaches, and figures](https://github.com/hputnam/Meth_Compare/issues/31) we could explore in the paper, I set out to refine genome feature tracks for *M. capitata* and *P. acuta*. Once we confirm we trust our analysis files, I can use these feature tracks to understand where methylation occurs in these species.

### *M. capitata* CDS track

The first thing I did was examine the *M. capitata* CDS track further. I went back to [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Generating-Genome-Feature-Tracks.ipynb) where I generated all the genome feature tracks. The first thing I realized was that I didn't make the gene track correctly! When I used `grep "gene"`, it pulled lines with CDS and intron information and saved that to the gene track. I used `grep AUGUSTUS	gene"` instead, and used similar code for the rest of the *M. capitata* tracks.

Now to the task at hand: understanding the CDS track. Looking at the gene track using `head`, I could see that CDS were split up by introns. In that way, the CDS track is similar to an exon track.

<img width="866" alt="Screen Shot 2020-04-07 at 10 44 36 AM" src="https://user-images.githubusercontent.com/22335838/78702013-c86d1c00-78bc-11ea-8118-e46823ceac47.png">

But looking at all the tracks in [this IGV session](https://github.com/hputnam/Meth_Compare/blob/master/genome-feature-files/Mcap-Genome-Feature-Tracks.xml), I don't think the CDS track includes UTR.

![Screen Shot 2020-04-06 at 2 48 24 PM](https://user-images.githubusercontent.com/22335838/78608390-ad939c80-7815-11ea-8bb0-eae66f75d613.png)

I posted [this issue](https://github.com/hputnam/Meth_Compare/issues/36) to get more clarity about the CDS track and see if we can derive UTR or exon information from the gene and CDS tracks. If not, then I don't think I'll be able to do comparisons between *M. capitata* and *P. acuta*.

### Visualizing *P. acuta* tracks

The *P. acuta* tracks were in better shape: I have gene, transcript, exon, intron, and CDS information. Within exons, I have initial, internal, and terminal exon tracks that can help us answer a lot of questions about exon-specific methylation. I wanted to create an IGV session for all the *P. acuta* tracks. I downloaded the genome from the Google Drive and tried visualizing the intron track I generated in [this notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Generating-Genome-Feature-Tracks.ipynb). In IGV, this track looks blank (even though I know from my files that there are introns on this scaffold):

<img width="781" alt="Screen Shot 2020-04-06 at 9 58 09 PM" src="https://user-images.githubusercontent.com/22335838/78631621-b7d38c00-7851-11ea-81a9-8082441229d1.png">

<img width="1007" alt="Screen Shot 2020-04-06 at 9 58 27 PM" src="https://user-images.githubusercontent.com/22335838/78631632-c02bc700-7851-11ea-998f-704a13330dbd.png">

I posted [this issue](https://github.com/RobertsLab/resources/issues/890) to get some help. I thought maybe the file wasn't sorted correctly, but since I pulled it directly from the genome I don't think that would be the issue. We'll see.

### Going forward

2. Create promoter, UTR, and intergenic region tracks for species depending on what information is available and what is possible
3. Intersect all genome feature tracks with CG motif information
2. Rerun the CpG characterization pipeline with full samples and incorporate new genome features
3. Create concatenation files and figure out methylation island analysis

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
