---
layout: post
comments: true
title: DML Analysis Part 12
tags: virginica MBDSeq DML bismark mox
---

## Running `bismark` on Mox

Today I started my `bismark` run on Mox! Here's how I did it:

### Create a script

Since I'm running `bismark` with [revised parameters](https://yaaminiv.github.io/DML-Analysis-Part10/), I'm putting these "clean" analyses in my class repository. In my class repository, I created a [shell script to run `bismark`](https://github.com/fish546-2018/yaamini-virginica/blob/master/scripts/2018-10-12-Bismark-Revised-Parameters.sh) based on [Sam's script](http://owl.fish.washington.edu/Athaliana/20180912_oly_WGBSseq_bismark/20180912_oly_WGBSseq_bismark.sh). Because I'm working in a script, I was able to specify variable paths for `bismark`, `samtools`, my genome folder, and my trimmed files.

### Set up Mox

I navigated to the `/gscratch/scrubbed/` directory and created a subdirectory called `data`. Within this subdirectory, I wanted to house my bisulfite genome and trimmed files. Steven and Sam said I could transfer these files to Mox in [this issue](https://github.com/RobertsLab/resources/issues/431#issuecomment-430770659). To do this, I initiated `rsync` transfers from Mox so I wouldn't have to deal with two-factor authentication.

`````
rsync --archive --progress --verbose yaamini@172.25.149.226:/Users/yaamini/Documents/project-virginica-oa/analyses/2018-04-27-Bismark/2018-04-27-Bismark-Inputs . #Transfer my bisulfite genome to Mox
`````
`````
rsync --archive --progress --verbose yaamini@172.25.149.226:/Volumes/web/Athaliana/20180411_trimgalore_10bp_Cvirginica_MBD/zr2096_*R1*.fq.gz . #Transfer analysis files
`````

When I used `ls -F` to look at my directory contents, I realized I only transferred half of the trimmed files. I used the code below to transfer the rest.

`````
rsync --archive --progress --verbose yaamini@172.25.149.226:/Volumes/web/Athaliana/20180411_trimgalore_10bp_Cvirginica_MBD/zr2096_*_val_2.fq.gz . #Transfered rest of files
`````
`````
rsync --archive --progress --verbose yaamini@172.25.149.226:/Volumes/web/Athaliana/20180411_trimgalore_10bp_Cvirginica_MBD/checksums.md5 . #Transfer checksums
`````

`````
md5sum -c checksums.md5 #Check checksums. All passed!
````

Based on [Steven's feedback](https://github.com/RobertsLab/resources/issues/431#issuecomment-430770659), I transferred my shell script to my user directory.

`````
rsync --archive --progress --verbose yaamini@172.25.149.226://Users/yaamini/Documents/yaamini-virginica/scripts/2018-10-12-Bismark-Revised-Parameters.sh . #Transfer script to USER directory, not scrubbed directory
`````

### Start Mox run

Now that I had everything on Mox, I was ready to start running the job!

`````
sbatch -p srlab -A srlab 2018-10-12-Bismark-Revised-Parameters.sh #Submitted batch job 381386
`````

Then [that failed](https://github.com/RobertsLab/resources/issues/438#issuecomment-430776330)...because I didn't properly specify my SBATCH path. My output directory is within my data directory, so I specified that in my script on genefish, then moved the fixed script to Mox.

`````
sbatch -p srlab -A srlab 2018-10-12-Bismark-Revised-Parameters.sh #Submitted batch job 381403
`````

Now I'm running! The more I think about it, the more I don't like analysis output in my `data` directory. Once my job ends, I'll move the output directory to an `analyses` subdirectory.

Fingers crossed this all works well.

{% if page.comments %}

<div id=“disqus_thread”></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page’s canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page’s unique identifier variable
};
*/
(function() { // DON’T EDIT BELOW THIS LINE
var d = document, s = d.createElement(‘script’);
s.src = ‘https://the-responsible-grad-student.disqus.com/embed.js';
s.setAttribute(‘data-timestamp’, +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href=“https://disqus.com/?ref_noscript”>comments powered by Disqus.</a></noscript>

{% endif %}

<script id=“dsq-count-scr” src=“//the-responsible-grad-student.disqus.com/count.js” async></script>
