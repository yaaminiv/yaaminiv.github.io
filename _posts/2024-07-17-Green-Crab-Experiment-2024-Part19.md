---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 19
tags: green-crab-cold triglyceride-assay
---

## Developing a lipid assay

Heidi and I spent some time this week trying the [Promega Triglyceride-Glo Assay](https://www.promega.com/products/energy-metabolism/lipid-metabolism-assay/triglyceride-glo-assay/?catNum=J3160).

### Methods

1. Calculate the volume of reagents necessary for the assay. Only use as much as is needed that day, plus one extra.
2. Add the Reductase Substrate to the Glycerol Detection Reagent and incubate at room temperature for one hour. After the one hour incubation, add the Kinetic Enhancer and mix by inversion.
3. Add the lipoprotein to the Glycerol Lysis solution.
4. For each sample, create a homogenate and add to the appropriate wells. Add 25 µL of water as blanks and 25 µL of the standard in triplicate.
5. Add the lysis solution, with and without lipase, to the appropriate wells.
6. Incubate at 37ºC for 30 minutes.
7. Add 50 µL of the glycerol detection solution to each well and incubate at room temperature for one hour.
8. Read on a GloMax luminometer.

### 2024-07-17

#### Notes

- For the homogenate, we picked samples 103 and 112. We weighed two aliquots of ~22-25 mg of tissue on the microbalance and homogenized with disposable pestles.
- After homogenization, we added 30 µL of water and vortexed.
- We then added 15 µL of the final homogenate to each well, then added an additional 10 µL of water to each well to make the volume 25 µL
- Initially we used a clear plate, but were told to use a white plate for luminometer readings instead! So we were given a white plate to use.

#### Results

<img width="871" alt="Screenshot 2024-07-19 at 11 13 54 AM" src="https://github.com/user-attachments/assets/29ef6683-6226-47e8-800e-8860d5cdc52b">

**Figure 1**. Luminometer readings for test plate

- We have really low concentrations overall, so our samples were too dilute
- We're seeing higher concentrations in the samples without lipase than with lipase, which doesn't make sense.
- The final volume of the samples was less than 100 µL, so some evaporation must have occurred.
- Our standards are also reading low, likely due to evaporation.

### 2024-07-18

Based on yesterday's test, Melanie at Promega suggested we try a dilution series of homogenate, and nail down good concentrations for the standard.

#### Notes

- Used foil to prevent evaporation for the 37ºC incubation, which went well!
- Columns 1-3 have lipase, columns 4-6 don't have lipase
- Dilution series
  - Row A: 25 µL homogenate
  - Row B: 20 µL homogenate, 5 µL water
  - Row C: 15 µL homogenate, 10 µL water
  - Row D: 10 µL homogenate, 15 µL water
  - Row E: 5 µL homogenate, 20 µL water
  - Row F: Water blank (25 µL)
  - Row G: Glycerol standard (25 µL at 20 µM)
- Homogenized with disposable pestle, and by passing the homogenate through a 1000 µL pipet tip to break up the chunks further
- Things that went wrong
  - Homogenization was really difficult. Even with the quantity we needed (474 mg), we still had chunks that were difficult to distribute evenly amongst the different plate wells. I don't think each well got the appropriate amount of homogenate
  - We didn't have enough homogenate for any wells in Row E besides E1
  - Accidentally added 25 µL glycerol detection reagent before the 37ºC incubation to A4-A6
  - Added 50 µL of glycerol detection reagent twice to Row D, so only G1 received any detection reagent

#### Results

<img width="949" alt="Screenshot 2024-07-19 at 11 15 06 AM" src="https://github.com/user-attachments/assets/2074df15-3884-4047-94d4-4b68720cf600">

**Figure 2**. Luminometer readings for test plate

- We need to redo our standard (obviously)!
- The columns with lipase are 10x higher than the columns without lipase, so that seems like a good sign
- Even with 25 µL of homogenate the concentrations are really dilute.

I'll need to discuss with Melissa further to figure out how to move forward!

### Going forward

1. Troubleshoot genotyping
2. Clarify methods for average TTR analysis
2. Individual-level TTR data analysis
3. Troubleshoot lipid assay protocol
5. Conduct lipid assay for crabs of interest

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
