---
layout: post
comments: true
title: Downloading Metaanalysis Data
tags: metaanalysis
---

## It's not as easy as clicking a button

To download [selected runs](https://github.com/RobertsLab/paper-gigas-metaanalysis/blob/master/data/metaanalysis-data-sources.xlsx) from the SRA database, you have to use `samtools`. I documented my process in [this Jupyter notebook](https://github.com/RobertsLab/paper-gigas-metaanalysis/blob/master/notebooks/2017-04-19-Downloading-SRA-Data.ipynb).

In R, it's easy to [create a script](https://github.com/RobertsLab/paper-gigas-metaanalysis/blob/master/scripts/2017-19-04-Downloading-SRA-Data.R) to download SRA data with `samtools`. I modified my script from [Sean's](http://rpubs.com/seanb80/269484).

```
SRA.list <- c("SRR1693528", "SRR1693531", "SRR1693532", "SRR1693534", "SRR1693537", "SRR1693538", "SRR1696820", "SRR1696826", "SRR1696827", "SRR942533", "SRR1696867", "SRR1697248", "SRR1696868", "SRR1045858", "SRR3742302", "SRR3742311", "SRR3742313", "SRR3742315", "SRR3742316", "SRR3742317", "SRR3742318", "SRR3742319", "SRR3742320", "SRR3742321", "SRR3742322", "SRR3742323", "SRR3742324", "SRR3742325", "SRR3742326", "SRR3742327", "SRR3742344", "SRR3742345", "SRR3742346", "SRR3742347", "SRR3742348") #Make a list of all SRA run codes for data I want to download. I excluded run codes from data already downloaded by Sean and Grace.

setwd("") #Set working directory to a folder where you can download data. My data is currently downloading on emu.

for(i in 1:length(SRA.list)) #Create a for loop so each SRA run specified in "SRA.list" is downloaded as a .fastq file and zipped. 
{
  
  system(paste0("/Users/Shared/Apps/sratoolkit.2.8.2-1-mac64/bin/fastq-dump ", SRA.list[i]))
  system(paste0("tar czf ", SRA.list[i], ".fastq.tar.gz ", SRA.list[i], ".fastq"))
  system(paste0("rm ", SRA.list[i], ".fastq"))
  
}
```
Sean's keeping tabs on my files as they download on emu. Once downloaded, he's moving them to [his scaphapoda directory](http://owl.fish.washington.edu/scaphapoda/Sean/SRA_Downloads/). When they all download, I'll move them to my directory in owl so I know where it is!

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
