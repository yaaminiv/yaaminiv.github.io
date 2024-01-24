---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 32
tags: green-crab-wc PCR gel
---

## Still at decontamination station

I SPOKE TOO SOON MY PCRs ARE STILL CONTAMINATED.

Like a fool, I decided to use reagents from 11/8 and 11/13 with my [previously extracted samples](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part28/), since [my previous gel didn't show any contamination](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part31/).

<img width="756" alt="Screenshot 2024-01-24 at 11 06 54 AM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/2c252f3d-3fa6-4848-8838-254ee0060e01">

**Figure 1**. Post-PCR gel for samples 35, 160, 163, 11, 185, 70, 98, ladder, 117, 126, 176, 168, extraction  blank, and PCR blank

When looking at the gel, it was hard to make out if there were one or two bands for samples. Like a fool, I threw my gel away after imaging so I couldn't just pop it on and run it for longer. The bands were also slightly smaller than what you'd expect. Primers would be ~50 bp, but the product should be ~126 bp, and everything looked smaller than the 100 bp band of the ladder. But, that could also be because the gel wasn't run out for a sufficient time, even though I used the same gel settings and 30 minute run time I had with all of my previous extractions.

Instead of setting up a new gel immediately, Sara suggested I could Qubit my samples to see if there was actually any DNA in them. I used the dsDNA HS kit to quantify DNA concentrations in a handful of samples.

**Table 1**. Qubit results for select samples

|        **Sample**        | **Concentration (ng/µL)** |
|:------------------------:|:-------------------------:|
|          15-035          |            1.81           |
|          15-160          |            7.29           |
|          15-163          |            4.33           |
|           5-070          |            10.8           |
|           5-126          |            5.26           |
|     Extraction Blank     |         "TOO LOW"         |
|         PCR Blank        |            14.7           |
| 11/13 reagents PCR Blank |            3.41           |

First off, there is some amount of DNA in my samples, so that's promising. What's more promising is that my extraction blank had "too low" of a DNA concentration, suggesting that my extractions themselves are clean! My post-PCR PCR blank had a high concentration of DNA in it. Since I had more Qubit reagents for one more sample, I also quantified the [11/13 PCR blank from yesterday](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part31/), and it showed a lower concentration of DNA than the PCR blank from today. This could be concerning, but it could also indicate that primers are forming longer sequences and getting quantified. Because of this last point, Carolyn suggested I run the gel again for a longer period of time.

<img width="612" alt="Screenshot 2024-01-24 at 11 07 28 AM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/0665b71f-5cb9-4ba0-8f9c-d5509ff5efb2">

**Figure 2**. Repeated post-PCR gel for samples 35, 160, 163, 11, 185, 70, 98, ladder, 117, 126, 176, 168, extraction  blank, and PCR blank

Running it out for longer confirmed my suspicions: my PCR was DEFINITELY contaminated. As a next step, Carolyn suggested I run a PCR with my DNA but use the *C. maenas* COI primers, a new NF H<sub>2</sub>O, and a new GoTaq Master Mix. If my PCR shows up uncontaminated, that means the issue is with the SMC primers. I'm also going to reserve a few spots in the gel to run out the PCR product for my 11/13 and 1/22 reagents.  

### Going forward

1. Work with Vanessa to leave decontamination station
2. Work with Vanessa to finish extracting respirometry samples
2. Work with Vanessa to continue with Chelex extractions, PCRs, and gels for TTR crabs
3. Perform Qubit assay with any sample consistently not showing up on a gel
4. Genotype remaining samples
3. Update methods and results of 2022 paper
4. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper
6. Update methods and results of 2023 paper

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
