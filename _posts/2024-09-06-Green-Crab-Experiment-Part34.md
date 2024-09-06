---
layout: post
comments: true
title: Green Crab Experiment Part 34
tags: green-crab lipidomics
---

## Lipidomics enrichment

The moment I've been waiting for (and kind of dreading): lipidomics enrichment. All analyses were run in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/06-lipidomics-analysis.Rmd).

### Overall and pairwise VIP annotations

But first, I wanted to annotate which overall and pairwise VIP were present in various WCNA modules! Those lists can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/metaboanalyst).

### Manual pathway enrichment

I wanted to do a rough analysis with phosphatidylethanolamine (PE) and phosphatidylcholines (PC) compounds, since Chapelle 1978 and 1986 found them most responsive to temperature. I also know that cell membranes are responsive to temperature, becoming more fluid in cold temperatures and more rigid in warm temperatures. In addition to PE and PC compounds, I also examined the other kids of glycerophospholipids — phosphatidylserine (PS) phosphatidylinositol (PI) — since glycerophsopholipids are involved in cell membranes. Phosphatidic acid (PA) is another glycerophospholipid, but it is not present in this dataset. Finally, I wanted to look at triglycerides, which are important energy storage molecules (and to provide myself some nice preliminary data for my cold tolerance triglyceride assays).

The best method I could think of to examine these differences was to create heatmaps of averaged lipid abundance! For each heatmap, I also included row annotations indicating if something was an overall VIP, pairwise VIP, both, or neither.

![Screenshot 2024-09-06 at 4 05 34 PM](https://github.com/user-attachments/assets/32da61b2-c377-4c49-9d52-ce7b34456ca4)

![Screenshot 2024-09-06 at 4 05 45 PM](https://github.com/user-attachments/assets/ddc47f18-8148-489f-a164-e96a3a09d20d)

![Screenshot 2024-09-06 at 4 05 53 PM](https://github.com/user-attachments/assets/01322050-060e-4994-aeaf-8510a103281f)

![Screenshot 2024-09-06 at 4 06 01 PM](https://github.com/user-attachments/assets/281eb21e-45ab-4841-9a72-1e26c228317c)

**Figures 1-4**. Heatmaps for PE, PC, PS, and PI lipid abundance

I really couldn't derive much from this besides some went up and some went down. I'll have to think on this more to figure out if there's a better way to pull out meaning from these pathways to interpret the data AND nicely compare it with the Chapelle data.

### LION enrichment

The main event! Trying to get enrichment information from the list of overall VIP lipids. I first set out to try enrichment with MetaboAnalyst, just for an easy 1:1 comparison with the metabolomics data. Long story short, barely any lipids were recognized by the platform, and the compounds that were recognized didn't always have KEGG pathway information. Seeing how those KEGG IDs are crucial for the Pathway Analysis module, I decided to switch over to LION.

[LION/web](http://lipidontology.com) is a web-based GUI that can be used for lipid ontology analysis. Similar to GO terms, LION terms synthesize lipid information from another lipid database, [LIPIDMAPS](https://www.lipidmaps.org), biophysical data, lipid functions, and organelle associations. Most importantly, it is lipid-specific! I used the Target-list mode, meaning I uploaded my targets (overall VIP) and a background (all known lipids). The target mode uses the `topGO` algorithm to conduct a Fisher's exact test for enrichment. LION could recognize 95% of my lipid list! The LION output can found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/metaboanalyst/LION-enrichment-report).

<img width="1430" alt="Screenshot 2024-09-06 at 9 13 12 AM" src="https://github.com/user-attachments/assets/e9eef998-847c-48c6-b093-57fd89cb0e7a">

<img width="966" alt="Screenshot 2024-09-06 at 9 14 55 AM" src="https://github.com/user-attachments/assets/2a22aa02-afcf-47ed-8c2e-6717cd1bce10">

**Figures 5-6**. Screenshots from LION enrichment

I imported the [enrichment results](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/06-lipidomics-analysis/metaboanalyst/LION-enrichment-report/LION-enrichment-results.csv) into R to create a plot similar to the enrichment ratio one I had for the pathway analysis. Instead of an enrichment ratio, I used -log(FDR) on the y-axis. It may make sense to use that on the y-axis for the metabolomics plot too, just for consistency.

![Screenshot 2024-09-06 at 4 20 15 PM](https://github.com/user-attachments/assets/6f69d88e-6165-44ed-b29a-62139a7453f4)

**Figure 7**. Lipid enrichment results

I showed these results to Carolyn and Ariana, and now I have a couple of thoughts:

- How do I parse the glycerophospholipid manual analysis?
  - Should I plot just VIP compounds?
  - Is there a way to look at results based on number of double bonds, since more double bounds would increase fluidity?
- How do I make sense of the VIP analysis and enrichment?
  - In talking with Ariana, I realized it probably makes more sense to conduct enrichment with *pairwise* VIP and not *overall* VIP. With the overall VIP, it's tough to figure out which molecule is responsible for driving a specific difference.
  - Looking at pairwise VIP would allow to me to answer my questions about short- vs. long-term acclimation at a specific temperature or a specific time more clearly

So back to the drawing board we go!

### Going forward

1. Pairwise VIP enrichment for metabolomics
2. Pairwise VIP enrichment for lipidomics
3. Mess around with manual pathway analysis for lipidomics
1. Update methods
2. Update results
8. Determine if I can add physiology data to `mixOmics` analyses
9. Integrate metabolomics and lipidomics data

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
