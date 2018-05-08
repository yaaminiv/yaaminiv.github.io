---
layout: post
comments: true
title: Gonad Methylation Analysis Part 8
tags: virginica bismark MBDSeq
---

## What are those?!  

![shuri](https://78.media.tumblr.com/1ba82b636e2962b2d3fc108ca3a150f4/tumblr_p4oz7qKvPJ1rzfnw1o3_1280.png)

What did I generate using [`bismark` alignment](https://yaaminiv.github.io/Gonad-Methylation-Analysis-Part5/) and [`bismark2report`](https://yaaminiv.github.io/Gonad-Methylation-Analysis-Part6/)? Time to find out.

### `bismark2report`

I thought I'd start with the HTML report since it doesn't require any additional software finnagling. These reports are really cool! They provide basic statistics like the number of analyzed sequences, the kind of alignments, kind of cytosine methylation, and alignment to individual bisulfite strands. I think the most important information at this stage is the alignment. I summarized the alignment information from each file set:

[zr2096_1_s1_R1.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_1_s1_R1_bismark_bt2_SE_report.html) and [zr2096_1_s1_R2.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_1_s1_R2_bismark_bt2_SE_report.html)
- Multiple alignments: 11%
- Unique alignments: 30-32%
- No alignment: 56-58%

[zr2096_2_s1_R1.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_2_s1_R1_bismark_bt2_SE_report.html) and [zr2096_2_s1_R2.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_2_s1_R2_bismark_bt2_SE_report.html)
- Multiple alignments: 19%
- Unique alignments: 54%
- No alignment: 25%

[zr2096_3_s1_R1.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_3_s1_R1_bismark_bt2_SE_report.html) and [zr2096_3_s1_R2.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_3_s1_R2_bismark_bt2_SE_report.html)
- Multiple alignments: 24%
- Unique alignments: 62-64%
- No alignment: 11-12%

[zr2096_4_s1_R1.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_4_s1_R1_bismark_bt2_SE_report.html) and [zr2096_4_s1_R2.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_4_s1_R2_bismark_bt2_SE_report.html)
- Multiple alignments: 22-23%
- Unique alignments: 61%
- No alignment: 15%

[zr2096_5_s1_R1.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_5_s1_R1_bismark_bt2_SE_report.html) and [zr2096_5_s1_R2.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_5_s1_R2_bismark_bt2_SE_report.html)
- Multiple alignments: 22%
- Unique alignments: 61-62%
- No alignment: 15-16%

[zr2096_6_s1_R1.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_6_s1_R1_bismark_bt2_SE_report.html) and [zr2096_6_s1_R2.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_6_s1_R2_bismark_bt2_SE_report.html)
- Multiple alignments: 23%
- Unique alignments: 63-64%
- No alignment: 12-13%

[zr2096_7_s1_R1.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_7_s1_R1_bismark_bt2_SE_report.html) and [zr2096_7_s1_R2.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_7_s1_R2_bismark_bt2_SE_report.html)
- Multiple alignments: 21%
- Unique alignments: 63-64%
- No alignment: 14%

[zr2096_8_s1_R1.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_8_s1_R1_bismark_bt2_SE_report.html) and [zr2096_8_s1_R2.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_8_s1_R2_bismark_bt2_SE_report.html)
- Multiple alignments: 19-20%
- Unique alignments: 56-57%
- No alignment: 23%

[zr2096_9_s1_R1.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_9_s1_R1_bismark_bt2_SE_report.html) and [zr2096_9_s1_R2.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_9_s1_R2_bismark_bt2_SE_report.html)
- Multiple alignments: 22-23%
- Unique alignments: 61-63%
- No alignment: 13-16%

[zr2096_10_s1_R1.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_10_s1_R1_bismark_bt2_SE_report.html) and [zr2096_10_s1_R2.fastq.gz](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-04-29-Bismark2Report-Outputs/zr2096_10_s1_R2_bismark_bt2_SE_report.html)
- Multiple alignments: 22-23%
- Unique alignments: 63-65%
- No alignment: 12-13%

### `bismark2summary`

All of the above information is nicely summarized in the [Bismark Project Summary Report](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/bismark_summary_report.html)!

![untitled2](https://user-images.githubusercontent.com/22335838/39730790-88d4ef40-5218-11e8-9dbb-5a2cc11776ea.png)

**Figure 1**. Collated alignment information for all sequence data. Sample 1 refers to zr2096_1_s1_R1.fastq.gz, sample 2 is zr2096_1_s1_R2.fastq.gz, ..., sample 10 is zr2096_10_s1_R2.fastq.gz

![untitled3](https://user-images.githubusercontent.com/22335838/39730791-89039886-5218-11e8-9ca0-cc374c2eeb3f.png)

**Figure 2**. Percent of calls with CpG methylation for all sequence data.

### `bismark` alignment

The [Bismark User Guide](https://rawgit.com/FelixKrueger/Bismark/master/Docs/Bismark_User_Guide.html#ii-bismark-alignment-step) suggests using a genome viewer to visualize the output SAM files. I'm a bit rusty on my IGV skills, but hopefully [previous exposure in Steven's 2016 Bioinformatics class](http://sr320.github.io/course-fish546-2016/07-Notes/#1) will help!

I downloaded the latest version of the Integrative Genomics Viewer (IGV 2.4) at [this website](http://software.broadinstitute.org/software/igv/download). I then uploaded the *C. virginica* genome I downloaded for `bismark` into the viewer. It wouldn't recognize the .fa file I had originally, so I duplicated the file and changed the extension to .fasta. It allowed me to upload the genome.

![untitled2](https://user-images.githubusercontent.com/22335838/39731461-940b4f18-521c-11e8-9dee-25576ad27b96.png)

**Figure 3**. *C. virginica* genome in IGV.

Now, I needed to add my alignment files. I tried adding in one of the files, but I got the following message:

![untitled3](https://user-images.githubusercontent.com/22335838/39731644-a956194c-521d-11e8-8be8-fc6f3d78a447.png)

**Figure 4**. Path to index file request.

I don't think `bismark` generated any .bai files, so I'm not sure how to proceed. I saved my IGV file [here](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-04-27-Bismark/2018-05-07-Subset-Alignment.xml), then posted [this issue](https://github.com/RobertsLab/resources/issues/248) to get to the bottom of it.

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
