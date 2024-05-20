---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 54
tags: green-crab-wc gel
---

## Rerunning samples on a gel

[Yesterday](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part53/) and the [time before that](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part52/), my gel bands were pretty squished together and I had a hard time determining if some samples had a strong enough band for sequencing. Today I reran those samples.

**Gel**:

1. Obtain PCR product, ladder, and TriTrak from the 4ºC fridge and move into gel room.
2. Microwave pre-made gel mix in the microwave for 3 minutes in 1.5 minute intervals. Be sure to swirl the bottle to mix the gel liquid.
3. Allow the bottle with gel mix to cool for a few minutes. While cooling, tape off sides of the gel tray.
3. After bottle has cooled enough to the touch but the gel mix is still in liquid form, pour the mix into the taped gel tray slowly to avoid bubbles. Use combs to make wells by slowly inserting them into the gel to avoid bubbles.
4. Allow gel to harden for at least two hours.
5. Once gel is hardened, place in gel box with 1x TAE buffer. If an extra gel was made, slide and place in a Ziploc bag with TAE buffer. Place that Ziploc bag in the same drawer as the pre-made gel mix, away from the light.
6. Obtain a piece of parafilm and pipet 1 µL of TriTrak dye for each sample, extraction blank, and PCR blank. On the parafilm, the dye will bead up. Place the dots far enough apart to avoid contamination.
7. Take 6 µL of PCR product and mix with a 1 µL dot of TriTrak dye by pipetting up and down at least 10 times. Then, pipet up 6 µL and load gel. Repeat for each sample until halfway through samples. Then, add 6 µL of diluted ladder + dye mix, and continue with remaining samples.
8. Run gel for 30 minutes.
9. After running gel, remove gel tray and image.

### Notes

- Samples: 72, 151, 170, 178, 180
- I used 3 µL of ladder for my gel
- Ran the gel for 35 minutes instead of 30 minutes to ensure proper separation

### Results

<img width="626" alt="Screenshot 2024-05-17 at 3 09 44 PM" src="https://github.com/yaaminiv/wc-green-crab/assets/22335838/14f38386-97b5-4241-9d74-25de66bd4171">

**Figure 1**. Gel image for 72, 151, 170, extraction blank (5/14), PCR blank (5/14), ladder 178, 180, extraction blank (5/16), and PCR blank (5/16)

I need to re-extract 72! But the other samples have average or bright bands, meaning that they're good for sequencing.

### Going forward

1. Re-extract 72
2. Send remaining samples for sequencing
4. Genotype remaining samples
5. Identify samples for gene expression and metabolomics
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
