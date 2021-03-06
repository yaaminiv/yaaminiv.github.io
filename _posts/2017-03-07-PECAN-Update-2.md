---
layout: post
comments: true
title: PECAN Update 2
tags: DNR pecan DIA
---

## To reconvert or not reconvert?

Spoiler alert: The answer is to reconvert.

In my [previous post](https://yaaminiv.github.io/Pecan-Run-1-Update-1/), I set out a game plan to troubleshoot my `pecanpie` run. First, Laura and I played around with the [.blib file](http://owl.fish.washington.edu/spartina/DNR_PECAN_Run_1_20170303_Output/pecan2blib/DNR_PECAN_Run_1_20170303_SpLibrary.blib) produced as part of my [PECAN Run 1 Output](http://owl.fish.washington.edu/spartina/DNR_PECAN_Run_1_20170303_Output/).

We followed the instructions in [this powerpoint](https://github.com/RobertsLab/project-pacific.oyster-larvae/blob/master/Skyline-example-files-ETS.sky/slides01.pdf) after downloading [Skyline daily](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&ved=0ahUKEwjj8cfl98XSAhVJi1QKHTeTAx4QFggcMAA&url=https%3A%2F%2Fskyline.ms%2Fproject%2Fhome%2Fsoftware%2FSkyline%2Fdaily%2Fregister-form%2Fbegin.view%3F&usg=AFQjCNGCxWGw2YgD5wKFZ24OBuV37He6ZA&sig2=DDOmDgqRwiN8yK56wC4poA) on the Windows machine. We successfully uploaded the background proteome to Skyline, but when we uploaded the .blib file, we got an error message:

![skyline-error](https://cloud.githubusercontent.com/assets/22335838/23686193/eebd0ee8-035c-11e7-8c6b-c4612579f46a.png)

This supports Sean's finding that nothing was ever generated by PECAN because it couldn't find what it considered to be valid MS2 data in my mzML files and stopped there. This did not happen to Laura's files, which is weird because we underwent the same MSConvert process. I will reconvert one raw file to an mzML file and rerun PECAN. Hopefully that will yield a usable .blib file!

The lab notebook I'm working in can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/DNR/2017-03-07-Reconvert-mzML-Files.ipynb). A couple of highlights:

- I had to install ProteoWizard Tools and Git Bash on the Windows computer
- Created a config file for MSConvert
- Used the following code to run MSConvert (which then ensued in the computer crashing the first time, but worked pretty well the second time)

```
cd C:\Users\srlab\Documents\2017-03-07-MSConvert

"c:\Program Files\ProteoWizard\ProteoWizard 3.0.10577\msconvert.exe" -c C:\Users\srlab\Documents\2017-03-07-MSConvert/msconvert-SIMMS1.config C:\Users\srlab\Documents\2017-03-07-MSConvert/2017_January_23_envtstress_oyster1.raw
```

![unnamed](https://cloud.githubusercontent.com/assets/22335838/23691802/af99baf4-037f-11e7-99dd-e5ab8c9c3228.png)

- Used the following code for my second PECAN run

```
pecanpie -o /home/srlab/Documents/DNR_PECAN_Run_2_20170307_Output \
-b /home/srlab/Documents/DNR_PECAN_Run_2_20170307/Combined-gigas-QC.txt \
-n DNR_PECAN_Run_1_20170307_SpLibrary \ 
-s gigas \
--isolationSchemeType BOARDER \
--pecanMemRequest 35 \
/home/srlab/Documents/DNR_PECAN_Run_2_20170307/2017-03-07-mzML-file-path-list.txt \
/home/srlab/Documents/DNR_PECAN_Run_2_20170307/2017-03-07-background-peptides-path-list.txt \
/home/srlab/Documents/DNR_PECAN_Run_2_20170307/2017-03-07-isolation-windows.csv \
--fido \
--jointPercolator
```

Then I used this code to run the search with `pecanpie`:

```
cd [directory specified by the -o argument for pecanpie] \
./run_search.sh
```

And used `qstat -f` to check that the job was running.

![pecanrun2](https://cloud.githubusercontent.com/assets/22335838/23693008/e91dc804-0386-11e7-8479-8664408e882c.png)

Tomorrow, I'll see if the log file has the same error as my first run (and maybe even get a viable .blib file to explore Skyline with while I set up the rest of my analyses........).

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
