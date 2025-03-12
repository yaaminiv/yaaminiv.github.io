---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 59
tags: green-crab-wc
---

## Samples for metabolomics and lipidomics

I need to identify which samples from this experiment for metabolomics and lipidomics! Here are some of the parameters:

- Need 6 samples/condition for metabolomics
  - During the experiment, I dissected 6 crabs/temperature. I won't have enough biological replicates to examine temperature and genotype differences at each timepoint. I've decided to look at temperature differences over time, but use the final timepoint to understand genotype impacts.
- Would like samples from days 0, 2, 7, 21, and 42
  - Days 2 and 21 would be similar to the 2022 experiment
  - Day 0 is important for a pre-experiment control. I wouldn't use samples from 15ºC for the rest of the timepoints since we didn't see any differences in the control over 22 days for the 2022 experiment
  - Day 42 was the end of the experiment for the 15ºc and 25ºC treatments. Things really shouldn't have changed too much between days 42 and 49 for the cold experiment. So I can use some of the samples from day 49 to ensure there are enough biological replicates to assess genotype differences at the end of the experiment

### Recording sampling dates

To pick samples, I needed to know when they were actually sampled. Turns out this was much more annoying than I thought it would be. My [crab metadata](https://docs.google.com/spreadsheets/d/1fXgWXxYD_uDuTNVs8u6DKRU_p-VWLaake0NAL0C9jU4/edit?gid=0#gid=0) sheet had a column for death date, but I didn't really update it throughout the experiment! Instead, I needed to go through my [TTR spreadsheet](https://docs.google.com/spreadsheets/d/1V5BdWi9ZFqBuqIpJwnvNfXU4YDyiDVZ4Rjw1FMOxx2A/edit?gid=0#gid=0) to determine which crabs were sampled on which dates. I would add sampling date in my [crab genotype spreadsheet](https://docs.google.com/spreadsheets/d/1B1tyeCI7F_T-l41144m6k_MEVhhU-XCHAEkr6PHoTpw/edit?gid=0#gid=0) so I can look at sampling date and genotype at the same time. Turns out this took WAY longer than I thought. Some notes about the process:

- Sampling done 6/28 (day 0) and 6/29 (day 1), but there was no marking on the TTR sheet for what crabs were sampled. Sampling for all other days was recorded in the notes column or with a "D" next to the crab ID
  - Once I added the information for all the other crabs sampled, I noticed that the first two crabs recorded in each tank on 6/28 and 6/29 were the ones that were sampled. I confirmed this by checking my physical notebook entry where I wrote a list of crab IDs to make sampling tubes for the 6/28 sampling.
- After I went through the TTR spreadsheet, I made sure each crab had a sampling date. If there wasn't a sampling date, I tried to figure out why based off of my notes. I started some of this process in August 2023 at the end of my experiment, but never finished. To motivate myself to do this work, I didn't publish the lab notebook posts. Some motivation that was because a whole 1.5 years later I'm done. Whoops.
			-Figured out discrepancy with 25-048 and 25-047! Information in [this lab notebook post](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part16/). TL;DR 25-048 and 25-047 were mixed up. Based on weight, 25-048 listed on 7/26/23 was actually 25-047! I made this correction in the Google Sheet.
			-15-199: No sampling date because Julia used this crab for her experiment!
			-15-041 and 15-184: Missing after 7/24 and not sampled 8/9. No death records in any lab notebook post. Crab could have been used for Julia's experiment? But not present in [Julia's data](https://github.com/yaaminiv/wc-green-crab/blob/main/data/time-to-right-genotype-julia.csv) because it could have been part of the tank that died. I will need to confirm with her.
			-25-118 in 25-107 sample tubes from 8/9/23. See [this lab notebook post](https://yaaminiv.github.io/Green-Crab-Experiment-2023-Part16/)
- Similarly, I sorted through crabs that were “double sampled” according to my records
			-25-107 and 25-108 discrepancy solved (see above)
			-25-114 sampled 6/29 and 7/24: Looked at TTR spreadsheet for 6/29. 114 listed in second spot (sampled) and at the end! Based on weight, "114" sampled on 6/29 is actually 25-194. This makes sense because there was no mention of 25-194 for the rest of the experiment! I fixed this in the TTR spreadsheet for 6/29 and added the correct sampling dates to the genotype spreadsheet
- I had a whole mess with T7 (5ºC tank)
  - 5-129: F, YG, 17.76 g
    - Weighed 25.09 g on 7/24? Correct weight when sampled on 8/16
    - Went through all other crabs alive on 7/24 and there were none that were unaccounted for
    - Possible that the wrong weight was recorded, so I replaced it with an N/A
  - 5-196: F, Y/YG, 13.78g
    - Sampled 7/24 at 16.39 g, but 8/9 at 14.67 g
    - The correct weight was used on 8/9, so it was not sampled on 7/24
  - The crab sampled 7/24 was 16.39 g, F, Y, and missing R5
    - 5-130: Was missing 8/16, but seen 7/3 and 7/5. On both of these days, weights were around 17 g. Also documented in metadata as missing R5.
    - 5-130 was sampled on 7/24
  - 5-133: F, YG, 12.13 g
    - Sampled on 6/28 with a weight of 17.59g. Not seen again
    - Likely a case of an incorrect weight recorded. Changed to N/A

After all of this painstaking record comparison, I had sampling dates for all my crabs!

### TTR discrepancies

While I was in the data cleaning mode, I noticed that I had several TTR rows highlighted in red due to discrepancies. Primarily these are crab IDs that were recorded but do not exist. Figured I might as well fix them now by referencing the crab metadata, sampling date information (which I now have compiled) with any identifying demographic information recorded in the TTR spreadsheet. All necessary changes were made to the TTR spreadsheet. I will need to redownload and clean it for analysis.

- 6/28/23
  - 15-004: Based on weight and label, could be 15-034 or 15-044. Not 15-034, and 15-044 was already recorded for TTR that day. There was no other identifying information. I removed this row since I could not determine which crab it corresponded to.
  - 15-182: M, Y, 29.29 g
    - Actually 15-132 (M, Y, 29.39 g)
  - 25-164: M, Y, 52.18 g, missing R1
    - Actually 15-167 (M, Y, 51.47, missing R1)
- 6/29/23
  - 15-054: M, Y, 52.18
    - 15-037 (M, Y, 48.20). Only crab with similar weight. If you squint an illegible 54 *could* look like a 37.
  - 15-130: F, Y, 2.97
    - Crab exists, but weight is very incorrect. Changed to N/A
  - 15-150: F, G, 19.60 g
    - Could be 15-158 (F, YG, 19.94 g) or 15-159 (F, G, 19.64)
    - Most likely 15-159 based on weight. It's also easier to replace 0 with a 9 as a typo when you're typing very quickly
  - 15-132: M, Y, 28.76 g
    - Could be 15-152 (M, Y, 29.39) or 15-154 (M, Y, 27.47)
    - 15-152 was already measured on 6/29! So this crab is 15-154.
- 7/3/23
  - 25-155: M, Y, 22.03 g
    - Actually 25-115 (M, Y, 21.54 g)
  - 25-166: F, Y, 20.25
    - Actually 25-116 (F, YG, 19.22 g)
- 7/12/23
  - 25-043: M, YG, 23.90 g, underside carapace injury
    - Could be 25-047 (M, G, 22.94) or 25-060 (M, YG, 21.89) based on weight
    - 25-060 is a respirometry crab, so that's not an option
    - 25-047 was YG and 23.98 g on 7/3 and YG and 24.25 g on 7/26. On 6/29 a healed ventral injury was noted. So this is likely the crab that was recorded!

### Samples for sequencing

Time to pick my samples! Some thoughts when I was picking:

- For the day 0/control timepoint, I will have two crabs/tank = 2*9 = 18 crabs. Within that 18, I want to have 6 of each genotype. In picking, I want to prioritize samples from the earlier days, even though we know that the control does not change over time.
- If I had a choice between picking a respirometry crab vs. a normal crab for a timepoint/genotype, I picked the respirometry crab.
- I identified 6 crabs/condition (temperature and genotype) from the 6/28/23 day 0 timepoint and 15ºC control crabs. 14/18 crabs are from 6/28/23. The remaining 4 were selected from the 15ºC tanks, prioritizing earlier days if possible to achieve the genotype balance.
- For all intermediate timepoints, I was only interested in 5ºC and 25ºC crabs.
- At the end, I wanted 6 of each genotype from 5ºC, and 6 of each genotype from 25ºC. In an idea world, I'd pick 2 crabs from each of the 6 tanks. I went through each tank picking one of each genotype (where possible), then filled in the genotype balance with crabs as needed from each temperature treatment.
  - 5ºC: Prioritized crabs from 8/16, filled in with 8/9 as needed
    - 6 CC, 6 CT, 5 TT
  - 25ºC: Prioritized respirometry crabs, filled in as needed with others
    - 5 CC, 6 CT, 5 TT

I'm pretty impressed with the balance that I have! Overall, I will need to sequence 89 crabs. As I mentioned in my notes, I picked the respirometry crabs or constant-TTR crabs were possible. I suspect that some samples I've identified will be too small, and may not have been split between metabolomics and RNA-Seq tubes. In these cases, I can pick a different crab from the same conditions (if I have options) to maximize the number of crabs that will be processed for both workflows.

Now to reorganize the freezer and find my samples!

### Going forward

1. Send samples for metabolomics
2. Decide on a plan for RNA
3. Extract RNA
4. Examine HOBO data from 2023 experiment
5. Demographic data analysis for 2023 paper
6. Start methods and results of 2023 paper

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
