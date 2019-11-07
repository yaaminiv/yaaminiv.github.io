---
layout: post
comments: true
title: WGBS Analysis Part 8
tags: manchester gigas-broodstock GOslim
---

## Adding GOslim annotations to genes

Last time I worked on my talk, [I had trouble annotating genes with GOslim information](https://yaaminiv.github.io/WGBS-Analysis-Part7/). I returned to [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb) to get GOslim terms for my WSN talk.

I was able to get pretty far in the GOslim annotation pipeline, but I was unable to sort my `blast` output using `sort -V`. I posted [this issue](https://github.com/RobertsLab/resources/issues/776). Sam told me that I needed to install `homebrew` on the machine I was using `genefish`. I was pretty sure I already installed `homebrew`, but I did it again! Sam also pointed out that the user names in my *C. gigas* and [*C. virginica* notebooks](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-08-28-blastx-to-GOslim.ipynb) were different. Turns out I installed `homebrew` on my laptop, which is what I used to annotate *C. virginica* genes with GOslim terms! Installing `homebrew`, then `coreutils`, allowed me to use `gsort -V` and successfully sort my `blast` output.

Once I annotated genes with GOslim terms, I separated out biological process and molecular function GOslim terms. Within each category, I noticed that there were some repeat lines. If a gene has multiple DML, it's likely the same GOslim term would be assigned. I removed duplicate lines using `uniq`. I saved unique [molecular function GOslim terms](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/Blastquery-GOslim-MF.sorted.unique) and [biological process GOslim terms](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/Blastquery-GOslim-BP.sorted.unted.unique). I also had some genes with "uncharacterized biological processes" GOslim terms that also had other specific GOslim terms assigned.

![Screen Shot 2019-10-29 at 1 12 58 PM](https://user-images.githubusercontent.com/22335838/68424993-03c2bd80-015a-11ea-824b-8d8a5693de44.png)

**Figure 1**. Gene with specific and unspecific GOslim term assigned.

I wanted to get rid of these specific lines but didn't know how, so I posted [this issue](https://github.com/RobertsLab/resources/issues/778). Before I could figure out how to code this, Steven suggested I just remove all "uncharacterized biological processes" entries. Sam said I could use `grep --invert-match` to find these entries and not include them in the output file. I saved unique, characterized GOslim entries for [molecular function](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/Blastquery-GOslim-MF.sorted.unique.noOther) and [biological processes](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-09-15-DML-Analysis/Blastquery-GOslim-BP.sorted.unique.noOther).

### Recoloring DML location plot

On the plane to WSN, I realized my black and white pots were, scienficially speaking, *blah* compared to the rest of my color-coordinated presentation. I decided to pick a color scheme from R that matched my presentation, then use it for my plots.

The color scheme that best matched my Powerpoint was the Red-Purple palette from `RColorBrewer`. I selected five colors from this palette and reversed the order so the darkest color would be used first. I saved the palette as a new object. To see if the scheme was color-blind friendly, I used `dichromat`.

<img width="693" alt="Screen Shot 2019-11-05 at 1 48 39 PM" src="https://user-images.githubusercontent.com/22335838/68424994-03c2bd80-015a-11ea-82bb-88069cc167f2.png">

**Figure 2**. Testing color scheme with `dichromat`.

Looking at the plot, I could still distinguish between the five colors being used, so I decided to continue using it for my talk.

![Screen Shot 2019-11-05 at 1 49 37 PM](https://user-images.githubusercontent.com/22335838/68424995-045b5400-015a-11ea-8bc9-9eb6ed7743c8.png)

**Figure 3**. Recolored plot with location of all CpGs and DML.

### Biological processes plot

Now that I had GOslim data *and* a color scheme, I was ready to make a GOslim plot! I started [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-10-31-Gene-Enrichment/2019-10-31-Gene-Enrichment-with-GO-MWU.Rmd) to do just that while flying from Seattle to San Diego. There's a bunch of code for GO-MWU in there from my *C. virginica* project I didn't edit, but I figured I might as well keep it in case I decide to use a similar method with the *C. gigas* data.

I started by counting the frequency of each GOslim term in my dataset, then converted those frequencies to percentages. I saved this information [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2019-10-31-Gene-Enrichment/2019-10-31-BP-GOSlim-DML-Gene-Annotations.csv). I then grouped my GOslim terms into broader categories to assign colors:

- **Metabolic Processes**: DNA Metabolism, RNA Metabolism, Protein Metabolism, Other Metabolic Processes
- **Stress Response**: Stress Response
- **Development**: Cell Cycle and Proliferation, Developmental Processes, Cell Organization and Biogenesis
- **Cell Activity**: Cell-Cell Signaling, Cell Adhesion, Death, Signal Transduction, Transport

I plotted this intially as a pie chart, but it didn't look that great. I decided to use a horizontal barplot instead, and organize the bars by category and percent of genes with DML in each category.

<img width="713" alt="Screen Shot 2019-11-05 at 1 50 42 PM" src="https://user-images.githubusercontent.com/22335838/68424997-045b5400-015a-11ea-83ff-4b6db1756af7.png">

**Figure 4**. Biological processes represented by genes with DML.

I'm pretty happy with the format of this graph, so I think I'll apply it to my *C. virginica* data too! Now it's time to update my talk (found [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/presentations/Venkataraman_FRI_1515.pptx)).

### Going forward

1. Present at WSN!
2. Return to *C. virginica* data and finish. that. paper.
2. Determine sequencing protocol for *C. gigas* adults and larvae

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
