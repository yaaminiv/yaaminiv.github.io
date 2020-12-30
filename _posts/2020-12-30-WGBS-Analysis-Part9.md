---
layout: post
comments: true
title: WGBS Analysis Part 9
tags: manchester gigas-broodstock GOslim methylation-islands
---

## Investigating methylation island characteristics

SICB is just around the corner, and I'm presenting my *C. gigas* WGBS preliminary analysis! I was hoping for a full dataset but alas...Zymo. I already recorded and uploaded my talk (found [here]()), but I wanted to investigate characteristics associated with methylation islands before I decided to update (or not to update) my talk. Based on [Long et al. 2013](doi.org/10.7554/eLife.00348), I wanted to look at biological processes associated with gene-methylation island overlaps. In that paper, they found that different methylation island lengths were associated with distinct biological processes. I wasn't sure how much of this information I could reliably get from such a small dataset, but here's to trying.

### Creating a master table

Reviewing the files [in my repository](https://github.com/RobertsLab/project-gigas-oa-meth/), I found DML-MI overlaps, gene-MI overlaps, and biolgical process GOSlim terms by *C. gigas* gene ID. I already had most of the starting materials I needed! I opened [this R Markdown script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2020-02-11-Characterizing-CpG-Methylation/2020-02-11-Characterizing-Methylation-Islands.Rmd) to import and reformat files. For the DML-MI overlaps, I counted the frequency of each methylation island in the overlap file so I could have a total number of DML that overlapped with each methylation island, as opposed to the location of each overlap:

```{r}
DMLMIOverlapCounts <- as.data.frame(table(DMLMI$MI.start.end)) #Count the frequency of each methylation island in MI-DML overlaps
colnames(DMLMIOverlapCounts) <- c("MI.start.end", "DML.overlaps") #Rename columns
head(DMLMIOverlapCounts) #Confirm counting
```
I then merged the gene-MI and BP GOSlim information by gene ID, and merged the resultant file with MI-DML frequency by MI start and end position. I included all rows when merging with DML information, and replaced N/A with 0 to represent no DML overlapping with that particular MI. I saved my master table [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2020-02-11-Characterizing-CpG-Methylation/2020-12-30-Gene-MI-Overlaps-BP-DMLCount.txt).

### Differences between MI categories

To categorize MI, I imported my [MI list](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2020-02-11-Characterizing-CpG-Methylation/2020-02-11-Methylation-Islands-500_0.02_50-filtered.tab). Then, I obtained the length for each MI, and plotted a histogram. The histogram wasn't very revealing when it came to length trends, so I identified the median length. The median length was 1010 base pairs. I decided to use "short" (≤ median) and "long" (> median) categories for the analysis.

I started by looking at the number of DML associated with short or long MI. As expected, higher numbers of DML could be found overlapping with long MI, but both short and long MI primarily had 0 DML. If they had any overlapping DML, it was likely they only had 1 overlapping DML. This is interesting because it tells me that there aren't clusters of DML in methylation islands: they're relatively spaced out. Of course, this is a preliminary dataset so it's probably something to look into with multiple samples.

Then, I examined the GOSlim terms associated with short and long MI. I determined the frequency of each GOSlim term in the short and long MI lists, then calculated percentages. I plotted a histogram for a cursory look and didn't see any differences! Since I didn't see any GOSlim term differences in genes with DML and all genes in either *C. gigas* or *C. virginica*, it tracks that I wouldn't see anything at the GOSlim level with methylation island lengths.

### Adding GOterms

I thought that the GOSlim level was too coarse to see any changes in individual GOterms between short and long MI. I opened [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/notebooks/2019-09-15-DML-Analysis.ipynb) and saved an intermediate file with gene ID, GOterm, and GOSlim terms. Then I imported that file in [my R Markdown script]() and merged it with the other files to create a revised master list. Then, I looked at GOterm differences between short and long MI, and saved that information [here](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2020-02-11-Characterizing-CpG-Methylation/2020-12-30-BPGOterm-shortMI-longMI.txt).

Since there are thousands of GOterms, it was hard to understand differences using histograms! I decided to conduct a contingency test with the GOterm counts for short and long MI:

```{r}
BPGOtermTestResults <- data.frame() #Create empty dataframe to store chi-squared results
for(i in 1:ncol(BPGOtermTest)) { #For each GOterm
  shortGOterm <- BPGOtermTest[1,i] #Variable for # GOterms in short MI
  longGOterm <- BPGOtermTest[2,i] #Variable for # GOterms in long MI
  shortNotGOterm <- sum(BPGOtermTest[1,-i]) #Variable for # other GOterms types in short MI
  longNotGOterm <- sum(BPGOtermTest[2,-i]) #Variable for # other GOterms in long MI
  ct <- matrix(c(shortGOterm, longGOterm, shortNotGOterm, longNotGOterm), ncol = 2) #Create contingency table
  colnames(ct) <- c(as.character(colnames(BPGOtermTest[i])), paste0("Not", colnames(BPGOtermTest[i]))) #Assign column names: type, not type
  rownames(ct) <- c(as.character(row.names(BPGOtermTest)[c(1,2)])) #Assign row names
  print(ct) #Confirm table is correct
  ctResults <- data.frame(broom::tidy(chisq.test(ct))) #Create dataframe storing chi-sq stats results. Use broom::tidy to extract results from test output
  ctResults$BPGOterm <- as.character(colnames(BPGOtermTest)[i]) #Add GOterm to results
  BPGOtermTestResults <- rbind(BPGOtermTestResults, ctResults) #Add test statistics to master table
}
```

I then corrected p-values using an FDR, and saved only significantly different GOterms in [this file](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/analyses/2020-02-11-Characterizing-CpG-Methylation/2020-12-30-BPGOterm-shortMI-longMI-ContTest-SigResults.txt), along with the original frequencies and percentages for short and long MI. There are ~1500 GOterms that are significantly different between short and long MI. Since this doesn't really have a direct connection to my SICB talk, I decided to not include it — especially because I really don't know what any of this means functionally, and I want to confirm these results with the larger dataset. I uploaded some supplementary material to my talk instead: the methylation island analysis from OSM, the *C. virginica* methylation paper, and the larval carryover effect paper. Hopefully in the two months that my SICB talk will be available, I'll be able to share actual results from the full sample dataset!

### Going forward

1. Analyze full sample data when it arrives!
2. Determine if RNA should be extracted
3. Determine if larval DNA/RNA should be extracted

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
