---
layout: post
comments: true
title: WGBS Samples Part 3
tags: manchester labwork gigas-broodstock WGBS
---

## Planning for WGBS

Last year I [extracted *C. gigas* broodstock DNA](https://yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part9/) and [sent off pooled gonad samples for WGBS](https://yaaminiv.github.io/WGBS-Samples-Part2/). Since I don't have enough DNA to do MBDBS with these samples, I'm seriously considering WGBS. Based on [this issue from Sam](https://github.com/RobertsLab/resources/issues/962), Zymo WGBS sequencing for 10 samples should be affordable and doable. Plus, my previous samples were sequenced by Zymo so there would be consistency there!

After a chat with Steven, I decided to investigate if I can sequence 5 samples per treatment. I would need at least 500 ng and 20 ng/µL for each sample. I looked at [this sample spreadsheet](https://github.com/RobertsLab/project-oyster-oa/blob/master/data/Manchester/2018-10-09-Broodstock-DNA-Extractions/2018-10-09-DNA-Extraction-Results.csv) updated the amount of DNA I had left after creating the pooled samples. Then, I identified samples from each treatment that had at least 500 ng of DNA. I tried balancing extraction batches and maturation stage, choosing only female samples since we saw a female-specific carryover effect. I also looked at the histology blocks to see if there was any tissue left for paired RNA extraction! I felt good about the sample if more than half of the tissue was left for future extractions. Here are the samples with caveats if they exist:

**Low**:

- 02-T1
- 04-T3
- 06-T1: No tissue left for RNA extraction
- 07-T2
- 08-T2: Less than half of tissue sample left for RNA extraction
- 10-T3

**Ambient**:

- 11-T4
- UK-01: No more than half of sample left for RNA extraction
- UK-02
- UK-03
- UK-04: This sample has less than 500 ng, but I wanted another sample that was a maturation stage of 2 and not indiscriminate like the rest of the ambient samples. I posted [this issue](https://github.com/RobertsLab/resources/issues/971) to see if there's any wiggle room. According to Sam, there may not be but Zymo might be more flexible.
- UK-06: Less than half of tissue sample left for RNA extraction

My next steps are to see if I need to do any library preparation so these samples can be sequenced.

### Going forward

1. Confirm Zymo quotes and finalize samples for WGBS
2. Double check DNA quality and concentrations
3. Prepare libraries (if needed)
4. Extract RNA from gonad samples

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
