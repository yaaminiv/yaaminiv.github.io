---
layout: post
comments: true
title: WGBS Analysis Part 11
tags: manchester gigas-broodstock fastqc mox
---

## `trimgalore` output and `fastqc`

Last week, I [started `trimgalore`](https://yaaminiv.github.io/WGBS-Analysis-Part10/). My `mox` script finished running, so I wanted to check the output before I started [`bismark`](https://github.com/FelixKrueger/Bismark).

### `trimgalore`

I checked the `mox` directories to start transferring files onto `gannet`. The trimming worked successfully, but there was no [`fastqc`](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) output! This was weird because [the script I used for these samples](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/02-trimgalore.sh) was the same as what I used for [the Hawaii samples](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/02-trimgalore.sh). Confused, I started [this discussion](https://github.com/RobertsLab/resources/discussions/1104) with my scripts and slurm output to determine why I didn't get any `fastqc` output. Sam looked at the slurm output and saw that I had an error associated with my path:

```
>>> Now running FastQC on the validated data zr3616_8_R1_val_1.fq.gz<<<

Can't exec "fastqc": No such file or directory at /gscratch/srlab/programs/TrimGalore-0.6.6/trim_galore line 1525, <IN2> line 5536487816.

>>> Now running FastQC on the validated data zr3616_8_R2_val_2.fq.gz<<<

Can't exec "fastqc": No such file or directory at /gscratch/srlab/programs/TrimGalore-0.6.6/trim_galore line 1535, <IN2> line 5536487816.
Deleting both intermediate output files zr3616_8_R1_trimmed.fq.gz and zr3616_8_R2_trimmed.fq.gz
```

I'm not sure why `fastqc` would disappear from my path after a few weeks. In any case, I used `rsync` to transfer all the output to [this `gannet` folder](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/trimgalore/), organized into various subfolders. Then, I followed Sam's advice to run `fastqc` separately to determine if it was truly a path issue.

### `[`fastqc`](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)`

I created [this script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/03-fastqc.sh) to run `fastqc` on all my trimmed samples. In the script, I specified the `fastqc` and [`multiqc`](https://multiqc.info/) paths, then used the variables throught the script:

```
# Paths to programs
fastqc=/gscratch/srlab/programs/fastqc_v0.11.9/fastqc
multiqc=/gscratch/srlab/programs/anaconda3/bin/multiqc
```

To run `fastqc`, I first specified files to analyze by including the absolute path to the directory. I changed the directory path for each trimming iteration:

```
# Populate array with FastQ files
fastq_array=(/gscratch/scrubbed/yaaminiv/Manchester/analyses/trimgalore/*.fq.gz)

# Pass array contents to new variable
fastqc_list=$(echo "${fastq_array[*]}")
```

When running `fastqc`, I also specified the `outdir` so the output would be written to the same folder as the `trimgalore` output.

```
# Run FastQC
# NOTE: Do NOT quote ${fastqc_list}
${fastqc} \
--threads ${threads} \
--outdir /gscratch/scrubbed/yaaminiv/Manchester/analyses/trimgalore \
${fastqc_list}
```

Finally, I created new `multiqc` reports:

```
#MultiQC
${multiqc} \
/gscratch/scrubbed/yaaminiv/Manchester/analyses/trimgalore/.
```

Unfortunately I didn't include the `-outdir` argument so the reports were written to the same directory as the slurm file. Next time! Once the script finished running, I moved all the `fastqc` and `multiqc` output files to [`gannet`](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/trimgalore/), included the html reports in [this output subdirectory](https://github.com/RobertsLab/project-gigas-oa-meth/tree/master/output/02-trimgalore), and [my class repository](https://github.com/fish546-2021/yaamini-gigas/tree/main/output/Manchester_01-trimgalore). Tomorrow, I'll review the output to make sure the trimming went well.

### Going forward

1. Update the repository README files
1. Check trimming output
2. Start `bismark`
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
