---
layout: post
comments: true
title: DML Analysis Part 6
tags: virginica MBDSeq DML gene-enrichment
---

## Blasting for Uniprot codes (again)

Turns out I didn't do my [`blastx`](https://yaaminiv.github.io/DML-Analysis-Part2/) correctly.

![untitled](https://user-images.githubusercontent.com/22335838/43119340-e4f51ee6-8eca-11e8-907a-b31e74bc5f4c.png)

Mistake 1: Not properly saving my `blastx` output...

I don't know how I didn't realize I never added in a line of code to actually SAVE my `blastx` output. How. What. Why.

Mistake 2: Blasting the entire genome against the Uniprot database, and not just the genes!

Again, how.

The solution is to re-blast! To help my own brain think clearly, I created a new [folder for gene enrichment analyses](https://github.com/RobertsLab/project-virginica-oa/tree/master/analyses/2018-06-14-Gene-Enrichment-Analysis). I also moved the database files and [R Markdown file](https://github.com/RobertsLab/project-virginica-oa/blob/master/analyses/2018-06-14-Gene-Enrichment-Analysis/2018-06-14-Gene-Enrichment-Analysis.Rmd) I created previously into this folder. Thankfully, I could use the same Uniprot database. I then downloaded the [*C. virginica* transcrpt file](ftp://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/002/022/765/GCF_002022765.2_C_virginica-3.0/GCF_002022765.2_C_virginica-3.0_rna.fna.gz).

Once all of my input files were assembled, it was time to `blastx`. I looked through the `blastx` help menu, but I couldn't find the arguments I needed awash in the "oh shit I just did everything incorrectly" feeling. I posted [this Github issue](https://github.com/RobertsLab/resources/issues/365#issuecomment-419248938), and Sam helped identify the arguments I needed. My code looked like this:

1. Path to blastx
2. -query provides the file we want to blast 3.-db specifies database created in the previous step
3. -outfmt specifies the type of output file. I will use 6, a tabular file
4. -out <filename> will allow me to save the output as a new file
5. -num_threads 4 uses 4 CPUs in the BLAST search
6. -max_target_seqs 1 keeps only one aligned sequence (i.e. the best match)

`````
/Users/Shared/Apps/ncbi-blast-2.2.29+/bin/blastx \
-query 2018-09-06-Virginica-transcripts.fna \
-db uniprot-filtered-reviewed.fasta \
-outfmt 6 \
-out 2018-09-06-Transcript-Uniprot-blastx.txt \
-num_threads 4 \
-max_target_seqs 1
`````

Hopefully that will finish running sometime this weekend! Once I have the `blastx` results, I can merge them with the overlap files and do some gene enrichment. I also need to send the transcript-Uniprot results to Mike Riffle in Genome Sciences so he can fix the background in the [gene enrichment tool he built](https://meta.yeastrc.org/compgo_yaamini_oyster/pages/goAnalysisForm.jsp). Until that gets fixed, I can use DAVID to get some preliminary gene enrichment results for PCSGA. I can also review the methods in Emma's geoduck paper so I understand the statistical analyses that accompany gene enrichment.

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
