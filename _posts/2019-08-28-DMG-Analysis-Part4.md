---
layout: post
comments: true
title: DMG Analysis Part 4
tags: virginica MBDSeq DMG
---

## Characterizing GOslim terms

I was working through my discussion outline, but was unsure what to make of the lists of parent GOterms I matched to DMG and genes with DML. Steven suggested I look at GOslim terms instead.

I used [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-08-28-blastx-to-GOslim.ipynb) to get GOslim terms for [transcripts](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2018-09-11-Transcript-Uniprot-blastx-codeIsolated.txt). I created separate lists for [biological processes](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/Blastquery-GOslim-BP.sorted) and [molecular functions](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/Blastquery-GOslim-MF.sorted). I returned to my [DML gene enrichment](2019-07-30-Gene-Enrichment-with-GO-MWU.Rmd) and [DMG gene enrichment](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-Differentially-Methylated-Gene-Analysis.Rmd) to figure out which GOslim terms go with DML and DMG. To do that, I merged GOslim terms with GO-MWU output, then counted the frequency of each GOslim term present. Here are the resulting tables:

**Biological processes**:

- [Hypermethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHyper-CVGOSlim-Frequency-BP.csv)
- [Hypomethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHypo-CVGOSlim-Frequency-BP.csv)
- [DMG](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-DMG-CVGOSlim-Frequency-BP.csv)

**Molecular function**:

- [Hypermethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHyper-CVGOSlim-Frequency-MF.csv)
- [Hypomethylated DML](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-12-02-Gene-Enrichment-Analysis/2019-07-30-condensedDMLHypo-CVGOSlim-Frequency-MF.csv)
- [DMG](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-08-14-Differentially-Methylated-Genes/2019-08-14-DMG-CVGOSlim-Frequency-MF.csv)

I'll take the categories with the most transcripts and start there for a discussion.

### Going forward

2. Update methods and results
3. Finalize figures
2. Update paper repository
3. Outline the discussion
4. Write the discussion
5. Write the introduction
6. Revise my abstract
7. Share the draft with collaborators and get feedback
8. Post the paper on bioRXiv
9. Prepare the manuscript for publication

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
