---
layout: post
comments: true
title: Green Crab Experiment Part 2
tags: green-crab experimental-design
---

## Setting up the experimental facility

I'm documenting my struggles with ESL so I can remember what *didn't* work in case future Yaamini wants to try any of these things again.

### Manipulating temperature

My biggest struggle is getting the tanks to be the right temperature! My original goal was to have tanks at 0ºC, 15ºC, and 30ºC. For this summer, 0ºC is pretty impossible. I'm using the cold room in ESL to get my tanks as cold as possible! At first, Phil helped me set the temperature to 2ºC. I put a few tanks and HOBO loggers in there to see how steady that held. A few days later, I noticed the cold room wasn't...cold. I reached out to Phil and neither of us were able to set the temperature any lower. He called in maintenance, and they told us that the chiller had transformed into a big block of ice and was unable to actually cool anything! Once the chiller was defrosted and worked on, the cold room was once again up and running. We decided to set the temperature at 5ºC since that's what the cold room was originally set at, and we didn't want to set the temperature any colder and overwork the chiller and require more maintenance.

**Figures 1-3**. Data from HOBO loggers showing temperature fluctuations in the cold room

<img width="1270" alt="Screen Shot 2022-06-07 at 4 21 30 PM" src="https://user-images.githubusercontent.com/22335838/176957411-a538c419-00ef-469b-840b-d63f9ade0506.png">

<img width="1270" alt="Screen Shot 2022-06-07 at 4 22 24 PM" src="https://user-images.githubusercontent.com/22335838/176957416-4fdcd3e0-9159-4162-93bf-2e9c74a0d2ba.png">

<img width="1270" alt="Screen Shot 2022-06-07 at 4 23 26 PM" src="https://user-images.githubusercontent.com/22335838/176957420-1cf42c98-3580-4397-91a5-463b2572d606.png">

Next came the temperatures in ESL. I initially had a standing water bath that I would refill every day, but the temperatures in the building itself warmed the seawater, so my tanks were way too warm! Phil suggested I create a flowing water bath with a mix of 10ºC and 15ºC water to keep the tanks cool. That helped with the ambient tank temperatures:

**Figures 4-5**> Data from HOBO loggers showing temperature fluctuations in the main ESL tanks

<img width="1252" alt="Screen Shot 2022-06-15 at 1 00 03 PM" src="https://user-images.githubusercontent.com/22335838/177197198-abe6cdec-7be0-49eb-84bf-582003e61c27.png">

<img width="1252" alt="Screen Shot 2022-06-15 at 1 00 46 PM" src="https://user-images.githubusercontent.com/22335838/177197202-6d1b3c6a-02ea-4ec3-8a1f-d75fd52dad43.png">

<img width="1252" alt="Screen Shot 2022-06-15 at 1 04 10 PM" src="https://user-images.githubusercontent.com/22335838/177197203-b451987d-a5e0-4c47-81b1-3f716cbf1957.png">

<img width="1252" alt="Screen Shot 2022-06-15 at 1 05 52 PM" src="https://user-images.githubusercontent.com/22335838/177197205-1439a2e9-e429-42fe-9253-aa1d189b83bc.png">

<img width="1252" alt="Screen Shot 2022-06-15 at 1 07 06 PM" src="https://user-images.githubusercontent.com/22335838/177197206-f7196a53-7984-429e-88dc-d38568b1a447.png">

With the flowing water bath, one immersion heater didn't suffice to raise the tank temperatures to 30ºC. I had to add two heaters to each 30ºC tank so the water would bet to the appropriate temperature.

### Miscellaneous tasks

Besides working on temperature control, I did the following:

- Measured crab:water ratio to determine if chambers are the correct size for crabs. The crab I used for the respiration test was 22.06 g, so I roughly have the 1:100 ratio needed for accurate respirometry measurements.
- Bought air line fittings to use the manifold in ESL along with some splitters. While I have the air line set up, the air going into each tank is not equal when I use the splitters, so I need to modify this.
- Measured the volume of the crab tanks: 21 cm x 26 cm x 41 cm (22.386 L)

### Going forward

1. Add clamps to chambers so I don't have to hold the probe to the chamber
3. Obtain flexible mesh for acclimation
4. Even out air flow to all tanks
6. Cover respiration chambers with paper or black plastic to simulate night conditions
8. Cover ESL tanks with dark plastic or paper to block out light
9. Get refractometer for salinity measurements
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
