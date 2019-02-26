---
layout: post
comments: true
title: DML Analysis Part 23
tags: virginica MBDSeq gene-enrichment
---

## DMR-mRNA gene product information

Since I didn't get much information from [my previous gene enrichment](), I thought I would start describing the function of coding regions with DMRs in them. When I looked at the [DMR-mRNA overlap file](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2018-11-07-DMR-mRNA.txt), I saw that there was gene product information buried in the last column:

```
ID=rna48;Parent=gene35;Dbxref=GeneID:111114201,Genbank:XM_022452489.1;Name=XM_022452489.1;Note=The sequence of the model RefSeq transcript was modified relative to this genomic sequence to represent the inferred CDS: inserted 2 bases in 2 codons;exception=unclassified transcription discrepancy;gbkey=mRNA;gene=LOC111114201;model_evidence=Supporting evidence includes similarity to: 4 Proteins%2C and 99%25 coverage of the annotated genomic feature by RNAseq alignments%2C including 9 samples with support for all annotated introns;product=vacuolar protein sorting-associated protein 13B-like;transcript_id=XM_022452489.1
```

My first step was to isolate the `product=` information from this column. The easiest thing to do would be to use `awk` to separate the text into multiple columns based on a `;` delimiter. However, there are different numbers of `;` in each line, so the product informaiton would not consistently be in the same column throughout the document. I wanted to separate out all product information, either by creating a custom delimiter or extracting all information after `product=`. I figured there'd be an elegant way to do this with bash, so I posted [this issue](https://github.com/RobertsLab/resources/issues/595). Unfortunately Sam couldn't help me with a solution! I took the easy way out and used Excel.

I created a duplicate column, replaced `product=` with `>`, then used `>` as a delimiter to separate product information from the rest of the column. I then used `;` as a delimiter with the column I just generated to separate the product information from the transcript ID. Finally, I deleted columns without product information that I generated throughout this process. My final document, found [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2019-02-25-DMR-mRNA-Product-Isolated.txt), looks like this:

![screen shot 2019-02-25 at 4 14 47 pm](https://user-images.githubusercontent.com/22335838/53377774-022cce80-3918-11e9-8d64-591bf78c068d.png)

I know you can use `sed` for a find-and-replace to do something similar in bash, but I couldn't get that to work:

![screen shot 2019-02-25 at 4 02 13 pm](https://user-images.githubusercontent.com/22335838/53377824-3d2f0200-3918-11e9-975c-564748d31a61.png)

Now that I had the product information isolated, I wanted to create a new document with summary information (gene product, percent methylation difference, number of transcript variants the DMR was found in). I figured this summary document would be a good starting place when I start writing. The summary document can be found [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2018-11-01-DML-and-DMR-Analysis/2019-02-25-DMR-mRNA-Product-Summary.txt). DMRs were found in interesting genes, including calcium uptake (could help with calcification), cilia and flagella associated protein (sperm motility, cellular structure), cytochrome P450 (oxidative stress protein), tubulin-specific chaperones (motility-related), telomere-associated protein (could be related to cell replication and apoptosis), sperm-tail PG-rich repeat-containing protein (no idea what this does but my guess is that it's sperm-related), and zygotic DNA replication. There were also 37 uncharacterized gene products.

### Going forward

1. Visualize gene product information
2. Relate this information to gene enrichment

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
