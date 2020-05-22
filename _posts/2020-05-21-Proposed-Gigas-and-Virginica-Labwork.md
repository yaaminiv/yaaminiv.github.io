---
layout: post
comments: true
title: Proposed Gigas and Virginica Labwork
tags: ATAC-Seq virginica gigas-broodstock larvae
---

## Retooling labwork plans

In January I started [thinking about *C. gigas* labwork](https://yaaminiv.github.io/Gigas-Labwork-Plans/). I [tried extracting DNA and RNA from *C. gigas*](https://yaaminiv.github.io/Gigas-Broodstock-RNA-Extraction/) but put the rest of that labwork on hold until I finished my quals. Now that I'm done with quals and have some time before I get back into t he lab because of COVID-19 concerns, I looked at all of my samples and drafted a plan for sequencing *C. virginica* and *C. gigas* samples.

### ATAC-Seq discussion with Genewiz

My main concerns with ATAC-Seq was optimizing a tranpsosase reaction and using frozen tissue, which [others echoed](https://yaaminiv.github.io/Figuring-out-ATAC-Seq/). Steven found a Genewiz protocol for ATAC-Seq that doesn't involve a transposase reaction! I would have to dissociate cells, clean up any dead cells with DNase, and cryopreserve the cells before sending them off. This addressed one of my concerns, but there wasn't anything in there about using frozen tissue as an input.

I chatted with a Genewiz representative to figure out if I could use frozen tissue, how successful the protocol would be, and get some cost estimates. They were concerned about frozen tissue, but suggested I investigate viability myself. I would thaw my tissue samples, dissociate cells, clean with DNase, and refreeze using their cryopreservation medium. Then, I would thaw the cells and stain it with [Trypan Blue](https://www.thermofisher.com/order/catalog/product/15250061). The dye would only stain permeable, or dead cells. Viable cells would have intact cell membranes so they wouldn't be permeable. At least 80-90% viability (50,000 cells per sample) would be ideal for ATAC-Seq. Below that, they could do a dead cell removal, or look into their low ATAC-Seq input pipeline. I'd have to talk to them again once I do those tests.

Assuming all things work out and I have >80% viability (REALLY HOPING THIS HAPPENS...I guess I could even pool samples together but that's a post-protocol test discussion), I can send samples to be sequenced. They aim for 30-50 million reads per sample to get adequate coverage, but the representative I spoke with suggested we can aim for the lower end of the spectrum and probably be okay. I also asked about their combined RNA-Seq analyses with ATAC-Seq. They take the pellet of cells and use half of it for ATAC-Seq and the other half for RNA-Seq. If I wanted to avoid additional freeze-thaw cycles, it would probably be better for me to split the pellet myself before cryopreservation, then take the half I wanted for MBD-BS and RNA-Seq and prepare those libraries myself.

### Hypotheses to test

Once I had ATAC-Seq information, I stepped back and thought about what questions I wanted to ask with this data and what associated hypotheses I had. 

1. **Is environmentally-sensitive differential methylation inherited by offspring, and does it impact gene expression?**

I believe it is heritable to some degree, and that offspring methylomes would be more similar to adult reproductive tissue than to somatic tissue. I also predict that hypomethylated genes, or environmental response genes, will have increased expression.

2. **How do chromatin state and methylation interact to affect gene expression in response to ocean acidification?**

According to Gatzmann et al. (2018), open chromatin states are more associated with hypermethylation and subsequently lower expression, while closed chromatin states are associated with hypomethylation and higher gene expression. I hypothesize that the same would be true with oysters. These chromatin states will differ between somatic and reproductive tissue because gametes undergo lots of chromatin packaging. I also predict that environmental response genes associated with ocean acidification response would be in these closed chromatin conformations and be hypomethylated.

3. **How do epigenetic responses between congeneric species differ?** 

Based on my [preliminary analysis](https://yaaminiv.github.io/Gigas-and-Virginica-Comparison-Part4/) and just general understanding of the species, I wouldn't expect any large diffences in their responses. If anything, there may be certain biological processes that are different, but even this detection could be an artifact of genome annotation quality.

### Samples needed

Now the tricky part: the samples. Questions 1 and 2 would be separate papers on their own, but each would incorporate a species comparison (Question 3). So that's quite a few samples to juggle.

To examine intergenerational epigenetic inheritance, I would want adult somatic and reproductive tissue, as well as offspring tissues. Reproductive tissue makes sense: I want to see if methylation patterns in the adult are carried through into the offspring. I would use the somatic tissue to demonstrate that offspring methylomes are more similar to reproductive tissue than somatic tissue. With *C. virginica*, I can use adult mantle, sperm, eggs, and either zygotes or larvae (depends on what Alan can extract). I will also use my *C. gigas* adult mantle, gonad, and larvae. Between species, I could compare offspring-offspring, gonad-gametes, and mantle-mantle. Finally, I would relate methylation back to gene expression.

Once I process all of the methylation and gene expression data above, I can then add the final layer: chromatin state information from ATAC-Seq. I won't use ATAC-Seq with offspring samples, but I would like to use *C. virginica* mantle, sperm, and eggs, and *C. gigas* adductor, ctenedia, and mantle. This allows me to look at somatic-reproductive tissue differences in *C. virginica*, differences in somatic tissue in *C. gigas*, and species differences in mantle tissue. I'm toying with the idea of doing a fourth-corner analysis with phenotype, ATAC-Seq, and methylation data since the ultimate goal is understanding how that impacts gene expression.

I compiled all of this information into [this spreadsheet](https://docs.google.com/spreadsheets/d/1F0WZmDFh5ZyNSMKsNz5OU1oLLZOLHJNC0DzvSxE_Zac/edit#gid=0) and included cost estimates. When thinking about what samples to axe if needed, I think the easiest thing to cut would be *C. gigas* adult tissue for ATAC-Seq. While it would be nice to do a species comparison for ATAC-Seq, *C. virginica* has a better quality genome which is definitely a priority.

### Going forward

1. Pitch plan and get feedback
2. Order necessary materials for extractions and protocol tests
3. Test ATAC-Seq protocol
4. Test RNA extraction protocol with tissue in histology blocks
5. Test DNA and RNA extractions with Trizol
6. Process all remaining adult samples
7. Extract DNA and RNA from larvae

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
