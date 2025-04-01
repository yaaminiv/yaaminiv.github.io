---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 41
tags: green-crab-cold mixed-effects-models
---

## Revising the water temperature analysis

I want to revise [the temperature analysis](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part38/). Since I had some temperature loggers fail during the experiments, there are timepoints with NAs. ANOVAs don't do well with unbalanced sample sizes, so I am shifting to a Kruskall-Wallis testing approach. Details for conducing this test can be found [here](https://www.r-bloggers.com/2022/05/how-to-perform-the-kruskal-wallis-test-in-r/).

### But first...some figures

Before I get to this...I remade some figures and forgot to post about it!

In [this script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/repeat-heat/03-TTR-analysis-julia.Rmd), I [added custom shaded areas](https://www.statology.org/ggplot2-shaded-area/) highlighting which treatments and timepoints were part of the heat shocks.

![juliaTTR](https://github.com/user-attachments/assets/7d74cd16-848c-4b62-9cf6-d7cc6bdb7118)

**Figure 1**. TTR plot for Julia's data

In [this script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/MA/03-TTR-analysis-MA.Rmd) and [this script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/03-TTR-analysis-WA.Rmd), I modified the temperature labels to reflect the actual temperatures experienced by crabs.

![Image](https://github.com/user-attachments/assets/5dd9dcd2-9a20-4884-aa0a-49ea9f7dd35c)

**Figure 2**. MA TTR

![Image](https://github.com/user-attachments/assets/f4c78b16-7c01-47c9-81c1-7f71729d694b)

**Figure 3**. WA TTR

### Julia's temperature analysis

Alright, back to the main event. I conducted Kruskal-Wallis tests in [this script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/repeat-heat/01-temp-conditions-julia.Rmd). I examined the differences in temperature across conditions for the two heat pulses:

```
tempPulse1Stats <- kruskal.test(temperature ~ condition, data = tempPulse1Long)

tempPulse2Stats <- kruskal.test(temperature ~ condition, data = tempPulse2Long)
```

As expected, the conditions at Pulse 1 were different between treatments, but the test also identified significant differences during the second pulse. I investigated this further by looking at temperature differences by tank:

```
tempPulse2TankStats <- kruskal.test(temperature ~ tank, data = tempPulse2Long) #Investigate differences by tank

tempPulse2TankPairwise <- pairwise.wilcox.test(tempPulse2Long$temperature, tempPulse2Long$tank,
                                               p.adjust.method = "bonferroni") #Control tanks (10 and 12) are not significantly different from eachother. Tanks 11 and 13 (treatment) are significantly different from all other tanks. Tank 13 seems to have the largest difference between all tanks.
```

Tank 13 being different from all other tanks makes sense based on the [temperature averages I calculated previously])(https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part38/). I saved the statistical output from the temperature-specific tests [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/repeat-heat/01-temp-conditions/kruskal-wallis-test-output.csv) and the tank-specific tests [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/repeat-heat/01-temp-conditions/pairwise-wilcox-test-output-pulse2vtank.csv). Since tank temperatures are different during the second pulse, I created a new mulitpanel plot showing the temperatures by tank during the second pulse, and also included vertical lines where TTR was measured. To add the vertical lines, I used `geom_vline`, and used `as.POSIXct(ymd_hms())` to put in the date and time for the TTR timepoints based on code from [this post](https://stackoverflow.com/questions/5388832/how-to-get-a-vertical-geom-vline-to-an-x-axis-of-class-date).

```
mainPlot <- tempClean[4:395,] %>%
  dplyr::select(dateTime,
                temp10a, temp10b,
                temp11a, temp11b,
                temp12a, temp12b,
                temp13a, temp13b) %>%
  rowwise() %>%
  mutate(., controlTemp = mean(c(temp10a, temp10b,
                                 temp12a, temp12b), na.rm = TRUE),
         treatmentTemp = mean(c(temp11a, temp11b,
                                temp13a, temp13b), na.rm = TRUE)) %>%
  ggplot(., aes(x = dateTime, y = treatmentTemp)) +
  geom_line(color = plotColors[1]) + #Average treatment temperature
  geom_segment(aes(x = tempClean$dateTime[31], xend = tempClean$dateTime[108],
                   y = treatmentTempStatsOverall$meanTempPulse1[2], yend = treatmentTempStatsOverall$meanTempPulse1[2]),
               color = plotColors[1], linetype = 3) + #Pulse 1 treatment segment
  geom_ribbon(data = tempClean[31:108],
              aes(x = dateTime,
                  y = treatmentTempStatsOverall$meanTempPulse1[2],
                  ymin = treatmentTempStatsOverall$meanTempPulse1[2] - treatmentTempStatsOverall$sdTempPulse1[2],
                  ymax = treatmentTempStatsOverall$meanTempPulse1[2] + treatmentTempStatsOverall$sdTempPulse1[2]),
              fill = plotColors[1], alpha = 0.10) + #Pulse 1 treatment ribbon
  geom_segment(aes(x = tempClean$dateTime[301], xend = tempClean$dateTime[395],
                   y = treatmentTempStatsOverall$meanTempPulse2[2], yend = treatmentTempStatsOverall$meanTempPulse2[2]),
               color = plotColors[1], linetype = 3) + #Pulse 2 treatment segment
  geom_ribbon(data = tempClean[301:395],
              aes(x = dateTime,
                  y = treatmentTempStatsOverall$meanTempPulse2[2],
                  ymin = treatmentTempStatsOverall$meanTempPulse2[2] - treatmentTempStatsOverall$sdTempPulse2[2],
                  ymax = treatmentTempStatsOverall$meanTempPulse2[2] + treatmentTempStatsOverall$sdTempPulse2[2]),
              fill = plotColors[1], alpha = 0.10) + #Pulse 2 treatment ribbon
  geom_line(aes(x = dateTime, y = controlTemp), color = plotColors[2]) + #Control treatment temperature
  geom_segment(aes(x = tempClean$dateTime[31], xend = tempClean$dateTime[108],
                   y = treatmentTempStatsOverall$meanTempPulse1[1], yend = treatmentTempStatsOverall$meanTempPulse1[1]),
               color = plotColors[2], linetype = 3) + #Pulse 1 control segment
  geom_ribbon(data = tempClean[31:108],
              aes(x = dateTime,
                  y = treatmentTempStatsOverall$meanTempPulse1[1],
                  ymin = treatmentTempStatsOverall$meanTempPulse1[1] - treatmentTempStatsOverall$sdTempPulse1[1],
                  ymax = treatmentTempStatsOverall$meanTempPulse1[1] + treatmentTempStatsOverall$sdTempPulse1[1]),
              fill = plotColors[2], alpha = 0.15) + #Pulse 1 control ribbon
  geom_segment(aes(x = tempClean$dateTime[301], xend = tempClean$dateTime[395],
                   y = treatmentTempStatsOverall$meanTempPulse2[1], yend = treatmentTempStatsOverall$meanTempPulse2[1]),
               color = plotColors[2], linetype = 3) + #Pulse 2 control segment
  geom_ribbon(data = tempClean[301:395],
              aes(x = dateTime,
                  y = treatmentTempStatsOverall$meanTempPulse2[1],
                  ymin = treatmentTempStatsOverall$meanTempPulse2[1] - treatmentTempStatsOverall$sdTempPulse2[1],
                  ymax = treatmentTempStatsOverall$meanTempPulse2[1] + treatmentTempStatsOverall$sdTempPulse2[1]),
              fill = plotColors[2], alpha = 0.25) + #Pulse 2 control ribbon
  geom_vline(xintercept = as.POSIXct(ymd_hms("2023-07-31 16:00:00")), linetype = 3, color = "black") + #Pre-exposure TTR timepoint
  geom_vline(xintercept = as.POSIXct(ymd_hms("2023-08-01 16:50:00")), linetype = 3, color = "black") + #Pulse 1 TTR timepoint
  geom_vline(xintercept = as.POSIXct(ymd_hms("2023-08-04 16:00:00")), linetype = 3, color = "black") + #Pulse 2 TTR timepoint
  scale_x_datetime(name = "") +
  scale_y_continuous(name = "Temperature (ºC)",
                     limits = c(14.9,33),
                     breaks = c(seq(15,30,5), seq(31,33,1))) +
  ggtitle("A. Overall Experimental Temperature") +
  theme_classic(base_size = 15)
```

```
tempClean[301:394,] %>%
  dplyr::select(dateTime,
                temp10a, temp10b,
                temp11a, temp11b,
                temp12a, temp12b,
                temp13a, temp13b) %>%
  rowwise() %>%
  mutate(., tank10avg = mean(c(temp10a, temp10b), na.rm = TRUE),
         tank11avg = mean(c(temp11a, temp11b), na.rm = TRUE),
         tank12avg = mean(c(temp12a, temp12b), na.rm = TRUE),
         tank13avg = mean(c(temp13a, temp13b), na.rm = TRUE)) %>%
  ggplot(., aes(x = dateTime, y = tank10avg)) +
  geom_line(color = plotColors[2]) +
  geom_line(aes(x = dateTime, y = tank11avg), color = plotColors[1]) +
  geom_line(aes(x = dateTime, y = tank12avg), color = plotColors[2]) +
  geom_line(aes(x = dateTime, y = tank13avg), color = plotColors[1]) +
  scale_x_datetime(name = "") +
  scale_y_continuous(name = "Temperature (ºC)",
                     limits = c(29,32.5)) +
  ggtitle("B. Temperatures for Second Heat Stress") +
  theme_classic(base_size = 15) + theme(axis.text.x = element_text(angle = 45, vjust = 0.55),
                                        plot.margin = margin(b = 0)) #Average treatment temperature #Take temperature data for the second pulse and average data at each time point for each tank.
```

```
mainPlot / pulse2Plot +
  plot_layout(heights = c(5, 1)) #Create multipanel plot. Use plot_layout to make the overal experimental temperature plot larger than the pulse 2 plot
```

![Image](https://github.com/user-attachments/assets/405c2c49-5af9-4c5b-a311-fff7eb7e82e9)

**Figure 4**. Julia's temperature data multipanel

### 2024 temperature analysis

I conducted Kruskal-Wallis tests for MA in [this script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/MA/01-temp-conditions-MA.Rmd) and WA in [this script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/01-temp-conditions-WA.Rmd). I used only data after the ramp down for this analysis. As expected, there was a significant difference between temperature treatments in MA (Chi-squared = 2941.37, p = 0) and WA (Chi-squared = 3191.67, p = 0).

![Image](https://github.com/user-attachments/assets/7692bede-ef15-4ab3-8cf6-112b860a9209)

**Figure 5**. 2024 experimental temperature multipanel

### Going forward

1. Re-analyze Catlin's TTR data
5. Analyze Catlin's HOBO data
1. Add Catlin's methods and results to manuscript
2. Outline introduction
3. Outline discussion
4. Write introduction
5. Write discussion
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
