---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 23
tags: hawaii gigas-ploidy bismark
---

## Reboot: The final countdown (for the Hawaii paper)

This summer I am determined to finish this paper! Not just determined, but I've actually set aside time to work on this paper and am doing Roberts Lab pubathon. So yeah, it's going to get done. My goal for the summer is to have an updated draft that can go out to any other co-authors. Bonus points if I submit for publication, but I'm also happy to do that fall quarter. Here's what stands between me and that goal (besides just revising different parts of the paper):

- Alas, enough time has passed that there is a new *C. gigas* genome! At first I couldn't find the files I needed so I thought I hallucinated the whole thing and posted [a Github issue](https://github.com/RobertsLab/resources/issues/2461#issuecomment-4860921194). Obviously, once I posted the issue I found files to indicate that yes, there was a new genome and Steven had run `bismark` with the new genome. Sam confirmed my findings and found all the associated output on `gannet`. So now I have to incorporate that into the analysis.
- A few methodological novelties I want to try with this dataset include a randomization test with `methylKit`, integration with *C. gigas* ATAC-Seq, csRNA-Seq, and 5'-GRO data, and `KOG-MWU` for DML comparison with other *Crassostrea spp.* epigenetic studies.
- Figure out if we can get the pH and water quality data. Probably a long-shot this far out...

### Genome comparison for `bismark` results

I decided to start by just recapping the `bismark` results from the previous genome, and comparing that with the new genome.

**Table 1**. Comparison of mapping statistics across two genomes.

|      **Metric**      |   **Roslin Genome**  | **xbMagGiga1 Genome** |
|:--------------------:|:--------------------:|:---------------------:|
|    Alignment score   |         -0.6         |          -0.8         |
| Alignments (# reads) | 3.1 x 10<sup>7</sup> |  3.2 x 10<sup>7</sup> |
|    Alignments (%)    |          61          |           63          |
|  CpG methylation (%) |         10.4         |          10.6         |

One thing to note here is that Steven and I used different mapping parameters. Steven used [--score-min L,0,-0.8](https://gannet.fish.washington.edu/v1_web/owlshell/bu-github/project-oyster-oa/analyses/Haws-10/zr3644_1_PE_report.txt) while I used [--score-min L,0,-0.6](https://gannet.fish.washington.edu/spartina/project-oyster-oa/Haws/bismark-2/zr3644_1_R1_val_1_val_1_val_1_bismark_bt2_PE_report.txt). Steven used a mapping parameter that was less sensitive and specific, which could explain the differences in the number and percent of alignments. I would prefer to use the more stringent alignment parameters, so I would need to talk to Steven about redoing the `bismark` alignments with the more stringent parameters. However, CpG methylation percentages are comparable across the two genomes and differing alignment parameters.

I also found that there was a different folder with `bismark` output from the new genome [here](https://gannet.fish.washington.edu/v1_web/owlshell/bu-github/project-oyster-oa/analyses/Haws-14/). This folder seems to only contain the coverage files and bedgraphs that are created after alignment.

### Genome comparison for `methylKit` results (or an attempt)

Now that I had a rough understanding of the differences in alignment between the genomes, I wanted to understand if that translated to a difference in methylation identification. The first thing I did was open [this R Markdown script](https://github.com/RobertsLab/project-oyster-oa/blob/594a171ef0c327716cacfc75bd9359246c26be40/code/Haws/04-methylKit.Rmd) (side note: I also updated R and R Studio since it's that time of year!). I also loaded the saved RData I had for `methylKIt` to avoid having to run `unite` again. However, when reviewing my script I noticed that I processed my files using ploidy treatment information specifically. I wonder if this impacts downstream `methylKit` analysis. So perhaps I would need to run things from scratch at some point after all.

Anyways, I needed to 1) confirm that the number of DML I previously counted and 2) identify DML using the new alignments. While I had my R Markdown script all loaded up, I remembered the biggest issue with running `methylKit` on a laptop: memory! R needs to process so much data that on a standard device the memory limit gets exhausted.

I posted [this issue](https://github.com/RobertsLab/resources/issues/2462) to request access to Gannet and Raven so I can run `methylKit` with the new alignments and compare the results.

### Going forward

1. Revise `bismark` methods and results
2. Revise `methylKit` methods and results
4. `methylKIt` randomization test
1. ATAC-Seq data integration
2. `KOG-MWU` for *Crassostrea* methylation comparison
5. Revise discussion
7. Revise introduction
5. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)


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
