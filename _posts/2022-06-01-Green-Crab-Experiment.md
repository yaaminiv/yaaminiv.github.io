---
layout: post
comments: true
title: Green Crab Experiment
tags: green-crab respirometry
---

## Respirometry trial

Today I learned how to do respirometry from Chris! The goal of today's trial was to learn the methods, determine what troubleshooting I need to do before proceeding with my experiment, and get an idea of how much time respirometry would take to complete so I can ask for assistance.

### Considerations

Before the trial I read a handful of *C. maenas* respirometry papers and identified best practices:

- Starve crabs before respiration trials for 24-36 hours (respiration trials should take place prior to water change and feeding) (Clark et al. 2013)
- Perform assay in the dark (low lights, black covering on respiration chambers) (Clark et al. 2013; Penney et al. 2016)
- 1:20 to 1:100 g:L crab to water volume ratio (Clark et al. 2013), should allow for sporadic movements (Mégevand et al. 2022)
- Acclimation to respiration chambers: overnight with intermittent flushing (Penney et al. 2016), one hour in aerated seawater (Mégevand et al. 2022)
- Magnetic stirrers under mesh grids to increase mixing (Mégevand et al. 2022)
- Wipe chambers with 70% ethanol and rinse with deionized water after measuring background respiration (Penney et al. 2016)

Overall the best respirometry set up is an intermittent flow system, which can seal or flush chambers with oxygenated seawater based on the amount of oxygen detected in the chambers. However, I don't have access to that! Neel has several Pyrex 90 mm x 50 mm (270 mL volume) containers with sensor spots on them that I can use with Ann's FireSting oxygen probes to measure oxygen saturation over time.

### Methods

Here's the protocol I followed for today:

- Place crabs in Pyrex containers and place parafilm over the conatiners. Use rubber bands to secure the parafilm. Poke holes to allow for oxygen exchange and place back into a holding tank. Allow crabs to acclimate for an hour
  - One crab was fine with the parafilm! Another wiley one poked a hole through the parafilm and escaped. I found some mesh screen and placed that on top of the chamber with a rubber band instead. My guess is the crab was a little too small for the chamber so it was able to maneuver too well, while the other crab could make intermittent movements but didn't have enough room to escape.
- Obtain FireSting and PC computer with software. Plug in O<sub>2</sub> meter, O<sub>2</sub> probes, and temperature probe. For each probe that's plugged in, change the settings (gear button) to measure percent oxygen saturation and calibrate the upper limit with ambient air.
- Start logging a new session
- Obtain a container with a crab. Empty all water and add new aerated seawater. Add enough seawater so it "domes" on top. Place parafilm over the container, ensuring there are no bubbles prior to sealing. After parafilm is sealed, add a Pyrex container lid and a weight (brick) on top to prevent oxygen intrusion.
- Quickly place the sealed container with the crab in the holding tank and hold up the probe to the sensor spot to measure oxygen. Measure until oxygen saturation dips below 80%.
- Once oxygen saturation is below 80%, remove the crab from the container and quickly reseal the chamber, avoiding any bubbles. Measure background respiration in the chamber for as long as the crab's respiration was measured. After measuring background respiration, stop logging.

### Results

The crab drew down oxygen pretty quickly! I'm not sure if it was just too large for the chamber or if crabs just respire quickly. The crab wasn't moving very much so I'm more confident that this is a routine metabolic rate as opposed to a stress rate. I only measured one crab because the smaller escape artist was moving around too much. I probably need to acclimate that size crab to the chamber for a longer period.

The water temperature was ~15ºC in the holding tank and the chamber reached 80% oxygen saturation in about 15 minutes. I could probably let the crabs reach 70-75%, which is similar to Mégevand et al. (2022). If I expect respiration to increase at 30ºC and slow down at 5ºC, I estimate needing 20-60 minutes per round for the crabs and background respiration together.

### Going forward

1. Measure crab:water ratio to determine if chambers are the correct size for crabs
2. Add clamps to chambers so I don't have to hold the probe to the chamber
3. Obtain flexible mesh for acclimation
9. Check temperature fluctations on all tanks
4. Buy air line fittings and finish air line installation for all tanks
5. Add heaters and chillers to all tanks
5. Add holding tanks for respirometry
6. Cover respiration chambers with paper or black plastic to simulate night conditions
8. Cover ESL tanks with dark plastic or paper to block out light
7. Add lights and cycler to ESL

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
