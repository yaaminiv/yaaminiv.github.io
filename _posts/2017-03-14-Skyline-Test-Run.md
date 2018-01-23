---
layout: post
comments: true
title: Skyline Test Run
tags: DNR skyline DIA
---

## We have data!!!

Kind of. After meeting with Emma, we realized that for the purpose of the poster, we could use any oyster .blib file we had on hand. We used [an oyster seed .blib](https://github.com/sr320/course-fish546-2016/blob/master/data/oysterseed2.blib) for our Skyline analyses. A .blib file is a list of peptide spectra found in designated samples. Using a different .blib would change the reference spectra we would match the peptides in our samples with. While this method isn't ideal for a paper, it's good enough for a preliminary poster.

To use Skyline, we followed [these tutorial slides](https://github.com/RobertsLab/project-pacific.oyster-larvae/blob/master/Skyline-example-files-ETS.sky/slides01.pdf), steps 2 through 6. My Jupyter Notebook can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-03-14-Skyline-Test-Run.ipynb).

In the end, we exported the data as a .csv and modified it. The files I'll use going forward are below:

[Maximum Area Spectra](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_Skyline_20170314/Oyster-MaxArea-Proteinbased.csv)

[Average Area Spectra](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_Skyline_20170314/Oyster-AverageArea-Proteinbased.csv)

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
