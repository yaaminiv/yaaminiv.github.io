---
layout: post
comments: true
title: MethCompare Part 3
tags: MethCompare IGV bedtools
---

## Finalizing genome feature tracks

Based on feedback [to only generate tracks common between species](https://yaaminiv.github.io/MethCompare-Part2/), I focused on validating the tracks I generated already for *M. capitata* and *P. acuta*.

### *M. capitata* tracks

Since *M. capitata* does not have any transcript, exon, or UTR information, I can't generate UTR or promoter tracks. While I could generate an intergenic region track, I figured this might be redundant. With `intersectBed -v`, I can figure out what CpG loci intersect with intergenic regions using the gene track. If Hollie wanted to make Circos plots with various gene tracks, any locus that falls outside of a gene would be considered intergenic. I held off on making these tracks for now, but I may revist later if we decide they are necessary.

Earlier this week, I posted [this issue](https://github.com/RobertsLab/resources/issues/893) asking for CG motif tracks for both species. I used the links from that issue to add CG motifs to [this IGV session](https://github.com/hputnam/Meth_Compare/blob/master/genome-feature-files/Mcap-Genome-Feature-Tracks.xml) with *M. capitata* feature tracks. Hopefully this will be useful for us later.

### *P. acuta* tracks

I wasn't able to visualize the *P. acuta* tracks I generated in IGV, so I posted [this issue](https://github.com/RobertsLab/resources/issues/890). Steven pointed out that the scaffold names did not match the names in the original genome file, since there was an additional underscore in scaffold names in the genome file but not in the genome feature tracks. I posited a complicated plan to add this underscore to the right place, but Sam usggested I streamline my multistep process by using `sed`.

<img width="942" alt="Screen Shot 2020-04-09 at 12 45 24 PM" src="https://user-images.githubusercontent.com/22335838/78934559-fe96d100-7a5f-11ea-917b-5761c980cacf.png">

Using these modified tracks, I was able to see them in IGV! Even though *P. acuta* has separate CDS and exon tracks, they're made up of the same components. It looks like the CDS are transcript specific and string together what is actually coded for each transcript, while thye exons are split up into initial, internal, and terminal sequences that may shed more light on what specific sections go into each transcript. Looking at the gene, transcript, exon, and CDS start positions, I did not see any UTR. That is consistent between species, so I wouldn't have been able to generate that information to begin with.

![Screen Shot 2020-04-08 at 1 53 29 PM](https://user-images.githubusercontent.com/22335838/78935160-0c008b00-7a61-11ea-8306-bf4a9ddfb651.png)

When spot-checking the tracks to make sure they were valid, I noticed that the introns seemed to overlap with coding sequences.

![Screen Shot 2020-04-08 at 1 52 26 PM](https://user-images.githubusercontent.com/22335838/78935217-2dfa0d80-7a61-11ea-945d-acd9c8b06709.png)

Looking closer, the transcript-specific introns were being mashed together, instead of showing two overlapping features.

![Screen Shot 2020-04-08 at 2 00 34 PM](https://user-images.githubusercontent.com/22335838/78935218-2fc3d100-7a61-11ea-99d6-9e2db4284873.png)

Since I know they are separate in the actual tracks, it wasn't the end of the world, but it was frustrating that I wasn't able to visualize it properly. I posted [this issue](https://github.com/RobertsLab/resources/issues/896) to see if I can get them to display as separate features. Sam suggested I look at the original GFF I pulled the tracks from, but this would involve adding the underscore to scaffold names after pulling out all AUGUSTUS lines. He also suspected that this would be an issue regardless becuase coordinate-based systems like IGV probably merged overlapping features. He was corret. When I looked at the CG motifs, I found that two adjacent motifs were combined into one feature.

![Screen Shot 2020-04-08 at 3 05 58 PM](https://user-images.githubusercontent.com/22335838/78935468-abbe1900-7a61-11ea-9af6-a1984db85d63.png)

While it's not ideal, I know that it's probably not my fault. I added the CG motifs to [this IGV session](https://github.com/hputnam/Meth_Compare/blob/master/genome-feature-files/Pact-Genome-Feature-Tracks.xml) for *P. acuta* tracks and decided to move on.

### Going forward

3. Intersect all genome feature tracks with CG motif information
4. Set up CpG characterization pipeline with revised tracks
2. Rerun the pipeline with full samples
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
