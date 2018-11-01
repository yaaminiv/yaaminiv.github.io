---
layout: post
comments: true
title: DML Analysis Part 16
tags: virginica MBDSeq DML mox 
---

## Mox update

I stopped [`bismark` job I started on Mox](https://yaaminiv.github.io/DML-Analysis-Part12/) because I didn't specify a path for `samtools`.  Steven figured this out manually because by redirecting my standard error, there was nothing going into my default slurm output from Mox. I [edited my script](https://github.com/fish546-2018/yaamini-virginica/blob/master/scripts/2018-10-12-Bismark-Revised-Parameters.sh) to include a path to `samtools` and remoe redirecting any error. I then restarted my Mox job. It's currently deduplicating my files, but I used `cat slurm-401682.out | grep "Mapping efficiency *"` to look at the mapping efficiencies for the alignment.

<img width="812" alt="screen shot 2018-11-01 at 9 54 12 am" src="https://user-images.githubusercontent.com/22335838/47868763-4ffa5580-ddc2-11e8-93da-e677f1558798.png">

**Figure 1**. Mapping efficiency for Mox alignment. The first mapping efficiency is for sample 10, second-tenth for samples 1-9.

Even though my Mox alignment used Bowtie 2-2.3.4, and my genefish alignment used Bowtie 2-2.2.9, I got the same mapping efficiencies! I created a table in my [paper draft](https://docs.google.com/document/d/1gOMJrnhs4D-jCKWlJK2tm0Z27IrSqMkmc7K1pDBmqi0/edit#) that Steven looked at. Turns out he was getting different mapping efficiences because he did not specify that the input data was `--non_directional`. I verfied that I needed that argument in [this issue](https://github.com/RobertsLab/resources/issues/216). The reason why Steven was getting different mapping efficiencies was because he did not include this argument.

However, I encountered another problem with my revised Mox run: I didn't specify `--samtools_path` in my alignment step! I didn't need to do this on genefish because `samtools` was already in my computer path. Because Mox couldn't find `samtools` my output files are now SAMfiles.

<img width="505" alt="screen shot 2018-11-01 at 9 53 19 am" src="https://user-images.githubusercontent.com/22335838/47868762-4f61bf00-ddc2-11e8-94c0-3a2476eb338c.png">

**Figure 2**. Output from `bismark` alignment.

I need to convert my SAMfiles to BAMfiles before I move onto `methylKit`. I also added `--samtools_path` in [a new script](https://github.com/fish546-2018/yaamini-virginica/blob/master/scripts/2018-10-31-Bismark-Revised-Parameters-Samtools.sh) and queued the job on Mox.

I then encountered a THIRD issue with Mox. Just looking at my slurm.out, I couldn't tell if my deduplication was actually running, or if it encountered an error.

<img width="1253" alt="47873735-39a6c680-ddcf-11e8-8003-000c1e8827b3" src="https://user-images.githubusercontent.com/22335838/47875386-a4f29780-ddd3-11e8-99a8-e9a44ad68dea.png">

**Figure 3**. Status of slurm.out file

I posted [this issue](https://github.com/RobertsLab/resources/issues/464). Sam pointed out that my script included a "\" at the end of the last line of code, which essentially left it hanging. I edited [this script]() to remove any hanging backslashes, cancelled both of my queued jobs (Sam later mentioned that I probably should not have cancelled *both* but oh well), and restarted a new Mox job.

From now onwards, I need to quadruple check all of my Mox scripts. Because I'm not running the `bismark` pipeline in chunks like I'm used to in Jupyter, it's hard to catch errors, fix them, and easily restart from where I left off.

### Going forward

1. Figure out how to convert SAMfiles to BAMfiles
2. Use `methylKit` to identify DML and DMR
3. Characterize DML and DMR locations

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
