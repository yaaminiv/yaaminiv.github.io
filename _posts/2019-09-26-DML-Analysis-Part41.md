---
layout: post
comments: true
title: DML Analysis Part 41
tags: virginica MBDSeq GO-MWU
---

## One last set of analyses before dedicated writing

Special Issue deadline: Oct. 1
Today: Sept. 26
Hours to get this wrapped up: at least 56

I told myself that after today, I would focus solely on fishing up the draft paper text before going down the rabbit hole of refining figures and cleaning up analyses. Here's what I got done today:

### Fixing the promoter track

During the last E2O meeting, I was tasked with checking that the promoter track I used accounted for mRNA directionality. I loaded my upstream flank track and mRNA track into IGV to check.

![Screen Shot 2019-09-26 at 3 20 16 PM](https://user-images.githubusercontent.com/22335838/65728873-2a78d900-e071-11e9-9096-ff60ee1fb990.png)

**Figure 1**. IGV screenshot with upstream flanks (promoter track) and mRNA confirming how I forked up.

In [this notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2018-11-01-DML-and-DMR-Analysis.ipynb), I went to correct my promoter track. I started with the 1000 bp upstream and downstream flanks I already created for each mRNA transcript. Looking at the tracks, I remebered that there was a strand indication! Some transcripts are on the forward strand (+) while others are on the reverse strand (-). To fix the promoter track, I needed the upstream flank information to only include forward strand transcripts, and the downstream flanks should only include reverse strands. I used `grep` to get this done:

```
!grep ".	+	." 2019-05-29-Flanking-Analysis/2019-05-29-mRNA-Upstream-Flanks.bed \
> 2019-05-29-Flanking-Analysis/2019-05-29-mRNA-Upstream-Flanks-Forward-Strands.bed

!grep ".	-	." 2019-05-29-Flanking-Analysis/2019-05-29-mRNA-Downstream-Flanks.bed \
> 2019-05-29-Flanking-Analysis/2019-05-29-mRNA-Downstream-Flanks-Reverse-Strands.bed
```

I then combined the individual tracks using `cat`:

```
!cat \
2019-05-29-Flanking-Analysis/2019-05-29-mRNA-Upstream-Flanks-Forward-Strands.bed \
2019-05-29-Flanking-Analysis/2019-05-29-mRNA-Downstream-Flanks-Reverse-Strands.bed \
> 2019-05-29-Flanking-Analysis/2019-05-29-mRNA-Promoter-Track.bed
```

With the new promoter track, I recharacterized overlaps between various genomic features and DML.  There were some changes to the number of overlaps, but it didn't change the overall pattern. Time to write!

### Going forward

2. Update methods and results
3. Update paper repository
4. Outline the discussion
5. Write the discussion
6. Write the introduction
7. Revise my abstract
8. Share the draft with collaborators and get feedback
9. Post the paper on bioRXiv
10. Submit to the Special Issue

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
