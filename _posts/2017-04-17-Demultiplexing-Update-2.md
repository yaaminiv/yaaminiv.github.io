---
layout: post
comments: true
title: Demultiplexing Update 2
tags: DNR DIA demultiplexing
---

## Another new MSConvert CLI

Over the weekend, I was only able to convert 2 oyster .raw file, which can be found in this [OWL folder](http://owl.fish.washington.edu/spartina/DNR_MSConvert_20170412/). Austin figured out that another program, Prism, slows down after a given number of isolation windows. He rewrote our `msconvert` CLI, and Emma brought it over this morning.

Before we tried the new version, I shared the converted oyster1.mzML file with her so she can test it on `pecanpie`. Her notebook has the information for our test, which can be found [here](https://www.evernote.com/shard/s242/sh/8d90189c-a3d4-47d7-bdae-bc352d82945e/95080e1c08f451a003f05b8beebba3a8).

First, I unzipped the new version of `msconvert` under srlab >> Documents >> oystertest >> oystertest2. I also made a copy of the config file and pasted it into oystertest2 (filename: config_fix.txt). I then ran the following code to convert oyster1.raw using the updated program.

```
cd C:\Users\srlab\Documents\oystertest\oystertest2

msconvert.exe -c config_fix.txt 2017_January_23_envtstress_oyster1.raw
```

![unnamed](https://cloud.githubusercontent.com/assets/22335838/25095909/8665f100-2352-11e7-9b26-6a020ef84c02.png)

Within minutes, the file finished converting! I uploaded to this [OWL link](http://owl.fish.washington.edu/spartina/DNR_MSConvert_20170417/2017_January_23_envtstress_oyster1.mzML).

I moved oyster1.raw to the folder oystertest >> finished-raw. I then started converting the rest of my oyster .raw files.

```
msconvert.exe -c config_fix.txt *.raw
```

![unnamed-2](https://cloud.githubusercontent.com/assets/22335838/25095939/b4e9ab8e-2352-11e7-8623-78004458681e.png)

I'm going to check on the files over the course of the day, and will upload them to this [OWL folder](http://owl.fish.washington.edu/spartina/DNR_MSConvert_20170417/).

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
