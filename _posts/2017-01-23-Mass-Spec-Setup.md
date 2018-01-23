---
layout: post
comments: true
title: Mass Spec Set-up
tags: DNR mass-spec DIA
---

## Mass Spectrometer Set-up

The day we've been gearing up for has finally arrived — it's mass spec time! We're using an Orbitrap Fusian Lumos machine to run our analysis.

We met Emma down at the [University of Washington's Proteomic Resource facility](https://proteomicsresource.washington.edu/) in South Lake Union. Emma previously prepared columns for us to use with traps packed up to about 3 cm. She attatched the trap to the mass spectrometer with the frit on the machine's side. We tested the flow through the trap. In the BSM program, we set our method to "trapping," set Solvent A to 5%, and used 0.2 for the flow. After ensuring that these settings worked, we ramped up the flow to 0.5.

Emma then attached the 30 cm analytical column and changed the methods in BSM to an analytical flow with 5% Solvent B and a 0.2 flow. We lost pressure at about 3500 psm but couldn't figure out why that was happening. We switched back to the trapping method, with 5% Solvent A and 0.2 flow and found a leak on the trap input side. Emma reattached the trap and continued to run the trapping method, and then switched to analytical when the flow was stable.

Once again, the pressure crashed, so we replaced the analytical column with a new one. The new column was equilibrated with 5% Solvent B and a 0.2 µL/minute flow rate for about ten minutes. She saw a solvent droplet visible at the column tip at about 2700 psi. She then switched the method to 5% Solvent A, keeping flow at 0.2 µL/min for 10 minutes. Holding flow steady, she changed Solvent A to 50% and let that run for 10 minutes. The pressure was approximately 1200 psi during this time. Ramping flow to 0.3 µL/min lead to a pressure of about 3500 psi. Emma then cut the column to aout 27 cm and switched Solvent B back to 5%, keeping flow at 0.3 µL/minute. Presure reached 4200 psi with no leaks!

While Emma was setting up the machine, Laura and I pipetted 15 µL of each sample into new glass autosampler vials. It is important to add the samples in with no bubbles. We also created a quality control (10 µL quality control solution and 30 µL Final Solvent) and blank (60 µL Final Solvent). We will need to continually fill the blank with Final Solvent as analysis continues, since it takes 3 µL for every blank. 

We decided to run Laura's geoduck samples first, including technical dupliates. I won't run my samples on the machine until the end of the week. Just gotta hang tight!

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
