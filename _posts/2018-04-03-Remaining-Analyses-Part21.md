---
layout: post
comments: true
title: Remaining Analyses Part 21
tags: DNR SRM skyline
---

## The curious case of the catalase(s)

You thought this series was over? April Fools, it's not. I finally addressed one of Emma's comments on my DNR paper that's been sitting there since February. Emma wasn't convinced that the two catalases I used as [protein targets](https://yaaminiv.github.io/Correlating-Technical-Replicates-Part10/) were actually separate proteins, so she suggested that I see if the protein sequences can be aligned. To do this, I opened up my Skyline file and looked at the protein sequences.

![unnamed](https://user-images.githubusercontent.com/22335838/38276438-dc97bc82-3749-11e8-8637-a1ec9e839ab2.png)

**Figure 1**. Protein sequence for CHOYP_CATA.1.3|m.11120

![unnamed-2](https://user-images.githubusercontent.com/22335838/38276440-ddfbc8f2-3749-11e8-8da1-13a0c3b2bcdf.png)

**Figure 2**. Protein sequence for CHOYP_CATA.3.3|m.21642

The letters code for different amino acids. The blue highlighted letters are peptides that in the Skyline document, and the black bolded letters are other peptides in the same protein. Lo and behold, you can align the sequences. Guess they are the same catalase after all!

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
