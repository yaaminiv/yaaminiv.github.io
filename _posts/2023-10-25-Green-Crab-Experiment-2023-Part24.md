---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 24
tags: green-crab-wc sequencher genotype
---

## Genotyping respirometry crabs

I got my Sanger sequencing data back! Before I sent out my samples, Carolyn showed me how to use Sequencher to clean up the data and call genotypes. I followed her protocol to get genotype data for 25/27 respirometry crabs.

### Methods

1. Open Sequencher. Create a New Project and add Sequences (`.abi` files, with chromatograms)
2. Double click the sequences for the forward strand of a sample, then click "show chromatogram."
3. With the chromatogram showing, remove the sequence information for poor quality chromatograms. Repeat for the reverse strand.
4. After cleaning the chromatograms, select both files, then click "Assemble Automatically." This will create a new contig. Rename that contig with the sample name.
5. Double click on the newly assembled contig, then click "Bases." This will bring you to a view of the forward and reverse sequences in blue colors on top, with the reference sequence on the bottom. There will be dots indicate disagreements.
6. Click "Show Chromatograms." To fix disagreements manually, click on the dot under the sequences in blue lettering. This will bring you to the place in the chromatograms with the disagreement. At each disagreement, evaluate if a nucleotide needs to be added, deleted, or changed. After fixing each disagreement, click "ReAligner" to ensure proper alignment at each step.
7. Repeat for all sequences.
8. Once all sequences have been cleaned and assembled into individual contigs, highlight all contigs and click "Assemble Automatically." This will create a "master contig". Rename with the genotyping date. Include reads that were not assembled into contigs in the event that one strand was poorer quality than another. The higher quality strands will be assembled while the lower quality strads will not be included in the master contig.
9. Once the master contig is assembled, rearrange the F and R strands for each sample so they are on top of eachother. The default order once assembled is to group all R together and all F strands together, which is not conducive for genotyping individual samples. Click "Show Chromatograms" and confirm that the chromatograms are in the correct order.
10. In the "Bases" view, click on the nucleotide with the ambiguity. This will bring all chromatograms to the locus used to genotype samples. Sometimes, sequence quality will show to loci with ambiguities, other times just one.
11. Go through chromatograms for each sample and confirm that the bases are correctly called at the locus with the ambiguity. Record the genotype at this locus. If sequence quality allows for genotype to be called at two loci, the genotypes at these loci should match.

### Notes

- 5-005: No readable sequence for F strand, unable to assemble contig
- 25-052: Garbage F read, unable to assemble contig
- 15-097: Poor qualilty F and R reads but still able clean data and assemble contig
- Assembled master contig with all individual sample contigs and R reads from 5-005 and 25-052
- Only had genotype data at one locus. Unsure where the second locus of data is.

### Results

**Table 1**. Number of CC, CT, and TT genotypes per treatment and tank

| **Treatment** | **Tank** | **CC** | **CT** | **TT** |
|:-------------:|:--------:|:------:|:------:|:------:|
|      5ºC      |     1    |    0   |    2   |    1   |
|      5ºC      |     4    |    0   |    3   |    0   |
|      5ºC      |     7    |    0   |    1   |    2   |
|      15ºC     |     2    |    2   |    0   |    0   |
|      15ºC     |     5    |    0   |    1   |    2   |
|      15ºC     |     8    |    1   |    1   |    0   |
|      25ºC     |     3    |    2   |    0   |    1   |
|      25ºC     |     6    |    0   |    1   |    2   |
|      25ºC     |     9    |    0   |    3   |    0   |

Looks like I have a few crabs per genotype in each treatment for my respirometry crabs, with the exception of missing CC from the 5ºC treatment. I need to confirm which genoytpe is connected to warm tolerance vs. cold tolerance.

Now that I have this data, I can incorporate it into my TTR and respirometry analyses for the subset of crabs that were used for both!

### Going forward

1. Treatment-wise respirometry analysis
2. Incorporate genotype into TTR and respirometry analyses
3. Prepare talk for PICES
4. Finish extracting respirometry samples
5. Continue with Chelex extractions, PCRs, and gels for TTR crabs
6. Update methods and results of 2022 paper
7. Update methods and results of 2023 paper

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
