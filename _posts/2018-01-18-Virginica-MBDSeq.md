---
layout: post
comments: true
title: Virginica MBDSeq
---

## All systems go on *C. Virginica* sequencing!

Today Sam and I visited Mac at NOAA Montlake to shear DNA in preparation for MBDSeq lab work next week. Here's an overview of the entire process:

**Step 1**: Isolate and quantify DNA. [Sam did this previously](http://onsnetwork.org/kubu4/2017/11/14/dna-isolation-quantification-c-virginica-gonad-gdna/).
- Isolate DNA - Use E.Z.N.A. Kit (Omega)
- Quantify DNA - Use Qubit dsDNA Kit (Invitrogen/ThermoFisher)

**Step 2**: Shear DNA to about 500 bp. Not all DNA with have methylated cytosines. By cutting the DNA into smaller fragments, we can wash away DNA that doesn't have any methylated cytosines and enrich the methylation signal. This is what we did today!
- Shear DNA to target fragment length (~500bp) - Use NOAA sonicator
- Verify shearing - Use Seeb Lab Bioanlyzer with DNA 12000 Kit (Agilent)

**Step 3**: Methylation enrichment. We're doing this next week.
- Enrich for MBD - Use MethylMiner Kit (Invitrogen/ThermoFisher)
- Quantify DNA - Use Qubit hsDNA Kit (Invitrogen/ThermoFisher)

Back to what we did today. 

We loaded eight samples into the QSONICA CD0004054245, which consists of a sample holder and a water bath. The sonicator blasts sound waves at our samples for ten minutes, alternating between 30 second on and off periods at 25% intensity and 4ºC. However, with our first eight samples, the machine spit out an "overload" error.

![img_8792](https://user-images.githubusercontent.com/22335838/35126365-f86ab810-fc61-11e7-8d90-00309a1d2413.JPG)

**Figure 1**. Overload error from sonicator.

Mac looked at the manual, and apparently there are tons of reasons why this could happen. They range from machine parts not being tight enough, to having too much water or electrical issues. We couldn't figure out what was wrong. More importantly, we didn't know how many cycles our samples went through. Pushing our samples through an additional ten cycles would make our DNA too small. We restarted the machine and ran through four more cycles. After the fourth "on" phase, the machine overloaded again. We assume it did something similar when we first turned it on.

Just to test the machine, we loaded eight blanks with 350 µL of water in the sonicator. We ran through five cycles, and the machine did not produce an overload error. Maybe the difference in sample volume (350 µL blank vs. 100 µL of DNA) could have produced the issue?

We then loaded our last two samples, 106 and 108 into the machine. We ran through five cycles before the machine produced the same overload error. We then restarted the machine and ran through five more cycles, stopping the machine after the fifth off cycle.

To verify shearing (and see if our first eight samples were sheared to the proper length), we used the Agilent Technologies 2200 TapeStation. Mac pipetted our samples into a different set of sample wells and mixed them with the necessary reagents. However, I distracted her while she was pipetting, so she accidentally pipetted two samples into the same well. Luckily, these samples were from the first round of unknown sonication! So she wasn't mixing samples we know were sheared properly with those that weren't.

![samples](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-Sample-Information.png)

**Table 1**. TapeStation sample set-up.

![gel](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-Gel-Image.png)

![A1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-A1-Ladder.png)

![B1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-B1-023.png)

![C1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-C1-035.png)

![D1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-D1-036.png)

![E1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-E1-031.png)

![F1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-F1-032.png)

![G1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-G1-103-and-104.png)

![H1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-H1-Blank.png)

![A2](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-A2-105.png)

![B2](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-B2-106.png)

![C2](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Virginica/2018-01-18-DNA-Shearing/2018-01-18-Virginica-Shearing-C2-108.png)

**Figures 2-13**. Shearing results. The peak indicates the average DNA fragment length. A1 was the ladder, which we used to estimate the length of sample DNA. G1 had two samples while H1 was blank because I distracted Mac while she was pipetting.

Looking at our results ([word document](http://owl.fish.washington.edu/spartina/Virginica-MBD/MBDSeq-Labwork/2018-01-18-Virgnica-Shearing-Results.docx) and [.gdna file](http://owl.fish.washington.edu/spartina/Virginica-MBD/MBDSeq-Labwork/2018-01-18-01-Virginica-Shearing-Results.gDNA)), we saw that most of our samples had average lengths around 350 bp. Sam deemed this good enough for us! Next week, we'll do the actual methylation enrichment.

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
