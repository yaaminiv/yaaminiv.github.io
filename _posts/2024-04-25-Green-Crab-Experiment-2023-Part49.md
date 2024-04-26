---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 49
tags: green-crab-wc PCR gel
---

## Continued layover in contamination station

### April 15

On Friday, I had Vanessa do a PCR with previously extracted samples. Based on the [previous gel](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part47/), I knew that the extraction blank had DNA contamination. However, I wanted to see if she could get a clean PCR blank if she cleaned everything. She used the molecular bench closest to the door, cleaned the outside of everything, put things in the UV crosslinker, than did the PCR. Today, I ran the gel.

<img width="653" alt="Screenshot 2024-04-15 at 5 25 47 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/27bb2805-cf96-4f8e-a4ff-db6825ac3346">

**Figure 1**. Gel image for 134, 161, 164, 181, 66, ladder, 114, 123, PCR blank, EB

In a super confusing turn of events, the extraction blank AND PCR blank are still contaminated. The bands are fainter than the sample bands, but I don't know what could be causing the contamination! When I talked to Carolyn, she suggested running a bunch of blanks, but prepping them on both benches to see if there's a bench/pipet effect.

### April 16 and 17

Time to figure out what happens when I run a bunch of blanks! I intended to run PCR blanks from reagents from 4/6, 4/10 (Vanessa's PCR), and to make a new set for 4/16. For each reagent date, I would run two blanks from the left bench (closest to the door) and the right bench (the bench Vanessa and I have been using). When I got to lab, I cleaned both benches, both sets of pipets and pipet boxes, and tube racks. I didn't put anything through the UV Crosslinker, just to see what would happen if I cleaned the normal amount.

When I opened my box of reagents, I didn't see any new SMC F or R from 4/10, even though I asked Vanessa to make it! I wonder if she mistakenly made some more in the existing 4/6 tubes. So, I ran blanks on both benches from 4/10 (4/6 SMC F and R, 4/10 GoTaq, 4/10 NF H<sub>2</sub>O) and 4/16 (new SMC F, R, GoTaq, and NF H<sub>2</sub>O).

![Screenshot 2024-04-17 at 4 06 18 PM](https://github.com/yaaminiv/wc-green-crab/assets/22335838/48b008a0-d805-437f-9a25-004a3d4c3301)

**Figure 2**. Gel image for 4/10 L1, 4/10 L2, 4/16 L1, 4/16 L2, ladder, 4/10 R1, 4/10 R2, 4/16 R1, 4/16 R2

So, the 4/6 blanks all came up not blank, but the 4/16 blanks looked...good? All 4/6 samples have a PCR product band, a primer dimer band (the boxed ones), and a smear for remaining reagents. There seems to be a slight bench effect, when the left bench looks better than the right bench.

My next step is to run yet another set of blanks using the 4/16 reagents and make up some new reagents. Hopefully this will show me that the 4/16 and new reagents are good to use and I can start re-extracting samples!

## April 22

I ran more blanks! I asked Vanessa where she put the SMC F and R primers she made on 4/10, and she said she put them in the 10 µM working stock box. So basically, the valid place to put them! I pulled those primers, the 4/16 L and R bench primers, and made new SMC F and R for each bench. I cleaned the normal amount and bounced between benches to prep samples. I used an opened bag of eppendorf tubes for the L bench and a new bag for the R bench.

![Screenshot 2024-04-22 at 5 04 17 PM](https://github.com/yaaminiv/wc-green-crab/assets/22335838/2ee974a3-3781-4c1b-969f-5b0a7a722cc6)

**Figure 3**. Gel image for 4/10 L1, 4/10 L2, 4/16 L1, 4/16 L2, 4/22 L1, 4/22 L2, ladder, 4/10 R1, 4/10 R2, 4/16 R1, 4/16 R2, 4/22 R1, 4/22 R2

We're getting somewhere! Looks like the 4/10 samples are contaminated, so I'll toss those reagents. Interestingly, the bands are a lot brighter on the R bench than the L bench. The 4/16 samples look smeary but okay on the L bench, but there's clear contamination on the R bench samples. The 4/22 samples look acceptable on both benches, but they look much better on the L bench. I guess I have a bench-specific contamination issue!

My next step is to do a nice, extreme cleaning with UV Crosslinking (outside only) and try extracting samples on the L bench.

## April 25

Moment of truth! I ran actual samples today. I first did a normal cleaning of the bench (not the outside UV Crosslinking I originally thought I would do) since the normal cleaning on the L bench seemed to suffice in previous blank tests. I tossed the SMC F and R primers from 4/10 and from the right bench (4/16 R and 4/22 R). I kept the GoTaq and NF H<sub>2</sub> from 4/16 but not 4/10, since my previous gel showed contamination across benches for 4/10 but not 4/16.

I extracted five samples (134, 161, 164, 181, 66) and an extraction blank. I ran these samples through PCR and on a gel. Additionally, I added the two 4/22 L blanks from my previous test to the gel run. I made a new Chelex solution for my extractions and used the 4/22 L reagents for the PCR.

<img width="588" alt="Screenshot 2024-04-25 at 11 29 13 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/e1722490-5b37-41bd-86de-4fb6db99a822">

**Figure 3**. Gel image for 134, 161, 164, 181, 66, ladder, extraction blank, PCR blank, 4/22 L1, and 4/22 L2

WE'RE BACK IN BUSINESS! Clear bands for my samples, and the blanks are actually blank!!

### Going forward

1. Work with Vanessa to leave contamination station
1. Re-extract and PCR samples with faint gel bands
1. Work with Vanessa to continue with Chelex extractions, PCRs, and gels for remaining crabs
4. Genotype remaining samples
4. Examine HOBO data from 2023 experiment
5. Determine best statistical approach for analyzing performance data
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
