---
layout: post
comments: true
title: Aggregating Metaanalysis Data
tags: metaanalysis
---

## I became well-aquainted [NCBI SRA database](https://www.ncbi.nlm.nih.gov/sra/) today

It's not the most user-friendly database, but it's still pretty great! Based off of [Sean and Grace's previous searches](http://rpubs.com/seanb80/267942) and some of my own searches, I found *C. gigas* methylation data. You can read up on my methods [here](https://github.com/RobertsLab/paper-gigas-metaanalysis/blob/master/notebooks/2017-04-18-Data-collection.ipynb).

I recorded the following metadata for each run I found:

- Project code
- SRA study code
- Experiment code
- Experimental Design
- Sample type
- Library information (Name, Platform, Strategy, Source, Selection, Layout)
- Run code
- Run information (Spots, Bases, Size, GC Content)
- Submitted by
- Date published

All of the metadata is in [this spreadsheet](https://github.com/RobertsLab/paper-gigas-metaanalysis/blob/master/data/metaanalysis-data-sources.xlsx). Tomorrow, I'll download the data (apparently it's not a straightforward process).

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

