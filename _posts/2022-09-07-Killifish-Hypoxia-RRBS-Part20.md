---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 20
tags: killifish RRBS poseidon BAT
---

## Re-align to the new genome

It's been a while so it's time to dive back into this project! Before revisiting my analyses, I want to start mapping the data to the [newest version of the genome](https://www.ncbi.nlm.nih.gov/genome/?term=txid8078[orgn]). Similar to the old genome, the newer version has 24 chromosomes. Based on [the annotation of genes closest to DMR](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part19/), there are some genes close to DMR that are not in the most current version of the genome. Additionally, it's hard to find the RefSeq annotation or RepeatMasker information for the old genome on NCBI. The old version of the genome was put together in 2015, while the new version is from 2020.

There are lots of reasons to use the genome! I'm going to go through the entire BAT workflow with the new genome, then compare it to the output from the old genome. Lack of large-scale methylation differences using both genome versions would lend further support to our results.

I downloaded new genome onto Poseidon from [this link](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/011/125/445/GCF_011125445.2_MU-UCD_Fhet_4.1/). I modified the [envfile](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-alignment-envfile.txt) to include the proper path to the new genome, and removed the code that bound Neel's home directory to the `singularity` container since the genome was in my home directory and not his [in the alignment script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-alignment.sh). The [`BAT-mapping`](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-BAT-mapping.sh) and [`BAT-mapping-stat`](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-BAT-mapping-stat.sh) scripts remain unchanged. Since I had a [problem running scripts over 24 hours last time](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part6/), I didn't bother to change the time limit on the script.

The genome indices are being built now, so as long as some samples start mapping in the next 24 hours I should be set to get mapping results in the next week!

### Going forward

1. Revise methylation landscape information
1. Update methods and results
3. Match DMR with RNA-Seq information
4. Continue BAT workflow with new genome
2. Try DMR identification with `bismark` and `methylKit`
6. Create OSF repository for all intermediate files

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
