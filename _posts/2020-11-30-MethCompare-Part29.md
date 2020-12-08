---
layout: post
comments: true
title: MethCompare Part 29
tags: MethCompare
---

## Returning to contingency tests

After weeks of procrastination because...well I don't have an admirable "because" (end of quarter burnout that magically starts before the end of the quarter anyone?), but I'm finally revising statistical methods! Based on a discusison I had with Katie [in relation to this issue](https://github.com/hputnam/Meth_Compare/issues/68), my original method of contingency tests to discern library preparation method-based differences in CpG location was the right approach. Even though the large sample sizes would lead to significant results, I could qualify those responses with an effect size in the discussion text. 

For both *M. capitata* and *P. acuta*, I looked at differences in CpG location (CDS, intron, upstream flank, downstream flank, and intergenic) between the methods (WGBS vs. RRBS, WGBS vs. MBDBS, and RRBS vs. MBDBS). I also examined differences between genomic CpG locations and the locations of CpG with data for each method. I used the [5x union BEDgraph data I generated for each method](https://github.com/hputnam/Meth_Compare/blob/master/code/02.04-Characterizing-CpG-Methylation-5x-Union.ipynb) for this analysis.

In [this script](https://github.com/hputnam/Meth_Compare/blob/master/code/02.05-Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd), I conducted contingency tests based on my original code! I was hoping to find it in the file's revision history, but since all my code files have been renamed, the previous commits weren't available. Thank goodness I included it in [this lab notebook post](https://yaaminiv.github.io/MethCompare-Part15/). 

First, I reformatted the data so the CpG counts for each genomic location were organized by method. For each method comparison, I subsetted the necessary rows, counted the number of CpGs in a genomic feature and not in that feature, then performed the chi-squared contingency test:

```{r}
McapCpGLocationWGRR <- data.frame() #Create empty dataframe to store chi-squared results
for(i in 1:ncol(McapCpGLocationStatTest)) { #For each genome feature
  Method1Feature <- McapCpGLocationStatTest[2,i] #Variable for # genome feature overlaps for method 1
  Method2Feature <- McapCpGLocationStatTest[3,i] #Variable for # genome feature overlaps for method 2
  Method1NotFeature <- sum(McapCpGLocationStatTest[2,-i]) #Variable for # other CpG types for method 1
  Method2NotFeature <- sum(McapCpGLocationStatTest[3,-i]) #Variable for # other CpG types for method 2
  ct <- matrix(c(Method1Feature, Method2Feature, Method1NotFeature, Method2NotFeature), ncol = 2) #Create contingency table
  colnames(ct) <- c(as.character(colnames(McapCpGLocationStatTest[i])), paste0("Not", colnames(McapCpGLocationStatTest[i]))) #Assign column names: type, not type
  rownames(ct) <- c(as.character(row.names(McapCpGLocationStatTest)[c(2,3)])) #Assign row names: method 1, method 2
  print(ct) #Confirm table is correct
  ctResults <- data.frame(broom::tidy(chisq.test(ct))) #Create dataframe storing chi-sq stats results. Use broom::tidy to extract results from test output
  ctResults$GenomeFeature <- as.character(colnames(McapCpGLocationStatTest)[i]) #Add CpG type to results
  McapCpGLocationWGRR <- rbind(McapCpGLocationWGRR, ctResults) #Add test statistics to master table
}
```

I then used an FDR threshold to correct for multiple comparisons and added comparison information to the data table:

```{r}
McapCpGLocationWGRR$p.adj <- p.adjust(McapCpGLocationWGRR$p.value, method = "fdr") #Correct p-value using FDR
McapCpGLocationWGRR$comparison <- rep("WGBS vs. RRBS", times = 5) #Add methods compared
head(McapCpGLocationWGRR) #Confirm changes
```

I saved the statistical output for all *M. capitata* comparisons [here](https://github.com/hputnam/Meth_Compare/blob/master/output/intermediate-files/02-Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union-CpG-Location-StatResults.txt) and for all *P. acuta* comparisons [here](https://github.com/hputnam/Meth_Compare/blob/master/output/intermediate-files/02-Characterizing-CpG-Methylation-5x-Union/Pact/Pact_union-CpG-Location-StatResults.txt). As expected, almost all comparisons were significant. I need to create supplemental tables and update my methods and results information in the manuscript.

### Going forward

1. Update methods and results
2. Contribute to discussion

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
  
