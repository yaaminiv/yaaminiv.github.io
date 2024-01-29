---
layout: post
comments: true
title: Green Crab Experiment Part 26
tags: green-crab
---

## Experimental design edits

A couple more minor tweaks to work on while I'm thinking about them. First, I wanted to modify the experimental temperature figure. I returned to [this R Markdown code](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/01-temp-conditions.Rmd) to update the x-axis. Specifically, I wanted to add all of the dates onto the x-axis. After poking around, I learned that I needed to use `scale_x_datetime` to add the date information. I could then specify different "breaks" for how frequent the x-axis should be labelled. To get the full range of dates, I used 24 hour breaks:

```
geom_ribbon(aes(x = dateTime, y = avgColdTemp, ymin = coldMean - coldSD, ymax = coldMean + coldSD), fill = plotColors[3], alpha = 0.15) +
  geom_line(aes(x = dateTime, y = avgColdTemp), color = plotColors[3]) +
  geom_hline(yintercept = 5.7, colour = plotColors[3], linetype = 3) +
  labs(x = "", y = "") +
  scale_x_datetime(date_breaks = "1 day", date_labels = "%B %d") +
  scale_y_continuous(limits = c(5, 7.5), breaks = seq(5, 7.5, 0.5)) +
  theme_classic(base_size = 15) + theme(axis.text.x = element_text(angle = 40, vjust = 1.0, hjust = 1.0),
                                        plot.margin = margin(2,0,1,0))
```

Initially, I thought I would change the x-axis to only include experimental day, but Carolyn suggested keeping the original months/days because it highlights that the experiment was conducted in the summer. She suggested adding a secondary x-axis for this figure with experimental day. Then I began another deeper rabbit hole dive to figure out how to add a secondary x-axis to a plot! While I could use `sec.axis` within `scale_x_datetime` to specify a second axis, this second axis had to be some kind of derivative of the first. And experimental day couldn't really be derived from date (or, at least I couldn't figure out a way to do this). So I settled for some post-hoc InDesign modification (I am paying for the Adobe Creative Suite so I might as well use it).

![Screenshot 2024-01-29 at 11 05 11 AM](https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/c895273b-5416-4f28-83ff-ecb879537e68)

**Figure 1**. Updated experimental temperature figure

While I had InDesign open, I slapped together an experimental timeline. I wanted to highlight the different timepoints where I measured TTR, did respirometry, or took heart samples for molecular analyses.

![Screenshot 2024-01-29 at 1 52 21 PM](https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/453c85eb-29f0-4608-904e-d9d5ac92dedd)

**Figure 2**. Experimental timeline

Finally, I added some details about ESL filtering and brand names for my temperature controllers and immersion heaters.

Guess I gotta go back into metabolomic and lipidomics land and figure more of that stuff out!


### Going forward

1. Repeat lipidomics WCNA with temperature and day separately
4. Look into MetaboAnalyst discussion comments
5. Determine how to get more common lipid names that match MetaboAnalyst formatting
6. Reach out to Shelly to see if she tried anything else for lipid analysis
7. Try RaMP enrichment instead of KEGG for lipid sets
6. Normalize for "captivity" effect
7. Look into individual lipid classes from earlier papers
8. Determine if I can add physiology data to `mixOmics` analyses
9. Integrate metabolomics and lipidomics data

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
