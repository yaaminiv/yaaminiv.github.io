---
layout: post
comments: true
title: Green Crab Experiment Part 42
tags: green-crab TTR metabolomics
---

## Supplementing the metabolomics analysis

Since I identified pairwise VIP lipids between 30ºC and 5ºC, I figured I'd do the same thing with the metabolomics analysis. I compared the two extreme temperatures at day 3 and day 22 to determine if there were any metabolites or pathways unique to those contrasts that could shed more light on temperature acclimation processes.

### Day 3: 30ºC vs. 5ºC

There weren't any pairwise VIP metabolites identified between 13ºC and either 30ºC or 5ºC at day 3, so I wasn't sure if I'd see anything comparing 30ºC and 5ºC at day 3. Interestingly, [I identified 9 pairwise VIP metabolites](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/30v5-day3_VIP_metabolites.txt), all of which had higher abundance in 30ºC.

![Screenshot 2024-12-03 at 2 16 07 PM](https://github.com/user-attachments/assets/6a3be0b9-6af2-4917-99ba-f02e9ad0f031)

**Figure 1**. Pairwise VIP metabolites for 30ºC vs. 5ºC at day 3

I then imported this set of VIP metabolites into MetaboAnalyst to understand what pathways they were associated with. There were three pathways significantly enriched in this set: phenylalanine metabolism; phenylalanine, tyrosine, and tryptophan biosynthesis; and valine, leucine, and isoleucine biosynthesis.

<img width="1127" alt="Screenshot 2024-12-03 at 2 20 18 PM" src="https://github.com/user-attachments/assets/4e67fd2e-e3ec-4b8c-814d-b3d0d6d8506e">

**Figure 2**. Enriched pathways associated with pairwise VIP metabolites for 30ºC vs. 5ºC at day 3.

The valine, leucine, and isoleucine biosynthesis pathway was also associated with pairwise VIP metabolites for 13ºC vs. 30ºC at day 22, so these enrichment results could indicate that the investment in this process starts early on in 30ºC crabs. While not enriched, phenylalanine metabolism was one of the top processes associated with pairwise VIP metabolites for 13ºC vs. 5ºC at day 22. I should look into phenylalanine's role in cold acclimation a bit more to understand these results.

### Day 22: 30ºC vs. 5ºC

I repeated this process to identify pairwise VIP between 30ºC and 5ºC at day 22. The list can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/30v5-day22_VIP_metabolites.txt). Since there were VIP identified between 13ºC and either 30ºC or 5ºC at day 22, I used `anti_join` to identify VIP metabolites unique to the 30ºC vs. 5ºC contrast. Those unique VIP metabolites can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/05-metabolomics-analysis/metaboanalyst/unique-30v5-day22_VIP_metabolites.txt). Interestingly, a lot of these metabolites had higher abundance at 5ºC.

![Screenshot 2024-12-03 at 4 53 01 PM](https://github.com/user-attachments/assets/cec06016-e05d-48d4-9137-643500060fa1)

![Screenshot 2024-12-03 at 4 53 09 PM](https://github.com/user-attachments/assets/f9e34ac1-de52-4e44-bb0b-3ac16c79eb8f)

**Figures 3-4**. All and unique pairwise VIP metabolites for 30ºC vs. 5ºC at day 22

I then uploaded the unique pairwise metabolites from the 30ºC vs. 5ºC at day 22 contrast into MetaboAnalyst. The output can be found [here](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/05-metabolomics-analysis/metaboanalyst/unique-30v5-day22_metaboanalyst-results). Unlike the same contrast at day 3, there were no significantly enriched pathways.

<img width="1127" alt="Screenshot 2024-12-03 at 2 20 45 PM" src="https://github.com/user-attachments/assets/1a4efaca-34cb-4efb-a94a-9850bba74d9e">

**Figure 5**. Enrichment results for pairwise VIP metabolites for 30ºC vs. 5ºC at day 22.

I'll need to add this information into the paper. I don't think it'll be in the main text, but in the supplement, perhaps as a justification for why I focused on the contrasts I did and perhaps as some justification for looking into phenylalanine.

### Going forward

1. Address remaining comments on paper draft
2. Update methods and results
1. Interpret metabolomic and lipidomic WCNA correlations
2. Integrate physiology and -omics data with WCNA
3. Metabolomics and lipidomics DIABLO analysis
4. Outline discussion
5. Write discussion
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
