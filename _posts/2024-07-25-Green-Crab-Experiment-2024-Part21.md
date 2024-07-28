---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 21
tags: green-crab-cold triglyceride-assay
---

## Trying some lipid blanks standards

After speaking with Melanie from Promega, we both agreed that the next best step was to run a plate with water blanks and standards to ensure those looked good. She also would reach out to some Promega lipid experts about effective homogenization methods, whether or not I could make the homogenate ahead of time, and why my homogenate values always read so low.

### Methods

1. Create a 40 µM dilution of the glycerol standard. Add 2 µL of the glycerol standard to 498 µL of the Glycerol Lysis solution to make an 80 µL dilution. Then, add 250 µL of the 80 µM dilution to 250 µL of Glycerol Lysis solution to create a 40 µL dilution. The 40 µL dilution should be used for all assay standards, and can be stored in the -80ºC.
2. Calculate the volume of reagents necessary for the assay. Only use as much as is needed that day, plus one extra.
3. Add the Reductase Substrate to the Glycerol Detection Reagent and incubate at room temperature for one hour. After the one hour incubation, add the Kinetic Enhancer and mix by inversion.
4. Add the lipoprotein to the Glycerol Lysis solution.
5. For each sample, create a homogenate and add to the appropriate wells. Add 25 µL of water as blanks and 25 µL of the 40 µM standard in triplicate.
6. Add the lysis solution, with and without lipase, to the appropriate wells. Cover the plate with foil.
7. Incubate at 37ºC for 30 minutes.
8. Add 50 µL of the glycerol detection solution to each well and incubate at room temperature for one hour.
9. Read on a GloMax luminometer.

### Notes

- Row A: DEPC water blank
- Row B: 40 µM glycerol standard
- Columns 1-3: With lipase
- Columns 4-6: Without lipase
- After the 30 min incubation, I noticed 45 µL of liquid in wells instead of the full 50 µL of liquid. Next time, I'll try adding a plate lid on top of the foil or adding some extra water in the oven to prevent drying.

### Results

<img width="1100" alt="Screenshot 2024-07-28 at 1 11 46 PM" src="https://github.com/user-attachments/assets/e55116a2-9d6b-49e1-8d16-276b6d92fec8">

**Figure 1**. Luminometer readings for blanks and standards

Good things first: all my empty wells have really low reading values! This is excellent because I've been cleaning and reusing the same plate, so it's a testament to how well the cleaning protocol (soap, water, dry, and a UV crosslink if I'm feeling fancy) works. The standard also looks good!! According to the manual, the values should be slightly higher for the without lipase than the with lipase values. This is because lipase can quench some of the signal if the luminescence value of whatever I'm trying to read isn't high enough.

The confusing things: my blanks are.....not blank. They're higher than the empty wells, and by an amount that is closer to the glycerol standard than the empty wells. My next step is to get a new DEPC water (or just use the aliquots in the -20ºC we use for molecular work) to determine if there's some glycerol contamination in the DEPC water that's currently open.

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
