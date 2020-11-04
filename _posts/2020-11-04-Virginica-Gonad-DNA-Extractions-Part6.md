---
layout: post
comments: true
title: Virginica Gonad DNA Extractions Part 6
tags: virginica labwork gonad WGBS
---

## Checking female DNA and RNA quality

Today Grace and I ran samples on the BioAnalyzer...or at least tried to! I prepared a gel dye matrix using reagents purchased in July. I added 15 µL of the blue dye to the gel matrix, vortexed, and centrifuged it for 15 minutes at 2300 rcf. Oddly enough not all the matrix made it through the filer, which is weird because I definitely used those same settings in the past. Grace centrifuged it for 5 more minutes to try and get more of the matrix through the filter. I then prepared the BioAnalyzer chip. I messed up my first chip since I pushed down the syringe, but it didn't get caught on the clip the way it was supposed to. I was able to properly prepare my second chip and ran it!

The results did not look good. There was an issue with ladder peak detection, and the BioAnalyzer was unable to recognize the marker.

![Capture](https://user-images.githubusercontent.com/22335838/98169835-dc541d00-1ea1-11eb-9735-5e2808570eb0.PNG)

![Capture2](https://user-images.githubusercontent.com/22335838/98169838-de1de080-1ea1-11eb-9ceb-6ee1e0e972d7.PNG)

**Figures 1-2**. Chip 1 BioAnalyzer results.

When [reviewing my previous BioAnalyzer struggles](https://yaaminiv.github.io/Sperm-DNA-MBD-Enrichment-Day3/), I noticed that I used dilutions since my samples were more concentrated than the upper detection limit of the high sensivitity assay! Grace made 1:20 dilutions of each sample using 1 µL of sample and 19 µL of nuclease free water, and I loaded those samples on another chip (which turned out to be my last one). Once again the assay didn't give us good results. 

![Capture6](https://user-images.githubusercontent.com/22335838/98169844-e0803a80-1ea1-11eb-8cba-9ca67ef25363.PNG)

![Capture7](https://user-images.githubusercontent.com/22335838/98169847-e0803a80-1ea1-11eb-820d-c31a3abb602a.PNG)

**Figures 3-4**. Chip 2 BioAnalyzer results

I had errors that indicated not all ladder peaks were found, and some samples didn't have any marker detection either. Since the kit is almost four months old and coming up on its expiry date, it's possible that the reagents are degraded. I also noticed that I didn't move the clip down to the bottom position before using the syringe, so that could have affected the chip integrity. Unfortunatley I had no more chips to test either theory. I put it in a request for another purchasing order and will circle back to it later! I need to see if I can continue extracting the male samples even though I can't check female DNA quality, and posted [this issue to check](https://github.com/RobertsLab/resources/issues/1021). I updated [this spreadsheet](https://github.com/RobertsLab/project-oyster-comparative-omics/blob/master/metadata/Virginica-Adult-DNA-RNA-Extractions.csv) to include the remaining sample volume. I noticed that sample 35 now had less than 500 ng of DNA, but since there's more than 400 I might be okay.

Since we couldn't do anything more with the DNA samples, I figured we'd run the RNA samples. Grace went to get the RNA kit ladder from the -80ºC, but there was no more left! We checked for the RNA kits in the 4ºC fridge and found a kit from 2019. Even though it was unopened, there's a chance that the reagents are past their four-month prime. I posted [this issue](https://github.com/RobertsLab/resources/issues/1020) to ask Sam for his advice!

### Going forward

1. Check DNA and RNA quality on the BioAnalyzer
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
