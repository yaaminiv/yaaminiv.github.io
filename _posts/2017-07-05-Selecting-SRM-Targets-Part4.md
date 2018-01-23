---
layout: post
comments: true
title: Selecting SRM Targets Part 4
---

## Differential Expression + Protein Function = Targets?

Using the [MSstats output](https://yaaminiv.github.io/Selecting-SRM-Targets-Part3/) I got from looking at pairwise comparisons with both Site and Eelgrass Condition, I looked at protein annotations to identify a short list of potential targets. In [this Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-07-05-Examining-Protein-Annotations.ipynb), I first merged my pairwise comparisons with both [protein annotations](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_TransitionSelection_20170707/2017-07-07-Preliminary-Transitions/2017-07-05-Gigas-Annotations.csv) and [accession codes](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_TransitionSelection_20170707/2017-07-07-Preliminary-Transitions/background-proteome-accession.txt). 

I went through the list in search of proteins related to oxidative stress, hypoxia, heat shock, immune resistance and shell formation. To do this, I used the search command within Excel and copied the protein names into a new list. I looked for key terms in both the name of the protein and biological process GOterms. I also searched specifically for versions of the following proteins:

- Superoxide dismutase
- Catalase
- Glutathione peroxidase
- Cytochrome P450 (CYP1A)
- Glutathione-S-transferase
- MDR proteins
- Nacrein

Finally, I created a [long list](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_TransitionSelection_20170707/2017-07-07-Preliminary-Transitions/2017-07-06-Protein-Longlist.xlsx) of about 100 proteins and shared it with Steven. We merged it with [e-value information](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_TransitionSelection_20170707/2017-07-07-Preliminary-Transitions/2017-07-07-Gigas-Annotations-Evalues.csv) and he selected proteins I should start with for SRM target identification. The next step is to use Skyline!

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
