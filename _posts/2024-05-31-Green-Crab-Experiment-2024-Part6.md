---
layout: post
comments: true
title: Cold Acclimation Green Crab Experiment Part 6
tags: green-crab-cold restriction-digest
---

## Picking a restriction enzyme

Since [my PCR primers look promising](https://yaaminiv.github.io/Green-Crab-Experiment-2024-Part4/), Carolyn said I could look into restriction enzymes! This involved a lot of finagling with [Primer3Plus](http://primer3plus.com) and [NEBcutter 3.0](https://nc3.neb.com/NEBcutter/prj/) but I figured it out.

First, I uploaded the SMC sequence to Primer3Plus to remind myself where the primers I picked sit on the whole sequence:

![Screenshot 2024-05-31 at 2 11 19 PM](https://github.com/yaaminiv/yaaminiv.github.io/assets/22335838/d6a1443c-3d78-480e-b999-d6d579724741)

**Figure 1**. New primers with the SMC sequence.

The primers yield a 690 bp product from 3474 bp to 4163 bp:

>new_primer_product TGATGCTCAGCACAGGAAGGCTGTGGCTGACATGATCCATGAACTGAGTAAAGACGCACAGTTCATCACCACTACATTCAGGCCTGAGCTACTGGAACATGCAAACAAATTTTATGGTGTAAAGTTTAGAAATAAGGTATCTCATGTGGAGTGTGTATCACGTGAAGAGGCTTATGATTTCGTTGAAGATGACCAAACTCATGGTTAAATATTTCTTACATACAATACAGTCAGGGTGAATCTTCAAAGAAATGTGAAGGCTTTTCCTTTTTGTAAATTTCCTCTTTTTTTTAATATTCATTGGATTTTGGTAGATAAAGATAAAGTTTGATATTCTCTTTATACTTATCTATATCTGATGAATATGAATGCTTCCAGTGTAAATTGTAAATATTTTTTAAAATTTGCATATCACATTTGTTTTTTCTAGAATAACTGTTATTTGTACAAAGAAATTAAAACCAGAGAAATATGTAGCTTATTTTTTGAATGAGCATAAATTGAGAGGGACCTGAGGTAACATAGATTTTATATTCATAGGAGAATAAAGCACTTATATACAGTATTGTATTATTCGTTTATGTATAGGAAAACTATTTATATTTCTGGAATTGAGTTATTTTTTTACTCAAACATAATAGGCATTACAATTTCATCATATTATGTTCTCATGAAGTTAAAGTATGGAAGA

I then pasted this new sequence into NEB, noting that the C/T SNP at 3528 bp in the SMC sequence is at 54 bp in the primer product, and the C/T SNP at 3562 bp in the SMC sequence is at 88 bp in the primer product. I then looked at all commercially available restriction enzymes between 50 and 110 bp in the primer product to see if any of them cut at either SNP site:

![Screenshot 2024-05-31 at 2 27 49 PM](https://github.com/yaaminiv/yaaminiv.github.io/assets/22335838/77c75c33-7b25-4ae0-9d8e-917dd00aa425)

**Figure 2**. Potential restriction enzymes for section of the primer product

There was one candidate restriction enzyme that cut at a SNP site! [Alul]() cut at 88 (the later C/T SNP), but it also cut at 477 (3951 in the SMC sequence). If I used Alul, this would mean I would get various bands:
  - TT homozygote: 213 bp, 477 bp
  - CC homozygote: 88 bp, 389 bp, 301 bp band
  - CT heterozygote: 88 bp, 213 bp, 301 bp, 389 bp, and 477 bp

The TT homozygote will be easy to distinguish and with the CC homozygote, I'll just see a large band complex at ~350ish bp. The CT heterozygote may be tricky. The 88 bp will probably be lost with all the primer-dimer bits. It's possible I could distinguish the 213 from the 301 and 389 bands if I run it long enough.

In any case, this is the only potential enzyme so I guess I'm going to order it! I know that the ToxLabs have a bunch of restriction enzymes, so I may try and ask Sibel if I can run tests with known TT, CC, and CT crabs before buying the restriction enzyme. It's also possible that designing a different reverse primer that eliminates the second cut site will help!

To the lab, I guess.

### Going forward

1. Purchase experimental materials
2. Set up cold room
3. Obtain MA crabs!
3. Identify candidate restriction enzymes for new primers
2. Tailor PCR protocol for new primers
3. Develop restriction band digest for genotyping
4. Develop heart rate protocol

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
