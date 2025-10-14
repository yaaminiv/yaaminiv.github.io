---
layout: post
comments: true
title: Green Crab Experiment Part 50
tags: green-crab metabolomics lipidomics ASCA
---

## Things I realized were missing when I was updating methods and results

A real "bits and bobs" situation.

### Integration analysis

Worked some InDesign magic (lol) and smashed some figures together. Not a fan of how the circos plot looks next to the WCNA output. I want to make the text larger and remove the temperature lines. I also wonder if I can just remove compound names that weren't connected to something else. I'm going to have Ariana give me feedback on this figure so I can make it better.

<img width="2529" height="1053" alt="Image" src="https://github.com/user-attachments/assets/f6abe1d0-418a-4356-8857-484449908fab" />

**Figure 1**. Integration figure

### Metabolomics ASCA

When I started writing the results, I realized I probably should perform enrichment for the PCs of interest. So, I did. All the output can be found in [this folder](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/ASCA). I opened the loadings names I saved and pasted them into MetaboAnalyst for Pathway Enrichment. I removed any duplicate names when needed. For name matching, I also looked into synonyms from PubChem where appropriate.

PC 1:
- Fixed 3-Aminopiperidine-2,6-dione (comma was removing the last part of the name)
- Enriched pathways
  - valine, leucine, and isoleucine biosynthesis (same as 13ºC vs. 30ºC at day 22 comparison!!): isoleucine, leucine, threonine
  - glutathione metabolism: cadaverine, putrescine, glycine, 5-oxoproline

PC 2:
- No significantly enriched pathways
- valine, leucine, and isoleucine biosynthesis was the top one but not significant

PC 3:
- No significantly enriched pathways
- tyrosine metabolism was the top one but not significant

PC4:
- Two significantly enriched pathways
  - arginine biosynthesis (top pathway but not enriched for 13ºC vs. 5ºC at day 22): 2-oxoglutarate, glutamate, citruline
  - phenylalanine, tyrosine, and tryptophan biosynthesis: phenylalanine, tyrosine

### Lipidomics ASCA

I did the same thing with the lipidomics ASCA! The output can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/06-lipidomics-analysis/ASCA).

PC 1:
- No enriched LION terms

PC 2:
- 11 enriched LION terms! Several related to glycerophospholipids, saturation state, etc.

### Results figures

Of course I now had to go back into InDesign to figure out what to do with my results figures. I need to remove the WCNA information, add ASCA information, and add ASCA enrichment information (maybe).  I started with the easier lift, which was to add the ASCA component information to the multipanel with the PLS-DA.

<img width="1934" height="1346" alt="Image" src="https://github.com/user-attachments/assets/cac3d418-b180-4fee-821b-e80fd94abc46" />

**Figure 2**. PLS-DA and ASCA multipanel

Now to figure out what to do with the enrichment information. I needed to add enrichment information from PCs 1 and 4 from the metabolomics ASCA and PC 2 from the lipidomics ASCA.

<img width="997" height="784" alt="Image" src="https://github.com/user-attachments/assets/d55fa13c-b42c-4569-81e1-51cfe659148f" />

**Figure 3**. Enrichment information

So..........I'm not a huge fan of that figure. I am strongly contemplating moving it to the supplement.

Next step for me: get some feedback on my methods in writing group and start outlining the discussion (?!?!?!?!?!)

### Going forward

1. Outline discussion
5. Write discussion
2. Outline introduction
3. Write introduction
5. Write abstract

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
