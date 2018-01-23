---
layout: post
comments: true
title: Correlating Technical Replicates Part 9
---

## Better results?

I discussed some of [my concerns using the CV filtering](https://yaaminiv.github.io/Correlating-Technical-Replicates-Part8/) with Steven. We decided to look at the dataset I was using to make my boxplots when we found that some samples were missing way more transitions than others. The annotated dataset can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-30-Protein-Areas-for-Boxplots-Annotated.csv).

<img width="1276" alt="screen shot 2017-10-30 at 1 55 37 pm" src="https://user-images.githubusercontent.com/22335838/32197445-8b823346-bd81-11e7-8494-4c3c421e2cc8.png">

**Figure 1**. Poor technical replication of certain samples. There are 111 transitions, and some samples were missing close to all of them.

The number of missing transitions in samples ranged from 4 to 100. Because there seemed to be certain samples with poor technical replication, I decided to remove samples missing more than half of the transitions from my dataset. I removed samples O06, O103, O122, O128, O14, O145, O49, O52, and O71 and saved this as a [new file](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-30-Protein-Areas-for-Boxplots.csv).

Finally, I remade my boxplots using data with NAs instead of zeros so it wouldn't skew my graphs. I also found this handy dandy way to plot the individual data points on top of the boxplot!

`````
boxplot(boxplotData[,i] ~ boxplotData$Site, xlab = "Sites", ylab = "Abundance") #Create the boxplot
stripchart(boxplotData[,i] ~ boxplotData$Site, vertical = TRUE, method = "jitter", add = TRUE, pch = 20, col = 'blue') #Add each data point
`````

My final boxplots can be found [in this folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-10-24-Coefficient-of-Variation/2017-10-26-CV-20-Boxplots). Hopefully this will provide some insight into my data issues!

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
