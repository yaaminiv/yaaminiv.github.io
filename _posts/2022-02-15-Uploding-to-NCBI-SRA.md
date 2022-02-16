---
layout: post
comments: true
title: Uploading Data to NCBI SRA
tags: how-to
---

## Documenting for posterity (because it's an annoying process)

I'm uploading the *C. gigas* Manchester WGBS data to the NCBI SRA, and I wanted to document this process so I remember how to do it next time!

### Obtaining accession numbers

First things first, I needed to get BioProject and BioSample accession numbers. I went to the [NCBI Submisison Portal](https://submit.ncbi.nlm.nih.gov) and registered a BioProject. Once it was registered, I used the accession number in my BioSample registration.

### Uploading data

Then came the saga of trying to upload data to the NCBI SRA. I first tried using the Aspera plug-in to upload my 16 files. I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1400) to ask how Sam does the same thing. He suggested using FTP, but newer Mac OS versions don't have `ftp`. Enter `homebrew`. I first installed `ftp`:

```
brew install inetutils
```

I followed the instructions from the NCBI SRA to upload the files using `ftp`. Here's where I ran into an issue: `ftp` takes FOREVER. Sam suggested I `ssh` into a Roberts Lab computer, mount `owl`, run `screen` so I can kill the Terminal session, then run `ftp` from there. He suggested I use the following code:

```
sudo mount -t cifs //owl.fish.washington.edu/web owl/ -o username=yaaminiv,vers=1.0
```

This code would mount `owl` remotely, since I need to navigate to the `nightingales` directory and upload my files from there. I used `ssh` to log into `ostrich`, but wasn't able to use `sudo` on this machine. Sam mentioned he provided instructions thinking I was using Linux, so I logged into `roadrunner` instead and it worked! After I navigated to the `owl` directory with my raw data, started `screen`, and I used `ftp -i`, then `mput` to upload all the files. To check on the `screen`, I used `screen -ls` to find the session number, then `screen -r` to attach the session.

Thankfully my connection didn't drop during the `ftp`, and I was able to load the files and submit to the NCBI SRA!

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
