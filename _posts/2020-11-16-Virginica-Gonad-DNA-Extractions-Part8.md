---
layout: post
comments: true
title: Virginica Gonad DNA Extractions Part 8
tags: virginica labwork gonad WGBS
---

## Checking female DNA quality

Today Grace and I ran [previously diluted DNA samples]() on the BioAnalyzer! I ordered new chips and reagents, which arrived on Wednesday afternoon with no delivery message. I happened to swing by on Saturday and learned that the reagents had arrived! I put them away, but they were at room temperature for ~72 hours once the ice packs had cooled. I didn't need to use the reagents today since I still had my previous set, but it's something to know for next time.

I prepared a chip using 1:20 diluted samples and ran it. The chip ran all the way through and there wasn't any issue with the ladder or marker, but I got "optical signal too high" errors on samples 41, 29, and 76:

![Capture](https://user-images.githubusercontent.com/22335838/99311449-7bb5d000-2811-11eb-8175-a54b57341122.PNG)

![Capture2](https://user-images.githubusercontent.com/22335838/99311452-7c4e6680-2811-11eb-8054-d0ef8f5c2518.PNG)

![Capture3](https://user-images.githubusercontent.com/22335838/99311453-7c4e6680-2811-11eb-992b-c097359d1b94.PNG)

**Figures 1-3**. Chip 1 electropherogram and gel results

When preparing this chip, I couldn't remember which wells I added the marker to or not, so I was concerned that I messed up the chip loading and that's why I had the error. We prepared a second chip to run, making sure to include the three problematic samples. The second chip had the same error!

**Figures 4-6**. Chip 2 electropherogram and gel results

![Capture4](https://user-images.githubusercontent.com/22335838/99311456-7ce6fd00-2811-11eb-83a7-2a775c3fed65.PNG)

![Capture5](https://user-images.githubusercontent.com/22335838/99311460-7ce6fd00-2811-11eb-9026-c7aa123acba6.PNG)

![Capture6](https://user-images.githubusercontent.com/22335838/99311463-7ce6fd00-2811-11eb-8c83-1d2df0c9ebdf.PNG)

Again, samples 29 and 76 had optical signal issues but sample 41 did not. Sample 36 also had an optical signal issue. Samples 54, 29, and 19 had issues with marker detection, but that's definitely my fault. I posted [this issue](https://github.com/RobertsLab/resources/issues/1029) to figure out why I kept getting that error. Hopefully I can figure out how to move forward with my quality checks!

### Going forward

1. Actually check DNA quality
2. Re-extract DNA from 16 and (maybe) 35
3. Test extraction protocol for male samples
4. Extract male samples and check quality
5. Send DNA and RNA for library preparation and sequencing

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
