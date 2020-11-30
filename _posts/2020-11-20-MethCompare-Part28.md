---
layout: post
comments: true
title: MethCompare Part 28
tags: MethCompare
---

## General methylation calculations

After revising the coral methods comparison paper, we decided to scrap the methylation characterization for individual samples and focus on the genomes themselves. Since the different library preparation methods had species-level differences, investigating how baseline genome methylation contributed to these differences strengths the manuscript. 

I returned to [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/code/02.04-Characterizing-CpG-Methylation-5x-Union.ipynb) to average methylation values in the union bedgraphs. For each CpG locus, I averaged methylation across all samples (irrespective of library preparation method) and saved it in a new column:

```
df['total'] = df[['10', '11', '12', '13', '14', '15', '16', '17', '18']].mean(axis=1) #Averaging columns for Mcap data
df['total'] = df[['1', '2', '3', '4', '5', '6', '7', '8', '9']].mean(axis=1) #Averaging columns for Pact data
```

Then, I saved the all sample average methylation information in a separate bedgraph, along with chromosome and start and stop bp information for all loci with data:

```
#Remove header
#Keep chr, start, end, and total average (col 2-4, 14)
#Remove rows where the 4th column (average %meth) is N/A
#Save file
!tail -n +2 Mcap_union_5x-averages.bedgraph \
| awk -F'\t' -v OFS='\t' '{print $2, $3, $4, $14}' \
| awk -F'\t' -v OFS='\t' '$4 != "N/A"' \
> Mcap_union_5x-averages-total.bedgraph
```

Since I kept the file naming convention consistent for these new bedgraphs, I simply used the rest of my code to characterize the percent methylation as high (≥50%), moderate (10-50%), or low (≤10%). I used `wc` to count the number of CpG with data for each category.

**Table 1**. Total CpG with data for each species and methylation category.

|  **Species**  | **Total** | **High** | **Moderate** |  **Low** |
|:-------------:|:---------:|:--------:|:------------:|:--------:|
| _M. capitata_ |  13340268 |  1526193 |    1196995   | 10617080 |
|   _P. acuta_  |  7326297  |  212590  |    426115    |  6687592 |

We have more CpG data for *M. capitata*, which could be because it's a larger genome. Even though both genomes are predominantly unmethylated, *M. capitata* is more methylated than *P. acuta*, with 11.4% of *M. capitata* and 2.9% of *P. acuta* CpGs being more than 50% methylated. We saw this in Hollie's circos plots, but it's nice to have an actual number too.

While coding, I updated any file paths I needed to, and made sure I didn't create any associated BEDfiles from these total averages that would mess up my genomic location code. I also went through [this R Markdown script](https://github.com/hputnam/Meth_Compare/blob/master/code/02.05-Characterizing-CpG-Methylation-5x-Union-Summary-Plots.Rmd) to fix paths and ensure I could still make a stacked barplot, even if it was only for the supplemental material.

### Going forward

1. Determine a valid statistical approach for genomic location characterization
2. Update methods and results
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
  
