---
layout: post
comments: true
title: Gigas Broodstock DNA Extraction Part 6
tags: manchester labwork gigas-broodstock
---

## Protocol test: Round 4

Today, Kaitlyn fulfilled her dream of getting to do labwork on the weekend (hey, she volunteered to help even after I told her she didn't need to). As I mentioned in my [planning post](https://yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part5/), we followed the [PAXgene protocol](https://yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part3/) to extract DNA from histology blocks with three changes:

- We had 2 methods for lysis: TissueTearor at setting 1 for 10 seconds, or vortexing with glass beads for two minutes. This is slightly different from the [previous TissueTearor methods](https://yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part4/). I thought I should use a milder TissueTearor setting so I can compare DNA shearing across methods with the Seeb's BioAnalyzer.
- We no longer have the Thermomixer. Instead, we're incubating the tubes on a heat block for 10 minutes, then vortexing at maximum speed for one minute. Total incubation time is still 60 minutes.
- I'm no longer removing RNA from the DNA I'm extracting. I would like to isolate DNA and RNA from the same samples so I can compare epigenetic and transcriptomic information.

![img_9445](https://user-images.githubusercontent.com/22335838/44960798-8eef5600-aeba-11e8-8a4b-8ff7dfcbc755.JPG)

**Figure 1**. Heat block incubation set-up.

I am using four tissue samples — split between two tubes for each method — with eight samples total. Kailyn pre-scraped two samples on Friday (see her lab notebook [here](https://yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part3/)) and scraped the other two samples this morning (IDs [here]()). Because the tissue is already preserved in wax, I wanted to see if scraping tissue into a tube beforehand affected DNA yield. Tissue scraping is a rate-limiting step, so being able to scrape out tissue the night beforehand would be a real timesaver!

### Method notes

- Step 9: We incubated the tubes at 37ºC for 25 minutes so the ethanol was fully evaporated
- Step 12-13: I used the TissueTearor at setting 1 for 10 seconds in the sample. Before use and after samples, I cleaned the TissueTearor in ethanol for 20 seconds, then rinsed in two different batches of DI water for 20 seconds each.
- Step 15: We incubated the tubes on the heat block at 56ºC for ten minutes, then vortexed for one minute. We repeated this process 6 times for a total incubation time of one hour. There was no gelatinous pellet, so the proteinkinase K digestion must have gone well!
- Step 17: Skipped!
- Step 18: The tubes were kept at room temperature for 3 minutes while the heat block reached temperature. We incubated tubes on the heat block at 80ºC for ten minutes, then vortexted for one minute. We repeated this process 6 times for a total incubation time of one hour.
- Step 27: No DNA came out of the T4-V2 and T6-TT tubes! I think there might have been something clogging the spin column...?
- Step 28: I made a working solution wiht 2626.8 µL of buffer and 13.2 µL of dye. S1 = 161.34; S2 = 9957.65

### Results

**Table 1**. Initial mass and DNA concentrations for tissue samples processed. 

| **Tube** | **Initial Mass (g)** | **DNA Concentration (ng/µL)** |
|----------|----------------------|-------------------------------|
|   T4-TT  |        0.0207        |              3.06             |
|   T4-V2  |        0.0207        |              N/A              |
|   T5-TT  |        0.0204        |              7.66             |
|   T5-V2  |        0.0207        |              N/A              |
|   T6-TT  |        0.0210        |              N/A              |
|   T6-V2  |        0.0214        |              2.04             |
|   T7-TT  |        0.0214        |              N/A              |
|   T7-V2  |        0.0210        |              N/A              |

I got DNA, even without a Thermomixer! My yields, however, were pretty inconsistent. They were either 2 ng/µL and above, or N/A. I think part of the problem was the initial mass of tissue: Kaitlyn went above 0.0200 g, maybe because I wasn't specific about being as close to 0.0200g as possible. The TissueTearor method worked better than the vortexing method. I think the big takeaway here is still variability between tissues.

### Going forward

Well, I know the method still works without the Thermomixer! If I'm doing a different bisulfite assay that only needs 1 ng/µL of DNA, I can definitely move forward with my samples. If not, it's back to testing.

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
