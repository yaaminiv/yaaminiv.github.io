---
layout: post
comments: true
title: WGBS Analysis Part 12
tags: manchester gigas-broodstock fastqc mox
---

## Reviewing `trimgalore` output with `multiqc`

Yesterday I [started running [`fastqc`](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) so I could evaluate `trimgalore` output](https://yaaminiv.github.io/WGBS-Analysis-Part11/). After [my script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/03-fastqc.sh) finished running, I transferred the files to relevant subdirectories in [this `gannet` folder](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/trimgalore/), and moved the HTML reports to [my repository](https://github.com/RobertsLab/project-gigas-oa-meth/tree/master/output/02-trimgalore) and [class repository](https://github.com/fish546-2021/yaamini-gigas/tree/main/output/Manchester_01-trimgalore). Then, I looked at the [`multiqc`](https://multiqc.info/) reports after the [first](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/trimgalore/01-abundant-adapters/multiqc_report_3.html), [second](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/trimgalore/02-remaining-adapters/multiqc_report_4.html), and [third](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/trimgalore/03-poly-G/multiqc_report_5.html) trims.

The main thing I wanted to check was the overrepresented sequences remaining in the analysis files, so I started by checking the summary module in each report:

![Screen Shot 2021-02-08 at 2 46 25 PM](https://user-images.githubusercontent.com/22335838/107544587-3d654e00-6b7f-11eb-9983-ab7485dd8b29.png)

![Screen Shot 2021-02-08 at 2 46 37 PM](https://user-images.githubusercontent.com/22335838/107544593-3e967b00-6b7f-11eb-84aa-792da00688dd.png)

![Screen Shot 2021-02-08 at 2 46 51 PM](https://user-images.githubusercontent.com/22335838/107544597-3f2f1180-6b7f-11eb-9a1e-69d5c00d5f91.png)

**Figures 1-3**. MultiQC status checks after the first, second, and third trims.

All samples passed the overrepresented sequences check! When I dug into the reports further, I found that some files still had adapter sequences after the second trim, but they were gone after the third trim:

![Screen Shot 2021-02-10 at 10 49 18 AM](https://user-images.githubusercontent.com/22335838/107557062-fd599780-6b8d-11eb-8280-477f209a1082.png)

![Screen Shot 2021-02-10 at 10 49 35 AM](https://user-images.githubusercontent.com/22335838/107557064-fdf22e00-6b8d-11eb-9cb7-499354449c1d.png)

![Screen Shot 2021-02-10 at 10 49 54 AM](https://user-images.githubusercontent.com/22335838/107557066-fe8ac480-6b8d-11eb-8582-a8d774666332.png)

**Figures 4-6**. MultiQC overrepresented sequences for sample 7 read 1

I looked at the rest of the MultiQC modules from the third trim to see if there were any other inconsistencies between samples:

![Screen Shot 2021-02-08 at 2 51 49 PM](https://user-images.githubusercontent.com/22335838/107544600-3fc7a800-6b7f-11eb-9bb5-1171ceffdace.png)

![Screen Shot 2021-02-08 at 2 52 23 PM](https://user-images.githubusercontent.com/22335838/107544602-3fc7a800-6b7f-11eb-9130-02c8ad5c4e36.png)

**Figures 7-8**. Modules with inconsistencies

The per sequence GC content had 2 files (sample 2 reads 1 and 2) that did not pass the test. There were no spikes that indicated a poly-G tail, and the distributions weren't completely different from the other samples, so I'm not concerned. I also had 10 files (samples 1-4 and 6, reads 1 and 2) that didn't pass sequence duplication levels. Again, the distributions didn't look too different from the other samples. The one thing that does concern me is that all of these samples are from the same treatment: 3N and high pH. The only other sample in that treatment, sample 5, passed the sequence duplication test. When looking at sample methylation levels in a PCA, I'll need to check if all six samples cluster together, or if the sequence duplication levels will affect that clustering.

### Going forward

1. Start [`bismark`](https://github.com/FelixKrueger/Bismark)
2. Update the repository README files
2. Write methods
3. Write results
3. Identify DML
2. Determine if RNA should be extracted
3. Determine if larval DNA/RNA should be extracted

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
