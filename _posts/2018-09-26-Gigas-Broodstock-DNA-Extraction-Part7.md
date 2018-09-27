---
layout: post
comments: true
title: Gigas Broodstock DNA Extraction Part 7
tags: manchester labwork gigas-broodstock
---

## Extraction plan for actual samples

But first...

### Bioanalyzer results

Kaitlyn ran DNA samples from protocol [test 3](https://yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part4/) and [test 4](https://yaaminiv.github.io/Gigas-Broodstock-DNA-Extraction-Part6/) on the Bioanalyzer. To see the results, I used the 2100 Expert software on the Windows machine. I selected "High Sensitivity DNA" and opened the data files. Each data file has two separate result views: electropherogram and gel. I took screenshots of both formats.

![electro1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/31d8e6020bc6ce3d7426c3f26810423ccdadf49a/images/Manchester/Lab-Notebook/2018-09-26-Bioanalyzer-Results/2018-09-26-Bioanalyzer-Electropherogram1.png)

![gel1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/31d8e6020bc6ce3d7426c3f26810423ccdadf49a/images/Manchester/Lab-Notebook/2018-09-26-Bioanalyzer-Results/2018-09-26-Bioanalyzer-Gel1.png)

![electro2](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/31d8e6020bc6ce3d7426c3f26810423ccdadf49a/images/Manchester/Lab-Notebook/2018-09-26-Bioanalyzer-Results/2018-09-26-Bioanalyzer-Electropherogram2.png)

![gel2](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/31d8e6020bc6ce3d7426c3f26810423ccdadf49a/images/Manchester/Lab-Notebook/2018-09-26-Bioanalyzer-Results/2018-09-26-Bioanalyzer-Gel2.png)

**Figure 1-4**. Electropherogram and gel results for samples run on Bioanalyzer. Sample names are on the left. The top two photos correspond to the first set of samples, and the bottom two with the second set.

Based on her findings, all of my methods lead to DNA shearing to 35 bp. However, my tissue tearor method samples also have electropherogram peaks at 10,380 bp. I wasn't sure if I needed a different lysis method or if I was good to go, so I posted [this issue](https://github.com/RobertsLab/resources/issues/398).

### Extraction protocol

The official protocol can be found [here](https://github.com/RobertsLab/resources/blob/master/protocols/DNA-Extraction-from-Histology-Blocks.md).

I have 10 gonad samples per pH treatment (ambient or low). Because 20 samples is a lot to work with at once, I'll do 2 days of DNA extractions with 10 samples each. I randomly selected which ambient and low pH samples to extract in each batch. The IDs correspond to names on histology photos in [this folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/images/Manchester/Gigas-gonad-histology/2017-04-08-Sampling). Below are diagrams for each histology block:

![gigas1](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Manchester/Gigas-gonad-histology/2017-04-08-Sampling/Gigas_1-04082017.JPG)

![gigas2](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Manchester/Gigas-gonad-histology/2017-04-08-Sampling/Gigas_2_04082017.JPG)

![gigas3](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Manchester/Gigas-gonad-histology/2017-04-08-Sampling/Gigas_3_04082017.JPG)

![gigas4](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Manchester/Gigas-gonad-histology/2017-04-08-Sampling/Gigas_4.JPG)

![gigas5](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/images/Manchester/Gigas-gonad-histology/2017-04-08-Sampling/Gigas_5.JPG)

**Figures 1-5**. Location of each tissue sample on histology blocks.

**Batch 1**: I'll extract DNA from this batch tomorrow.

Low pH:

- 6-T1
- 9-T2
- 2-T1
- 5-T3
- 4-T3

Ambient pH:

- UK-07
- 12-T6
- UK-04
- UK-03
- UK-01

**Batch 2**: I'll extract DNA from this batch on Tuesday.

Low pH:

-	1-T3
-	7-T2
-	8-T2
-	10-T3
-	3-T1

Ambient pH:

-	UK-06
-	11-T4
-	UK-05
-	UK-08
-	UK-02

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
