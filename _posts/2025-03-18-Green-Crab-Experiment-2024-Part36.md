---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 36
tags: green-crab-cold
---

## Making figures for the manuscript

We're working on a manuscript deadline! I need to make some composite figures that highlight the results, so that's what we're going to do.

### Julia's results

First things first: I moved all of Julia's stuff into the same Github repository as the 2024 MA and WA data! Figured it would be easier to have everything in the same repository for publication. I opened up [this code](https://github.com/yaaminiv/cold-green-crab/blob/main/code/repeat-heat/03-TTR-analysis-julia.Rmd) and re-tooled some existing figures. I wanted to highlight differences in righting by timepoint and by the presence of the T allele. I also calculated some average values to include in the manuscript, which I put [here](https://github.com/yaaminiv/cold-green-crab/tree/main/output/repeat-heat/03-TTR-analysis).

![Image](https://github.com/user-attachments/assets/9109e991-04a8-4591-a281-7b0e9c8953a8)

**Figure 1**. Julia's results

### 2024 results

I made multipanel figures for my WA results in [this code](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/03-TTR-analysis-WA.Rmd) and placed the average calculations [here](https://github.com/yaaminiv/cold-green-crab/tree/main/output/WA/03-TTR-analysis).

<img width="653" alt="Image" src="https://github.com/user-attachments/assets/7971cef0-6b3c-43a5-9ccb-c361d3816ac8" />

**Figure 2**. WA results

Similarly, made the multipanel figure for my MA results in [this script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/MA/03-TTR-analysis-MA.Rmd) and saved calculations [here](https://github.com/yaaminiv/cold-green-crab/tree/main/output/MA/03-TTR-analysis). The MA is slightly different than the WA one because I needed to include a panel for genotype-specific differences.

<img width="1146" alt="Image" src="https://github.com/user-attachments/assets/9b888644-f95c-43cf-aee0-89c0a8e6dcdb" />

**Figure 3**. MA results

My next steps are to re-analyze Catlin's data and tackle the HOBO data.

### Going forward

1. Genotype Catlin's samples
1. Add Catlin's methods and results to manuscript
3. Analyze 2024 HOBO data
4. Analyze Julia's HOBO data
5. Analyze Catlin's HOBO data
6. Create an experimental setup figure
2. Outline introduction
3. Outline discussion
3. Determine methods for comparing population responses
3. Troubleshoot lipid assay protocol
5. Conduct lipid assay for crabs of interest

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
