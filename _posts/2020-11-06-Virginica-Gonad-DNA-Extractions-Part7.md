---
layout: post
comments: true
title: Virginica Gonad DNA Extractions Part 7
tags: virginica labwork gonad WGBS
---

## Checking female RNA quality

Sam suggested we can move forward with the RNA BioAnalyzer kit in [this issue](https://github.com/RobertsLab/resources/issues/1020), so that's what Grace and I did! Although he didn't think we needed to run a blank chip, I wanted to run one just in case any of the reagents had degraded. I prepared a blank RNA chip with the ladder and reagents. 

![Capture](https://user-images.githubusercontent.com/22335838/98583096-b13e4480-2278-11eb-9541-97e52ba93d3e.PNG)

**Figure 1**. Blank RNA chip. Any errors were because there was no RNA detected

The ladder and marker were all recognized by the machine so we were good to go! We loaded a new chip with 11 samples, but there was an issue with the machine. It said that Port 1 on the chip couldn't be read, so I set the electrode cleaning chip on the machine for ~ 1-2 minutes, then reloaded the chip. Port 1 on the chip still couldn't be read, so I put the electrode cleaning chip on for 30 minutes as we prepared gel dye matrix and another chip. When we prepared the chips, we thawed the ladder and samples on wet ice. We loaded the chip again and were able to get data.

![Capture3](https://user-images.githubusercontent.com/22335838/98583107-b4d1cb80-2278-11eb-9e0c-380b67af3eca.PNG)

![Capture5](https://user-images.githubusercontent.com/22335838/98583109-b56a6200-2278-11eb-9624-9f69ee6e12bf.PNG)

**Figures 2-3**. RNA BioAnalyzer chip run

I'm a little concerned that the samples are on the shorter side. This might be because when we made the 1:100 dilutions for the BioAnalyzer chip, we vortexed the diluted samples to mix them thoroughly, then vortexed them again before we put them on the chip. It's possible that all the vortexing ended up shearing the RNA. For our next chip, we loaded samples without vortexing them.

![Capture4](https://user-images.githubusercontent.com/22335838/98583108-b56a6200-2278-11eb-9067-809939192229.PNG)

![Capture7](https://user-images.githubusercontent.com/22335838/98583111-b602f880-2278-11eb-9a57-93b8dc777b0d.PNG)

**Figures 4-5**. Second BioAnalyzer chip rrun

Again, I was worried about the samples being sheared when vortexing, but it's possible that the undiluted samples (which are too high of a concentration to run on the BioAnalyzer) are not sheared since they haven't undergone any vortexing or mixing. Now that I know my RNA quality looks decent, I just need to wait for the DNA chips so I can check the DNA quality.

### Going forward

1. Check DNA on the BioAnalyzer
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
