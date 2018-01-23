---
layout: post
comments: true
title: Selecting SRM Targets Part 5
---

## I have draft transitions!

Emma wasn't kidding when she said sorting through proteins on Skyline takes a while. Even so, I have [preliminary target transitions](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_TransitionSelection_20170707/2017-07-07-Preliminary-Transitions/2017-07-07-Preliminary-Target-Transitions-Evalues.csv)! My steps are laid out in [a Jupyter notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-07-07-SRM-Target-Identification-in-Skyline.ipynb).

To come up with the transition list I sent Emma and Steven, I made a [duplicate Skyline document](http://owl.fish.washington.edu/spartina/DNR_Skyline_SRM_20170707/Gigas-7-7-Transition-List.sky.zip) to edit. I followed the rough instructions in [Emma's Skyline tutorial slides](https://github.com/RobertsLab/project-pacific.oyster-larvae/blob/master/Skyline-example-files-ETS.sky/slides01.pdf) to delete bad quality proteins, peptides and transitions.

**Deletion criteria**:

- Delete a protein if it only has one associated peptide
- Delete a peptide if
  - There are less than three transitions
  - There is too much peak intereference, so the peak isn't clearly defined (i.e. a sloppy peak)
  - There is missing data for samples
- Delete a transition if
  - It is a precursor ion
  - It has low abundance (want to keep the three most abundant transitions)
  - It is noisy
  
For example, the least abundant transition in this peak (highlighted in red) is noisy...

![unnamed-1](https://user-images.githubusercontent.com/22335838/28142739-2d891a9a-6717-11e7-91d0-61b6ef1a7d8b.png)

...so I deleted it.

![unnamed-2](https://user-images.githubusercontent.com/22335838/28142742-2eea219a-6717-11e7-8575-276c80a70708.png)

While the above photos show the same peptide and transitions, the sample number is different, leading to this peak! For this reason, I needed to use the deletion critera for all samples. If there were more than two peaks between samples, I deleted the peptide.

I first sorted through the proteins Steven marked as "interesting" in [my shortlist](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_TransitionSelection_20170707/2017-07-07-Preliminary-Transitions/2017-07-07-Protein-Shortlist-Evalues.csv). Some of those interesting proteins had poor quality peaks, so I looked for proteins with similar annotations to examine instead. Overall, I narrowed 9000+ proteins down to 23 proteins, with 64 peptides and 228 transitions. I was stuck as to how to narrow it down even further, so I figured I would have Steven and Emma help out. Tomorrow, I'll use their feedback to narrow down my list down to 100-150 transitions.

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

