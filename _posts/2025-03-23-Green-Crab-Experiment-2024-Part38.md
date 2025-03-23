---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 38
tags: green-crab-cold
---

## Water temperatures for WA and Julia's experiments

I need to create figures depicting the water temperature conditions for each experiment! I [previously analyzed temperature conditions for the MA experiment](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part31/), so I proceeded to do the same thing for the WA experiment and Julia's data.

### WA experiment

I conducted all analyses in [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/01-temp-conditions-WA.Rmd). I previously started the analysis, but I stopped because there were issues with certain HOBO loggers dying and needing to be replaced!

The first thing I needed to do was calculate the average temperature conditions within tanks and treatments. I initially created a `for` loop to calculate conditions after the ramp down had occurred:

```
for (i in 1:(length(hobos)/2)) {
  treatmentTempStats[i,2] <-
    mean(rbind(hobos[[i]][,2][115:length(hobos[[i]][,2])],
               hobos[[i+8]][,2][115:length(hobos[[i+8]][,2])]), na.rm = TRUE)
  treatmentTempStats[i,3] <-
    sd(rbind(hobos[[i]][,2][115:length(hobos[[i]][,2])],
             hobos[[i+8]][,2][115:length(hobos[[i+8]][,2])]), na.rm = TRUE)
} #For each tank, combine the treatment temperature data from both HOBO loggers, either find the average or SD, and save in treatmentTempStats
```

I made these calculations starting at the 115th row (after 7 a.m. on 2024-07-23). However, some of the HOBO loggers were put in after that point since the original logger batteries died! I individually calculated average conditions for the tanks where loggers were replaced:

```
treatmentTempStats$meanTemp[1] <- mean(rbind(hobos[[1]][,2][115:length(hobos[[1]][,2])],
                                             hobos[[1+8]][,2][1:length(hobos[[1+8]][,2])]), na.rm = TRUE) #Tank 1 revised mean
treatmentTempStats$sdTemp[1] <- sd(rbind(hobos[[1]][,2][115:length(hobos[[1]][,2])],
                                         hobos[[1+8]][,2][1:length(hobos[[1+8]][,2])]), na.rm = TRUE) #Tank 1 revised SD

treatmentTempStats$meanTemp[2] <- mean(rbind(hobos[[2]][,2][1:length(hobos[[2]][,2])],
                                             hobos[[2+8]][,2][115:length(hobos[[2+8]][,2])]), na.rm = TRUE) #Tank 2 revised mean
treatmentTempStats$sdTemp[2] <- sd(rbind(hobos[[2]][,2][1:length(hobos[[2]][,2])],
                                         hobos[[2+8]][,2][115:length(hobos[[2+8]][,2])]), na.rm = TRUE) #Tank 2 revised SD

treatmentTempStats$meanTemp[5] <- mean(rbind(hobos[[5]][,2][1:length(hobos[[5]][,2])],
                                             hobos[[5+8]][,2][1:length(hobos[[5+8]][,2])]), na.rm = TRUE) #Tank 5 revised mean
treatmentTempStats$sdTemp[5] <- sd(rbind(hobos[[5]][,2][1:length(hobos[[5]][,2])],
                                         hobos[[5+8]][,2][1:length(hobos[[5+8]][,2])]), na.rm = TRUE) #Tank 5 revised SD

treatmentTempStats$meanTemp[8] <- mean(rbind(hobos[[8]][,2][115:length(hobos[[8]][,2])],
                                             hobos[[8+8]][,2][1:length(hobos[[8+8]][,2])]), na.rm = TRUE) #Tank 8 revised mean
treatmentTempStats$sdTemp[8] <- sd(rbind(hobos[[8]][,2][115:length(hobos[[8]][,2])],
                                         hobos[[8+8]][,2][1:length(hobos[[8+8]][,2])]), na.rm = TRUE) #Tank 8 revised SD
```

The calculations were saved [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/01-temp-conditions/temp-mean-sd.csv).

I then calculated treatment-level conditions. The colder conditions were 2.137014 ± 0.3486562 and the cold conditions were 5.199698 ± 0.2157631. Due to having two extra tanks in the room, the conditions were not as cold as the MA experiment (~1.66ºC). However, the SE vaguely overlap, so there's that. I'll probably need to include some sort of section in the discussion about this, but I don't think a ~0.5ºC difference would lead to failure to right in one population but not another.

Before I could make the figure I had to merge my datasets together by approximate time. [The `roll` argument in the `data.table` package was meant for this](https://rdatatable.gitlab.io/data.table/articles/datatable-joins.html#rolling-join)! The package website wasn't very helpful, so I based most of my code on [this Stack Overflow question](https://stackoverflow.com/questions/39282749/how-to-join-two-dataframes-by-nearest-time-date) and [this blog](https://www.r-bloggers.com/2016/06/understanding-data-table-rolling-joins/). I also confirmed that I could roll join multiple tables based on [this Stack Overflow question](https://stackoverflow.com/questions/13273833/merging-multiple-data-tables). After A LOT of trial and error with the code, I figured out that tables should be merged from smallest to largest. I went about the merges in three steps:

```
#Format all temperature data files as data.tables

tank1a <- as.data.table(tank1a)
tank1b <- as.data.table(tank1b)

tank2a <- as.data.table(tank2a)
tank2b <- as.data.table(tank2b)

tank3a <- as.data.table(tank3a)
tank3b <- as.data.table(tank3b)

tank4a <- as.data.table(tank4a)
tank4b <- as.data.table(tank4b)

tank5a <- as.data.table(tank5a)
tank5b <- as.data.table(tank5b)

tank6a <- as.data.table(tank6a)
tank6b <- as.data.table(tank6b)

tank7a <- as.data.table(tank7a)
tank7b <- as.data.table(tank7b)

tank8a <- as.data.table(tank8a)
tank8b <- as.data.table(tank8b)
```

```
temp <- tank3a[tank2b, on = "dateTime", roll = T, rollends = F][tank3b, on = "dateTime", roll = T, rollends = F][tank4a, on = "dateTime", roll = T, rollends = F][tank4b, on = "dateTime", roll = T, rollends = F][tank6a, on = "dateTime", roll = T, rollends = F][tank6b, on = "dateTime", roll = T, rollends = F][tank7a, on = "dateTime", roll = T, rollends = F][tank7b, on = "dateTime", roll = T, rollends = F][tank8a, on = "dateTime", roll = T, rollends = F][tank1a, on = "dateTime", roll = T, rollends = F] #Use data.table rolling join to match HOBO logger data together by nearest dateTime. Use only the loggers wiht at least 411 observations
```

```
temp2 <- tank5b[tank1b, on = "dateTime", roll = T, rollends = F][tank2a, on = "dateTime", roll = T, rollends = F][tank5a, on = "dateTime", roll = T, rollends = F][tank8b, on = "dateTime", roll = T, rollends = F][temp, on = "dateTime", roll = T, rollends = F] #Roll HOBO loggers with limited data. First file named is the smallest one, final file rolled is temp (largest file with the most data)
```

Each join step, I confirmed that the data for each logger started and stopped at the correct time. I could then select the relevant columns needed to make the figure!

```
temp2 %>%
  dplyr::select(dateTime,
                temp1a, temp1b,
                temp2a, temp2b,
                temp3a, temp3b,
                temp4a, temp4b,
                temp5a, temp5b,
                temp6a, temp6b,
                temp7a, temp7b,
                temp8a, temp8b) %>%
  rowwise() %>%
  mutate(., colderTemp = mean(c(temp1a, temp1b,
                                temp2a, temp2b,
                                temp3a, temp3b,
                                temp4a, temp4b), na.rm = TRUE),
         coldTemp = mean(c(temp5a, temp5b,
                           temp6a, temp6b,
                           temp7a, temp7b,
                           temp8a, temp8b), na.rm = TRUE)) %>%
  ggplot(., aes(x = dateTime, y = colderTemp)) +
  geom_line(color = plotColors[1]) +
  geom_ribbon(aes(x = dateTime, y = coldMean, ymin = coldMean - coldSD, ymax = coldMean + coldSD), fill = plotColors[2], alpha = 0.15) +
  geom_hline(yintercept = coldMean, colour = plotColors[2], linetype = 3) +
  geom_line(aes(x = dateTime, y = coldTemp), color = plotColors[2]) +
  geom_ribbon(aes(x = dateTime, y = colderMean, ymin = colderMean - colderSD, ymax = colderMean + colderSD), fill = plotColors[1], alpha = 0.15) +
  geom_hline(yintercept = colderMean, colour = plotColors[1], linetype = 3) +
  scale_x_datetime(name = "") +
  scale_y_continuous(name = "Temperature (ºC)",
                     breaks = c(seq(0,5,1), seq(5,15,5))) +
  theme_classic(base_size = 15)
#Combine data from all dataframes and select columns of interest. Use rowwise operations to calculate average temperature for each treatment at each timepoint. Plot colder temepratures for each timepoint. Add a ribbon with the overall average colder temperature and SD. Add a line for the overall average colder temperature. Repeat with the cold temperature. Modify x-axis label. Modify y-axis.
```

![Image](https://github.com/user-attachments/assets/341fb1b2-32d1-45ee-8a05-2694b2eeb256)

**Figure 1**. WA water temperature conditions

### Re-analysis of WA data

I [revised my WA genotypes](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part37/), so I need to update the data analysis in [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/WA/03-TTR-analysis-WA.Rmd). I saved the [revised average TTR](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/ttr-model-comparison-stat-output.csv) and [threshold analysis](https://github.com/yaaminiv/cold-green-crab/blob/main/output/WA/03-TTR-analysis/ttr-binomial-model-comparison-output.csv) stats afterwards.

### Julia's experiment

Time to tackle Julia's data! I first exported and cleaned the raw data from the HOBO loggers, which can be found [here](https://github.com/yaaminiv/cold-green-crab/tree/main/data/repeat-heat/HOBO). This was slightly complicated due to a temperature logger malfunction in tank 11, but I could cross-reference the anomalies with conditions in tank 13.

Then I created [this R Markdown script](https://github.com/yaaminiv/cold-green-crab/blob/main/code/repeat-heat/01-temp-conditions-julia.Rmd) to analyze the data. This data analysis was going to be a bit different. I needed to calculate average conditions by tank and treatment for the different parts of the experiment: pre-exposure, pulse 1, rest, and pulse 2. Before analysis, I rolled the data together:

```
#Format all temperature data files as data.tables

tank10a <- as.data.table(tank10a)
tank10b <- as.data.table(tank10b)

tank11a <- as.data.table(tank11a)
tank11b <- as.data.table(tank11b)

tank12a <- as.data.table(tank12a)
tank12b <- as.data.table(tank12b)

tank13a <- as.data.table(tank13a)
tank13b <- as.data.table(tank13b)
```

```
temp <- tank13a[tank10b, on = "dateTime", roll = T, rollends = F][tank10a, on = "dateTime", roll = T, rollends = F][tank13b, on = "dateTime", roll = T, rollends = F][tank11b, on = "dateTime", roll = T, rollends = F][tank12a, on = "dateTime", roll = T, rollends = F][tank12b, on = "dateTime", roll = T, rollends = F][tank11a, on = "dateTime", roll = T, rollends = F] #Roll data by closest dateTime. List begins with the shortest file and ends with the longest file
```

```
tempClean <- temp %>%
  dplyr::select(dateTime,
                temp10a, temp10b,
                temp11a, temp11b,
                temp12a, temp12b,
                temp13a, temp13b) #Select necessary columns from rolled data
```

I then used a series of `for` loops to calculate conditions for each portion of the experiment:

```
treatmentTempStats <- data.frame("tank" = c(10:13),
                                 "meanTempPre" = rep(0, times = 4),
                                 "sdTempPre" = rep(0, times = 4),
                                 "meanTempPulse1" = rep(0, times = 4),
                                 "sdTempPulse1" = rep(0, times = 4),
                                 "meanTempRest" = rep(0, times = 4),
                                 "sdTempRest" = rep(0, times = 4),
                                 "meanTempPulse2" = rep(0, times = 4),
                                 "sdTempPulse2" = rep(0, times = 4)) #Create an empty dataframe
```

```
#Pre-exposure: 1:22

for (i in seq(2, 8, 2)) {
  treatmentTempStats[(i/2),2] <-
    mean(rbind(tempClean[[i]][1:22],
               tempClean[[i+1]][1:22]), na.rm = TRUE)
  treatmentTempStats[(i/2),3] <-
    sd(rbind(tempClean[[i]][1:22],
             tempClean[[i+1]][1:22]), na.rm = TRUE)
} #For each tank, combine the treatment temperature data from both HOBO loggers, either find the average or SD, and save in treatmentTempStats

#Pulse 1: 31:108

for (i in seq(2, 8, 2)) {
  treatmentTempStats[(i/2),4] <-
    mean(rbind(tempClean[[i]][31:108],
               tempClean[[i+1]][31:108]), na.rm = TRUE)
  treatmentTempStats[(i/2),5] <-
    sd(rbind(tempClean[[i]][31:108],
             tempClean[[i+1]][31:108]), na.rm = TRUE)
} #For each tank, combine the treatment temperature data from both HOBO loggers, either find the average or SD, and save in treatmentTempStats

#Rest: 112:291

for (i in seq(2, 8, 2)) {
  treatmentTempStats[(i/2),6] <-
    mean(rbind(tempClean[[i]][112:291],
               tempClean[[i+1]][112:291]), na.rm = TRUE)
  treatmentTempStats[(i/2),7] <-
    sd(rbind(tempClean[[i]][112:291],
             tempClean[[i+1]][112:291]), na.rm = TRUE)
} #For each tank, combine the treatment temperature data from both HOBO loggers, either find the average or SD, and save in treatmentTempStats

#Pulse 2: 301:406

for (i in seq(2, 8, 2)) {
  treatmentTempStats[(i/2),8] <-
    mean(rbind(tempClean[[i]][301:406],
               tempClean[[i+1]][301:406]), na.rm = TRUE)
  treatmentTempStats[(i/2),9] <-
    sd(rbind(tempClean[[i]][301:406],
             tempClean[[i+1]][301:406]), na.rm = TRUE)
} #For each tank, combine the treatment temperature data from both HOBO loggers, either find the average or SD, and save in treatmentTempStats
```

I saved the conditions [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/repeat-heat/01-temp-conditions/temp-mean-sd.csv). It's weird to see that the second pulse for tank 13 was lower than every other tank. I am not entirely sure why that happened. I then used a similar approach to calculate overall treatment conditions:

```
treatmentTempStatsOverall <- data.frame("treatment" = c("control", "treatment"),
                                        "meanTempPre" = rep(0, times = 2),
                                        "sdTempPre" = rep(0, times = 2),
                                        "meanTempPulse1" = rep(0, times = 2),
                                        "sdTempPulse1" = rep(0, times = 2),
                                        "meanTempRest" = rep(0, times = 2),
                                        "sdTempRest" = rep(0, times = 2),
                                        "meanTempPulse2" = rep(0, times = 2),
                                        "sdTempPulse2" = rep(0, times = 2)) #Create an empty dataframe
```

```
#Pre-exposure: 1:22

for (i in seq(2, 4, 2)) {
  treatmentTempStatsOverall[(i/2),2] <-
    mean(rbind(tempClean[[i]][1:22],
               tempClean[[i+1]][1:22],
               tempClean[[i+4]][1:22],
               tempClean[[i+5]][1:22]), na.rm = TRUE)
  treatmentTempStatsOverall[(i/2),3] <-
    sd(rbind(tempClean[[i]][1:22],
             tempClean[[i+1]][1:22],
             tempClean[[i+4]][1:22],
             tempClean[[i+5]][1:22]), na.rm = TRUE)
} #For each tank, combine the treatment temperature data from both HOBO loggers, either find the average or SD, and save in treatmentTempStatsOverall

#Pulse 1: 31:108

for (i in seq(2, 4, 2)) {
  treatmentTempStatsOverall[(i/2),4] <-
    mean(rbind(tempClean[[i]][31:108],
               tempClean[[i+1]][31:108],
               tempClean[[i+4]][31:108],
               tempClean[[i+5]][31:108]), na.rm = TRUE)
  treatmentTempStatsOverall[(i/2),5] <-
    sd(rbind(tempClean[[i]][31:108],
             tempClean[[i+1]][31:108],
             tempClean[[i+4]][31:108],
             tempClean[[i+5]][31:108]), na.rm = TRUE)
} #For each tank, combine the treatment temperature data from both HOBO loggers, either find the average or SD, and save in treatmentTempStatsOverall

#Rest: 112:291

for (i in seq(2, 4, 2)) {
  treatmentTempStatsOverall[(i/2),6] <-
    mean(rbind(tempClean[[i]][112:291],
               tempClean[[i+1]][112:291],
               tempClean[[i+4]][112:291],
               tempClean[[i+5]][112:291]), na.rm = TRUE)
  treatmentTempStatsOverall[(i/2),7] <-
    sd(rbind(tempClean[[i]][112:291],
             tempClean[[i+1]][112:291],
             tempClean[[i+4]][112:291],
             tempClean[[i+5]][112:291]), na.rm = TRUE)
} #For each tank, combine the treatment temperature data from both HOBO loggers, either find the average or SD, and save in treatmentTempStatsOverall

#Pulse 2: 301:406

for (i in seq(2, 4, 2)) {
  treatmentTempStatsOverall[(i/2),8] <-
    mean(rbind(tempClean[[i]][301:406],
               tempClean[[i+1]][301:406],
               tempClean[[i+4]][301:406],
               tempClean[[i+5]][301:406]), na.rm = TRUE)
  treatmentTempStatsOverall[(i/2),9] <-
    sd(rbind(tempClean[[i]][301:406],
             tempClean[[i+1]][301:406],
             tempClean[[i+4]][301:406],
             tempClean[[i+5]][301:406]), na.rm = TRUE)
} #For each tank, combine the treatment temperature data from both HOBO loggers, either find the average or SD, and save in treatmentTempStatsOverall
```

I saved the output [here](https://github.com/yaaminiv/cold-green-crab/blob/main/output/repeat-heat/01-temp-conditions/overall-temp-mean-sd.csv). Tank 13 brought down the average pulse 2 conditions for the treatment tanks. Again, the ranges overlap so maybe it's ok? I started to calculate differences using an ANOVA for each portion of the experiment, but I'm not going to include those results since I think I need to use an approach that does better with uneven sample sizes.

Finally I made the experimental condition figure! I wanted to include averages and SD ribbons for the two pulses. To do this, I employed `geom_segment` for the line, then defined the particular data window for `geom_ribbon` to ensure that they were concentrated at the pulses and not across the whole figure:

```
tempClean[4:406,] %>%
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
              fill = plotColors[1], alpha = 0.15) + #Pulse 1 treatment ribbon
  geom_segment(aes(x = tempClean$dateTime[301], xend = tempClean$dateTime[406],
                   y = treatmentTempStatsOverall$meanTempPulse2[2], yend = treatmentTempStatsOverall$meanTempPulse2[2]),
               color = plotColors[1], linetype = 3) + #Pulse 2 treatment segment
  geom_ribbon(data = tempClean[301:406],
              aes(x = dateTime,
                  y = treatmentTempStatsOverall$meanTempPulse2[2],
                  ymin = treatmentTempStatsOverall$meanTempPulse2[2] - treatmentTempStatsOverall$sdTempPulse2[2],
                  ymax = treatmentTempStatsOverall$meanTempPulse2[2] + treatmentTempStatsOverall$sdTempPulse2[2]),
              fill = plotColors[1], alpha = 0.15) + #Pulse 2 treatment ribbon
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
  geom_segment(aes(x = tempClean$dateTime[301], xend = tempClean$dateTime[406],
                   y = treatmentTempStatsOverall$meanTempPulse2[1], yend = treatmentTempStatsOverall$meanTempPulse2[1]),
               color = plotColors[2], linetype = 3) + #Pulse 2 control segment
  geom_ribbon(data = tempClean[301:406],
              aes(x = dateTime,
                  y = treatmentTempStatsOverall$meanTempPulse2[1],
                  ymin = treatmentTempStatsOverall$meanTempPulse2[1] - treatmentTempStatsOverall$sdTempPulse2[1],
                  ymax = treatmentTempStatsOverall$meanTempPulse2[1] + treatmentTempStatsOverall$sdTempPulse2[1]),
              fill = plotColors[2], alpha = 0.15) + #Pulse 2 treatment ribbon
  scale_x_datetime(name = "") +
  scale_y_continuous(name = "Temperature (ºC)",
                     limits = c(14.9,33),
                     breaks = c(seq(15,30,5), seq(31,33,1))) +
  theme_classic(base_size = 15)
#Combine data from all dataframes and select columns of interest. Use rowwise operations to calculate average temperature for each treatment at each timepoint. Plot colder temepratures for each timepoint. Add a ribbon with the overall average colder temperature and SD. Add a line for the overall average colder temperature. Repeat with the cold temperature. Modify x-axis label. Modify y-axis.
```

![Image](https://github.com/user-attachments/assets/8f4a353a-73c9-4546-9397-99a53f5d8feb)

**Figure 2**. Temperature conditions for Julia's experiment

### Going forward

1. Create an experimental setup figure
2. Outline introduction
3. Outline discussion
1. Genotype Catlin's samples
2. Re-analyze Catlin's TTR data
5. Analyze Catlin's HOBO data
1. Add Catlin's methods and results to manuscript
3. Determine methods for comparing population responses
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
