---
layout: post
comments: true
title: West Coast Green Crab Experiment Part 65
tags: green-crab-wc RNA-library-preparation
---

## Optimizing RNA library preparation + continuing with samples

I've got reviewer comments relatively under control, so it's back to determining why my library preparation yields are so low.

- Protocol can be found [here](https://docs.google.com/document/d/1KUGF7xg5rOeEQ883Pr_nJX1BmIbkS9pryi89AFXG9qM/edit?tab=t.0). Any updates that I think are necessary from my notes are then added to the protocol.
- All primer and cDNA concentration information can be found in [this spreadsheet](https://docs.google.com/spreadsheets/d/1B1tyeCI7F_T-l41144m6k_MEVhhU-XCHAEkr6PHoTpw/edit?gid=1215190646#gid=1215190646).

### 2025-06-30

I think bead drying time may be an issue for my samples since the RNA room is so humid! Sara suggested drying for longer than 3 minutes, but obviously monitoring to see when beads are perfectly matte

#### Notes

- Samples: 5-181, 15-033, 25-046, 25-050, 25-054, 25-115, 5-003, 5-131
- Tagging primers: i7-3, i5-1 through i5-8
- I had an issue with the 10 µL multichannel pipet, so I used a single 10 µL pipet to add the 6.5 µL fragmentation master mix.
- Some of the sample 8 beads were disturbed when removing supernatant before the first ethanol wash
- For the first bead dry, I dried all samples for 4-5 minutes. They were more matte this time. I didn't switch the mag beads to the lower configuration, but I slid the tube down to get the beads to pellet well. At the top the beads were pretty spread out.
- After adding 22 µL TE buffer, I accidentally put the beads back on the mag stand! I removed the magnets, shook samples vigorously, spun down, and incubated for 2 minutes
- Forgot to bring the NEBNext Bead Reconstitution Buffer out when the PCR was running, so my samples incubated for about 20 minutes while I tried to pour all of my body heat into the buffer
- I remixed samples 1 and 8 after adding the reconstitution buffer about a minute into the incubation time since I saw they weren't well mixed
- Did not spin the beads after the two ethanol washes since I wanted to avoid disturbing the pellet. Air dried beads for 6-7 minutes since I didn't quick spin to remove excess ethanol
- Qubit S1 = 52.98, S2 = 25194.38

#### Results

My yields were still not looking good! Most yields were between 1-3 ng/µL. Did I somehow over-dry my beads this time?!

## 2025-07-01

I am working through yet another set of samples, but I've bumped the PCR to 15 cycles to see what will happen.

### Notes

- Samples: 5-074, 25-055, 5-133, 5-125, 5-075, 25-195, 5-010, 5-015
- Tagging primers: i7-4, i5-1 through i5-8
- Spilled the Tris buffer when I was making a strip tube aliquot! I have 6-8 mL left, which should be enough for 120 samples. I think we'll be good?
- For the first bead clean, I ran out of SPRI beads (even though I MEASURED 1000 µL of BEADS WHICH SHOULD HAVE BEEN MORE THAN ENOUGH). I had to use the Omega dupes of the Ampure beads for sample 8, but they unfortunately were not brought to room temperature for at least 30 minutes before using. Hopefully that won't completely destroy my yields.
- When getting 20 µL of fragmented RNA, I used a 1000 µL multichannel pipet with 20 µL tips. Solution was clear after 3.5 minutes
- I noticed a lot of white precipitate in the End Prep Reaction Buffer. This has been there every time I've done library prep alone, but I haven't given it much thought. Today I shook the buffer until the precipitate was reconstituted.
- Changed the number of PCR cycles to 15. Noticed that it was originally set to 13 *repeats* not 13 total cycles! So previously I've been doing 14 PCR cycles.
- When I went to do PCR, my MM was frozen! I placed the samples and thawed oligos in the -20ºC for a few minutes while I poured my body heat into the MM. I vortexed the MM before using.
- Took the Omega beads out of the 4ºC when I took out the Bead Reconstitution Buffer. They sat for ~60 minutes while Da-In used the RNA room to pool samples and my PCR was running
- Lost some of sample 5 while mixing the 70 µL of beads, and while mixing the bead reconstitution buffer. There was something weird going on with the pipetter.
- Some of sample 5 also got on the lid of sample 4 when I was adding the Omega beads. I wiped it off and went in again with a kim wipe with RNAse Away.
- After the first 5 minute bead incubation, there was a "halo" of Omega beads at the bottom. I let them sit for 2 more minutes, but they weren't moving, so I moved forward.
- Today was just a clumsy day....I spilled about 1 µL of reconstitution buffer. I also noticed while adding the buffer to the samples that the 200 µl pipetter had some ampure bead drops on the actual body of the pipet?! I need to figure out if I can make an aliquot of beads and avoid getting stuff on the pipetter.
- Disturbed S6 while removing the second ethanol wash and disturbed S3 while mixing the TE buffer
- Did the quickest spin of my life after the second ethanol wash and before the final bead dry.
- Took the 1.3 µL out for Qubit before taking out the 20 µL libraries
- Qubit S1 = 56.83, S2 = 26952.89

### Results

I FINALLY HAD GOOD YIELDS! I honestly think  it's a combination of being more attentive when drying the beads and doing the shorted quick spin of my life after the ethanol washes and before the drying. The really short spin keeps the pellet in tact. I could then slide the tubes on the magnets so the pellet moved up before I removed the leftover ethanol and dried the beads.

When I was working with Sara during the first bead clean today, she said she was surprised that my pellets were really spread apart after the spin. She also told me to stop the dry when the spread out beads started to turn light. I think *this* was the bead drying step I was messing up! I now know to:

1. Do a very, very, very quick spin to ensure the pellet stays in tact
2. Move the tubes down the magnet to get the pellet higher on the magnet without switching the orientation
3. Move the tubes up in the rack so I can see how much ethanol is left at the bottom of the tubes and pull it out while beads are drying (before 2 minute mark ideally)
4. Stop drying if any of the spread out beads on top of the main pellet start to turn light and crack

## 2025-07-02

### Notes

- Samples: 25-175, 25-052, 15-161, 15-092, 5-009, 5-122, 25-202, 5-004
- Tagging primers: i7-5, i5-1 through i5-8
- Tubes popped out of the ice when it was cooling after fragmentation, but I stuffed it back inside
- Added slightly more than 5 µL of eluted fragmented mRNA due to an issue with the 10 µL multichannel pipetter
- During end preparation, I went to add the reaction buffer and enzyme mix. I added 2.5 µl of the reaction buffer, then decided to put that tube on the left side of the buffer and then grab the enzyme mix. When I got the enzyme mix, I looked down and realized I had the buffer instead! I wasn't sure if I added 2.5 µL of buffer or 2.5 µL of enzyme mix. So, I added my 1.5 µL of enzyme mix, and then added another 1.5 µL of buffer just in case. The proportions were off until the bead clean.
- When adding the ligation master mix, I used 300 µL pipet tips with a 100 µL pipetter, instead of 20 µL pipet tips with a 100 µL pipetter
- I ejected some of the ethanol on the sides of the tube after the first ethanol wash. I pulled it back up and cleaned the tubes and stands.
- Added 20 µL of TE with the 100 µL pipetter, but then I realized I needed 3 more µL of TE! I added that in with the 10 µL multichannel
- I got some beads in the Qubit sample, so I removed it from the tube and added another 1.3 µL of sample
- Went to incubate Qubit samples but then realized I forgot to vortex! So samples were sitting for ~1 min before vortexing and then ~2 min afterwards.
- Qubit S1 = 63.65, S2 = 25136.67

### Results

Well, they're not as great as yesterday, but they are still serviceable. When talking to Sara, it seems like my buffer-enzyme ratio in end preparation were the likely culprit. The kit is really sensitive to the correct ratios of reagents! I'll avoid mixing things up tomorrow oops.

## 2025-07-03

### Notes

- Samples: 25-175, 25-052, 15-161, 15-092, 5-009, 5-122, 25-202, 5-004
- Tagging primers: i7-6, i5-1 through i5-8
- When getting the 5 µL fragmented mRNA, I had issues with the 10 µL multichannel not pulling up liquid. I had to go in multiple times to get all of the liquid, so I may have had more than 5 µL of mRNA when starting cDNA synthesis.
- Opened new tube of NEBNext UltraExpress Second Strand Master Mix
- Created an aliquot of Omega beads in a 1.5 µL eppendorf tube so I don't have to deal with sticking a pipetter into a large bottle
- Again, issues with the 200 µL multichannel pipet. Sample 5's liquid just wouldn't eject, so I lost some of the beads when mixing them in
- Forgot to spin samples after adding 72 µL of beads, and only realized this after the 2 minute magnetic rack incubation. So, I spun them down and reincubated on the magnetic rack for 2 minutes
- First bead dry: samples 6-8 2 minutes, rest of the samples 3 minutes. Samples 1-4 had a lot of EtOH at the bottom even though I tried pulling it out with a 10 µL multichannel
- More multichannel issues with the 100 µL pipet. I transferred 20 µL of the cDNA into new tubes, but had to manually go back in for samples 1, 4, 5, and 7 to top up the volume.
- Sample 6 was frozen after adding the End Prep Reaction Buffer and Enzyme Mix, so I thawed in my hand and mixed 6 additional times
- After adding the Bead Reconstitution Buffer to sample 4, I was unable to get some of it out after mixing.
- I have more than enough Bead Reconstitution Buffer left to finish the protocol (would need 360 µL for 90 samples, and they gave me over 3 mL)
- Second bead dry was 2 minutes, but I closed samples 6-8 a bit before that
- Qubit S1 = 53.33, S2 = 26483.58

### Results

Once again I had good yields! It was clearly the enzyme mix and buffer issue last time that messed with my output.

### Going forward

1. Prep all RNA libraries: i7-7, i7-8, i7-9, i7-10, i7-11
2. Re-prep specific libraries: i7-1
3. Increase yields on specific libraries
3. Send RNA for sequencing!
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
