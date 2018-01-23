---
layout: post
comments: true
title: Comments and Tags
tags: maintenance how-to
---

## I pimped out my notebook!

I finally sat down and enabled Disqus commenting for my lab notebook posts and figured out how to tag my lab notebook entries. A quick how-to on both:

**Comments**:
- Register on [Disqus]()
- At the top of each entry, add the text "comments: true"
- Copy and paste the following code at the end of the lab notebook entry between an `{% if page.comments %}` and `{% endif %}`

`````
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
`````
 
- Use the following code to count comments 

`````
<script id="dsq-count-scr" src="//the-responsible-grad-student.disqus.com/count.js" async></script>
`````

**[Tags](https://yaaminiv.github.io/tagview/)**:
- Add "tags: " at the top of each lab notebook entry
- List some tags after the colon! Helps if they are lowercase. Use a space between words to differentiate between tags (ex. "DNR labwork" sets two tags: "DNR" and "labwork"). Use a hyphen for multiword tags (ex. "mass-spec," not "mass spec").

Yet another thing I can cross off of my to-do list.

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
