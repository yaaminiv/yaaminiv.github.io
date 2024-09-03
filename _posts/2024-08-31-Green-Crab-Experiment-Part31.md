---
layout: post
comments: true
title: Green Crab Experiment Part 31
tags: green-crab metabolomics
---

## (Somewhat) finalized metabolomics figures

...and other miscellaneous tasks I realized I needed to do while writing.

### Overlaps between overall and pairwise VIP

While writing the methods and results sections of the manuscript, I realized that I needed to make a clear distinction between overall VIP and pairwise VIP metabolites! The overall VIP metabolites are those identified from the PLS-DA with all data. The pairwise VIP are identified from pairwise contrasts and have an additional p-value attached to them. When going through [my code](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/05-metabolomics-analysis.Rmd), I saw that sometimes I identified overlaps between a feature of interest and overall VIP, and sometimes features and pairwise VIP, but it was rare I did both. I meticulously went through to ensure I characterized overlaps between metabolites in WCNA modules, metabolites in pathways of interest, or metabolites in significantly enriched pathways with both the overall and pairwise VIP. The naming conventions aren't the greatest, but all of the tables can be found in [this folder](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/metaboanalyst).

### Pathway figures

Once I got all of those values, I realized I needed to add overall VIP score and pairwise VIP t and *P*-values to my figures.

![Screenshot 2024-09-03 at 10 06 08 AM](https://github.com/user-attachments/assets/9ba03a3a-3bf5-416d-8d1a-2ea8b358dcbe)

**Figure 1**. Combined respiration plot

![Screenshot 2024-09-03 at 10 05 59 AM](https://github.com/user-attachments/assets/b4efc8bc-3811-4a87-9b55-1d98e0844a4b)

**Figure 2**. Calcium signaling plot

![Screenshot 2024-09-03 at 10 06 30 AM](https://github.com/user-attachments/assets/b730dfa8-885b-48bd-840e-5987ae9f2c94)

**Figure 3**. Valine, leucine, and isoleucine biosynthesis plot

![Screenshot 2024-09-03 at 10 05 48 AM](https://github.com/user-attachments/assets/b36a8e33-c9c5-499d-ae5f-862076d32856)

**Figure 4**. Arginine biosynthesis plot

![Screenshot 2024-09-03 at 10 06 16 AM](https://github.com/user-attachments/assets/f5d1f6e4-d846-4f3c-856a-34d2c73ff357)

**Figure 5**. Glutathione metabolism plot

![Screenshot 2024-09-03 at 10 05 32 AM](https://github.com/user-attachments/assets/780b2f7a-c7db-46ef-9835-703c68ce3899)

**Figure 6**. Alanine, aspartate, and glutamate metabolism plot

### Multipanel metabolomics figures

The last thing I did was make a multipanel plot for all my metabolomics data! I settled on four panels: PLS-DA, WCNA heatmap, heatmap of overall VIP metabolites, and pathway enrichment ratio.

<img width="886" alt="Screenshot 2024-09-02 at 10 57 34 AM" src="https://github.com/user-attachments/assets/a8f84d22-bd75-432c-a551-bdce0321c16a">

**Figure 7**. Multipanel of overall metabolite results

### Going forward

1. Repeat lipidomics WCNA with temperature and day separately
3. Repeat lipidomics WCNA with all lipid data
7. Try lipid-specific enrichment with LION
1. Update lipidomics methods
2. Update lipidomics results
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
