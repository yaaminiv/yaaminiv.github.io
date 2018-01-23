---
layout: post
comments: true
title: PECAN Update 4
tags: DNR pecan DIA
---

## Reformatting PECAN inputs

[Jupyter notebook found here](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-03-08-Formatting-PECAN-Inputs.ipynb)

After comapring my PECAN inputs with Laura's we realized that I had been using the undigested proteome as a database this whole time! I had the digested file, but just didn't use it. To remedy this, I did the folliwng (see Jupyter notebook for explicit details):

- Converted the digested proteome to a .tabular file format
- Removed extraneous columns from my .tabular digested proteome (needed only the "Protein Name" and "Sequence" columns
- Updated the file path in my background proteome path list
- Selected 5 samples to analyze
- Updated mzML file path list
- Uploaded all PECAN inputs to OWL
- Downloaded folder from OWL into Documents directory on Roadrunner

Everything is now set up for Sean to run the code!

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
