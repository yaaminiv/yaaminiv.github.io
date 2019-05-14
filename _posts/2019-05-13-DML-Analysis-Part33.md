---
layout: post
comments: true
title: DML Analysis Part 33
tags: virginica MBDSeq
---

## Continued troubleshooting

When I asked about statistical tests for characterizing overlaps in [this issue](https://github.com/RobertsLab/resources/issues/685), Mac pointed out that some overlap categories for total CpGs were much lower than those for all 5x CpGs. Since total CpGs are the background for all 5x CpGs, Steven suggested I dig into this further. We went back and forth in the issue comments, but I decided to clean some stuff up and make it easier to follow.

### Creating new genome feature tracks

In [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-05-13-Generating-Genome-Feature-Tracks.ipynb), I downloaded the *C. virginica* genome file. I used `grep` to separate out individual tracks for genes, mRNA, and exons. Using `subtractBed`, I found areas of no overlap between mRNA and exons, and called those introns. I restricted `subtractBed` using `-s` to enforce strandedness. Although pre-made tracks are available [here](https://github.com/RobertsLab/resources/wiki/Genomic-Resources#crassostrea-virginica), it's good practice to create these tracks myself so I know how they are made. I counted 38,929 genes, which is similar to the 39,493 genes listed in the [NCBI Annotation Report](https://www.ncbi.nlm.nih.gov/genome/annotation_euk/Crassostrea_virginica/100/). I counted 60,201 mRNA and 731,279 exons, same as the pre-made tracks. The major difference comes from how introns were counted. The track I generated has 688,167 entries, while the pre-made intron track has 319,262 entries. I'm not entirely sure why this is the case, but figured looking at the track in IGV could help. I used `awk` to generate BEDfiles for each genome feature track to visualze overlaps more easily in IGV. In [this IGV file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-05-13-Generating-Genome-Feature-Tracks/2019-05-13-Genome-Track-Verification.xml), I looked at CG motifs, as well as the gene, mRNA, exon, and intron tracks I generated. 

By comparing the intron track I generated to the pre-made intron track, it looked like there were some regions that were being properly subtracted, and others that were not. 

<img width="1164" alt="Screen Shot 2019-05-13 at 11 24 01 PM" src="https://user-images.githubusercontent.com/22335838/57675367-a567b880-75d6-11e9-80ce-21e56303270f.png">

**Figure 1**. Screenshot from IGV. From top to bottom, the gene, mRNA, exon, and intron tracks I generated are displayed. The bottom-most track was a pre-made intron designation.

At one point, I thought I had a brain blast and decided to use `subtractBed` with the gene track and exon track instead of the mRNA and exon tracks. Using this method, I counted 307,088 possible introns, but there were still discrepancies between the pre-made track and my track:

<img width="1161" alt="Screen Shot 2019-05-14 at 12 01 13 AM" src="https://user-images.githubusercontent.com/22335838/57677395-97686680-75db-11e9-8037-f6a0296727a1.png">

**Figure 2**. Discrepancy between bottom three intron tracks suggests I don't have the right code for intron track generation.

Reading the [`subtractBed` help file](https://bedtools.readthedocs.io/en/latest/content/tools/subtract.html), I couldn't figure out how to solve this issue. I decided to proceed figuring out the code for the next steps while I wait for answers to my questions in [this issue](https://github.com/RobertsLab/resources/issues/692).

The last thing I did was look at how many CG motifs overlapped with the genome tracks I created — the source of my struggles. I first compared the number of lines in the CG motif file with a rough count of all CGs in the *C. virginica* genome. To get my rough estimate, I used the following code:

`````
fgrep -o -i CG {fullGenome} | wc -l # -o: print only matching phrases, -i: ignore case
`````

The 14,458,703 in the CG motif file is roughly similar to 14,277,725 in the full genome, so Sam and Steven felt comfortable with me using the CG motif file. I found 7,914,842 overlaps with genes, 7,507,167 with mRNA, and 2,330,546 with exons. For reference, there were 2,323,389 CG-exon overlaps from the pre-made track. Once I sort through my issue with the intron track, I can characterize overlaps.

### Difference between gene and mRNA tracks

This screenshot shows some extra genic regions that are not included as mRNA, but are included as exons

<img width="1163" alt="Screen Shot 2019-05-13 at 11 59 40 PM" src="https://user-images.githubusercontent.com/22335838/57677379-90415880-75db-11e9-9480-16e652d1348e.png">

**Figure 3**. Area where gene track has extra information that is included in exons but not in the mRNA track.

My assumption is that coding sequences (CDS) are involved, but I would not expect to have to incorporate CDS into intron generation. This is something to continue investigating.

### Going forward

1. Finalize the intron track
2. Understand the difference between the gene and mRNA tracks
3. Characterize location differences between hypermethylated and hypomethylated DML
4. Describe (somehow) genes with DML in them
5. Figure out what's going on with the gene background
6. Figure out what's going on with DMR
7. Work through gene-level analysis
8. Update paper repository
9. Update methods and results
10. Start writing the discussion

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
