---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 29
tags: green-crab-wc PCR gel
---

## Decontamination station (my least favorite stop on the extraction train)

I need to figure out why my [extraction and PCR blanks were so contaminated](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part28/). Here's how I did that:

### Step 1: Run PCR blanks

Carolyn suggested I make an all new PCR master mix with brand new SMC F, SMC R, GoTaq, and nuclease-free water. I made new SMC F and R primers (10 µL 100 mM working stock, 90 µL NF water) for today's master mix. I then ran a PCR with three blanks: my master mix from today, yesterday's master mix, and another master mix I made using reagents Julia left in the PCR reagents box. I then ran these three samples on a gel with a ladder.

<img width="455" alt="Screenshot 2023-11-09 at 3 47 59 PM" src="https://github.com/yaaminiv/apalm-hypoxia-omics/assets/22335838/1bce92f9-1880-47cb-b781-b901ef1efc71">

**Figure 1**. Gel for 11/8 MM blank, Julia MM blank, and 11/9 MM blank.

And they're all contaminated. Because the 11/8 blank is so much more contaminated than today's blank, I think that Carolyn's original suspicion that the weirdness with the Redfield fume hoods and lack of proper ventilation due to the fume hood issue yesterday may have kicked around a bunch of potential contaminants.

### Step 2: Clean the shit out of everything and run more PCR blanks

- RNAse AWAY tube boxes, outside of pipets, and tube racks then stuck them in the UV transilluminator
- 11/13 MM blank: BRAND NEW SMC F and SMC R, GoTaq, and NF H<sub>2</sub>O
- Ran 11/9 MM blank, Julia MM blank, and 11/13 MM blank

<img width="518" alt="Screenshot 2023-11-13 at 2 26 43 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/ce51d610-835a-4ffb-8d21-809d9853f587">

**Figure 2**. Gel for 11/9 MM blank, Julia MM blank, and 11/13 MM blank

11/9 and 11/13 blanks are contaminated but the Julia MM is somehow not contaminated...?!

### Step 3: Take pipets apart and clean the shit out of everything and run more PCR blanks

- Took apart 1000 µL, 250 µL, and 10 µL pipets to clean
- Two of each: 11/9 MM blank, Julia MM blank, 11/13 MM blank
- Ran out of OG Julia reagents while making MM so used the 11/8 reagents

<img width="813" alt="Screenshot 2023-11-17 at 2 13 50 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/45bcc98d-65dd-4f07-bb8b-391fdb5e3e41">

**Figure 3**. Gel for 11/9 MM blanks, Julia MM blanks, and 11/13 MM blanks

She's still contaminated but less so for the 11/9 and Julia reagents! So clearly cleaning whatever got thrown into the air by the fume hood issues is helping. I'll remake MM and try this same gel again. If the 11/13 blanks are still really bright, I'll take those out of the running for the decontamination saga.

### Going forward

1. Keep cleaning and running PCR blanks
7. Revisit Julia's genotypes
2. Finish extracting respirometry samples
2. Continue with Chelex extractions, PCRs, and gels for TTR crabs
3. Perform Qubit assay with any sample consistently not showing up on a gel
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
