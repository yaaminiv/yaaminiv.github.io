---
layout: post
comments: true
title: DML Analysis Part 34
tags: virginica MBDSeq IGV
---

## Even more troubleshooting with IGV

### Gene background issues

Before I could proceed with a gene enrichment, I needed to sort through my [gene background issues](https://yaaminiv.github.io/DML-Analysis-Part27/). The gene background consists of all loci with 5x coverage in my dataset. Previously, my gene background was "hot flaming garbage" when I looked at it in IGV. Assuming that the gene background was working correctly in `methylKit`, I [returned to my R Markdown file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-10-25-MethylKit/2018-10-25-MethylKit.Rmd) to see if the gene background issues were a result of the way I exported the file.

Looking at the gene background, I noticed that there were columns with coverage metrics, as well as the number of cytosines and thymines at each locus. The start and stop position for each locus was the same, which could be affecting how the visualization occurs in IGV.

![Screen Shot 2019-05-15 at 1 56 52 PM](https://user-images.githubusercontent.com/22335838/57808904-4d859a80-7719-11e9-95aa-f7ab6b49a327.png)

**Figure 1**. Gene background information in `methylKit`.

I decided to subset the first four columns for exporting: chromosome (`chr`), start position (`start`), stop position (`stop`), and strand (`strand`). I did this by creating a new dataframe. I also subtracted 1 from each start position to visualize the data. Finally, I exported the gene background as a BEDfile and looked at it in IGV.

`````
methylationInformationFilteredCov5DestrandReduced <- data.frame("chr" = methylationInformationFilteredCov5Destrand$chr,
                                                                "start" = methylationInformationFilteredCov5Destrand$start,
                                                                "stop" = methylationInformationFilteredCov5Destrand$end,
                                                                "strand" = methylationInformationFilteredCov5Destrand$strand) #Subset data
methylationInformationFilteredCov5DestrandReduced$start <- (methylationInformationFilteredCov5DestrandReduced$start - 1) #Subtract 1 from the start position for visualization
write_delim(methylationInformationFilteredCov5DestrandReduced, "2019-05-14-Methylation-Information-Filtered-Destrand-Cov5.bed", delim = "\t", col_names = FALSE) #Write out all methylation information as a background to be used for gene enrichment analyses.
`````

I opened [this IGV session](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-07-IGV-Verification/2019-03-07-DML-and-DMR-Visualization.xml), which had my previous gene background file in it. I added the new file and compared the two versions:

![Screen Shot 2019-05-14 at 2 20 13 PM](https://user-images.githubusercontent.com/22335838/57807019-33e25400-7715-11e9-94c4-64844d4ba312.png)

![Screen Shot 2019-05-14 at 2 21 09 PM](https://user-images.githubusercontent.com/22335838/57807020-33e25400-7715-11e9-95b3-4551781f98d3.png)

![Screen Shot 2019-05-14 at 2 22 43 PM](https://user-images.githubusercontent.com/22335838/57807021-33e25400-7715-11e9-9b0a-df8b9616e891.png)

**Figures 2-4**. Gene background visualization in IGV.

Looking at different sections, I found that the gene background aligned with CG motifs and various DML instead of looking like large regions of information! I feel confident with this new version of the gene background and think I could use it for gene enrichment.

### Difference between gene and mRNA tracks

During lab meeting, I pointed out that there were coding sequences included in genes that were not part of the mRNA track. These coding sequences had defined exons. Kaitlyn pointed out that the exons are the elements retained in mRNA, but the exons in coding sequence (CDS) are the parts that get translated. That clears up the question I posed in [this issue](https://github.com/RobertsLab/resources/issues/692). 

### Refining the intron track

Based on this information, I decided to use `subtractBed` with the gene ane exon tracks to create my intron track. When I looked through that track in IGV, I found that there were some areas where the intron track looked good. Introns from coding sequences were included (unlike the one I generated using mRNA), and the first bp in every exon sequence was no longer being included in the intron track.

![Screen Shot 2019-05-15 at 3 23 05 PM](https://user-images.githubusercontent.com/22335838/57814173-abb97a00-7727-11e9-8c70-81ead5848bf8.png)

**Figure 5** Example of introns generated from the gene and exon tracks that are not included in the intron track I generated from mRNA or the intron track I downloaded from the [Genomic Resources wiki](https://github.com/RobertsLab/resources/wiki/Genomic-Resources#genome-feature-tracks-1).

![Screen Shot 2019-05-15 at 3 22 40 PM](https://user-images.githubusercontent.com/22335838/57814174-abb97a00-7727-11e9-8bd5-e26b7f0aba9e.png)

![Screen Shot 2019-05-15 at 3 21 51 PM](https://user-images.githubusercontent.com/22335838/57814175-ac521080-7727-11e9-9084-4f8df5212aa0.png)

**Figure 6-7**. Instances were the intron track from the [Genomic Resources wiki](https://github.com/RobertsLab/resources/wiki/Genomic-Resources#genome-feature-tracks-1) includes the first bp of each exon, but the intron track I generated does not.

However, there were instances of the intron track I generated still including exons:

![Screen Shot 2019-05-15 at 3 28 58 PM](https://user-images.githubusercontent.com/22335838/57814344-41550980-7728-11e9-8d8c-27ef37e4f4ee.png)

![Screen Shot 2019-05-15 at 3 25 57 PM](https://user-images.githubusercontent.com/22335838/57814345-41550980-7728-11e9-8ed9-256b37297e07.png)

**Figures 8-9**. Areas where the intron track still contained exons.

I posted my findings in [this comment](https://github.com/RobertsLab/resources/issues/692#issuecomment-492849768) and asked if it was an artifact of enforcing same strandedness with `-s`, or if it was worth using `subtractBed` on the intron and exon tracks to get rid of the overlaps.

### Going forward

1. Finalize the intron track
2. Conduct a gene enrichment for DML
3. Figure out what's going on with DMR
4. Work through gene-level analysis
5. Update paper repository
6. Update methods and results
7. Start writing the discussion

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
