---
layout: post
comments: true
title: Hawaii Gigas Methylation Analysis Part 4
tags: hawaii gigas-ploidy mox trimgalore
---

## Reviewing second round of trimming

### `trimgalore`

Based on [Sam's suggestion at Science Hour](https://github.com/RobertsLab/resources/issues/1065#issuecomment-761256413), I [trimmed my data a second and third time](https://yaaminiv.github.io/Hawaii-Gigas-Methylation-Analysis-Part3/) with [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/01-trimgalore-2.sh). Before starting `bismark` alignments, I wanted to look at the results of this trimming!

I transfered the output off `mox`:

```
rsync --archive --progress --verbose /gscratch/scrubbed/yaaminiv/Hawes/analyses/trimgalore-2/* yaamini@172.25.149.226:/Volumes/web/spartina/project-oyster-oa/Haws/trimmed-data-2/ #Transfer output to gannet (mounted on computer)
```

I also saved the output for the second round of trimming (removing remaining adapter sequences) [here](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_01-trimgalore/trimmed-data-2) and the third round of trimming (removing poly-G tails] [here](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/Haws_01-trimgalore/trimmed-data-2/poly-G). I started by comparing the `multiqc` reports from the [first](https://htmlpreview.github.io/?https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/Haws_01-trimgalore/multiqc_report_1.html), [second](https://htmlpreview.github.io/?https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_01-trimgalore/trimmed-data-2/multiqc_report.html), and [third](https://htmlpreview.github.io/?https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_01-trimgalore/trimmed-data-2/poly-G/multiqc_report_1.html) rounds of trimming.

![Screen Shot 2021-01-19 at 10 44 36 AM](https://user-images.githubusercontent.com/22335838/105078867-6fcdd080-5a43-11eb-9332-607a8585063d.png)

![Screen Shot 2021-01-19 at 10 44 46 AM](https://user-images.githubusercontent.com/22335838/105078870-71979400-5a43-11eb-933e-dbd7c30eb011.png)

![Screen Shot 2021-01-19 at 10 45 03 AM](https://user-images.githubusercontent.com/22335838/105078872-72c8c100-5a43-11eb-9a9f-bc8179b5ae7c.png)

**Figures 1-3**. Status checks for each round of trimming

Based on the status checks, there are no overrepresented sequences in the resultant files from the third round of trimming! I double checked this by looking at the reports from sample 24. There were no overrepresented sequences in the first paired read after the second round of trimming, but there were overrepresented sequences in the second pair. This makes sense because I trimmed out the remaining adapter sequences, but not the poly-G tail. After the third round of trimming, neither paired read had any overrepresented sequences!

I also checked the per sequence GC content in each `multiqc` report:

![Screen Shot 2021-01-19 at 10 50 14 AM](https://user-images.githubusercontent.com/22335838/105079370-2b8f0000-5a44-11eb-98e2-4f3b0f6cf82c.png)

![Screen Shot 2021-01-19 at 10 50 23 AM](https://user-images.githubusercontent.com/22335838/105079376-2cc02d00-5a44-11eb-82c9-57daedfebc25.png)

![Screen Shot 2021-01-19 at 10 50 31 AM](https://user-images.githubusercontent.com/22335838/105079379-2d58c380-5a44-11eb-9b56-201475b5c20e.png)

**Figures 4-6**. Per sequence GC content for each round of trimming

There are peaks at the end of the distribution after the first and second round of trimming that can be atttributed to the poly-G tails. After the third round of trimming, that second peak is gone! While trimming out the poly-G tail works, it does make me wonder what other artifacts may be present in my sequences. That can't be answered until Zymo takes a look at the sequences. Another thing [Hollie pointed out](https://github.com/RobertsLab/resources/issues/1065#issuecomment-763029528) is that while trimming is relatively easy, it does mean that we're "wasting" reads on the poly-G tail instead of data. I decided to look at the length of the sequences between the second and third rounds of trimming. Even without the poly-G tails, I would have had to do two rounds of trimming with `trimgalore` and `cutadapt` anyways. The third round of trimming is the "extra" round that would lead to potential data loss.

To do this, I downloaded the plot data from sequence counts in the `multiqc` report. I know I could technically get these numbers from the general stats files `multiqc` produces, but that would require a little more finagling and I liked the format of these tables better. I saved the sequence cound information for the second trim [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_01-trimgalore/trimmed-data-2/fastqc_sequence_counts_plot-trim2.csv) and third trim [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_01-trimgalore/trimmed-data-2/poly-G/fastqc_sequence_counts_plot-trim3.csv). For each file, I calculated the total number of reads, percent unique reads, and percent duplicate reads in Excel. Between the two trims, I expected the total number of reads and percent duplicate reads to decrease after trim 3, but the percent unique reads to increase. I created a [new file with this information](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_01-trimgalore/trimmed-data-2/poly-G/fastqc_sequence_counts_plot-trim2v3.csv), and calculated the difference in total reads between trims and the percent of reads "lost" after trim 3. Looking at the file, I lose 1-3% of reads after trimming the poly-G tail. This is about 511,333 to 1,197,753 reads for each individual file.

### Obtaining adapter sequences

Since Hollie is also talking to Zymo about her data, she asked me to [send her the adapter sequences in my data](https://github.com/RobertsLab/resources/issues/1065#issuecomment-763029528). To do this, I used `grep` to extract the trimmed sequences from each trimming report:

```
grep Sequence *trimming_report.txt > adapter-sequences.txt #Obtain adapter sequences for first trim
cd trimmed-data-2 #Change directory
grep Sequence *trimming_report.txt | cat >> ../adapter-sequences.txt #Obtain adapter sequences for second trim and append to existing adapter sequence file
```

The adapter sequences are saved in [this file](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/Haws_01-trimgalore/adapter-sequences.txt).

### `bismark`

While waiting for information from Zymo, I decided to start `bismark`. I need to run the alignments on the files after the third round of trimming. I edited [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/02-bismark.sh) to reflect the changes in file names.

```
rsync --archive --progress --verbose yaamini@172.25.149.226:/Users/yaamini/Documents/project-oyster-oa/code/Haws/02-bismark.sh . #Transfer script to user directory
sbatch 02-bismark.sh #Run script
```

### Going forward

1. Get more information from Zymo about poly-G tails in samples
2. Review `bismark` output
3. Transfer scripts used to a [nextflow workflow](https://github.com/nextflow-io/nextflow)
4. Identify methylation analysis framework beyond `methylKit`

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
