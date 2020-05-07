---
layout: post
comments: true
title: MethCompare Part 12
tags: MethCompare bedtools prop.test
---

## Characterizing union bedgraphs

I'm building up to [characterizing CpG locations in various genome features](https://github.com/hputnam/Meth_Compare/issues/60). To start, I translated my [individual sample characterization](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x.ipynb) and [figure creation](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.Rmd) pipeline to union bedgraphs. While having individual sample information might be nice when we decide to look at interindividual variation in methods, our methods comparisons will benefit from concatenating the three individual samples.

### Identifying methylated loci

I created [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation-5x-Union.ipynb) to run the union bedgraphs through my characterization pipeline. I downloaded the union bedgraphs from [this link](https://gannet.fish.washington.edu/seashell/bu-github/Meth_Compare/analyses/10-unionbedg/). Once I obtained the union data, I needed a way to average the three columns for each method while ignoring any missing values. This is easy to do in R, and I figured there would be a simmple way to do it with the shell or `awk`. After many attempts at targetted Google searches, I couldn't find anything! I posted [this issue](https://github.com/RobertsLab/resources/issues/924). Sam suggested I use `pandas`, which is a python tool that would allow me to manipulate dataframes in my Jupyter notebook.

First, I installed `pandas` for the notebook:

```
import pandas as pd
print(pd.__version__)
```

I then imported the full union bedgraph into `pandas` and averaged the three samples for each method across rows while ignoring missing values:

<img width="535" alt="Screen Shot 2020-05-07 at 9 53 20 AM" src="https://user-images.githubusercontent.com/22335838/81322276-98926f00-9048-11ea-8fd5-42aeb3f887eb.png">
<img width="803" alt="Screen Shot 2020-05-07 at 10 01 17 AM" src="https://user-images.githubusercontent.com/22335838/81323078-b44a4500-9049-11ea-9eef-a82312d8e1b8.png">
<img width="747" alt="Screen Shot 2020-05-07 at 10 01 28 AM" src="https://user-images.githubusercontent.com/22335838/81323102-bca28000-9049-11ea-9af0-c3153d1057da.png">

The function `.mean(axis = 1)` averages across rows and automatically ignores missing values. To save the dataframe, I used `df.to_csv` (confusing) but specified tab-delimited output. I also used `na_rep` to specify what character missing values should be saved as, and used `quoating = 3` to make sure my output did not have any quotes. I learned how to select columns from [this link](https://stackoverflow.com/questions/34734940/row-wise-average-for-a-subset-of-columns-with-missing-values) and navigated [this `pandas` page](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_csv.html) for the output saving code. Not the greatest documentation imo.

I then separated all the methods, removed lines where the percent methylation was "N/A", and counted the number of lines with data:

```
#Remove header
#Keep chr, start, end, and WGBS average (col 2-4, 13)
#Remove rows where the 4th column (average %meth) is N/A
#Save file
!tail -n +2 Mcap_union_5x-averages.bedgraph \
| awk -F'\t' -v OFS='\t' '{print $2, $3, $4, $13}' \
| awk -F'\t' -v OFS='\t' '$4 != "N/A"' \
> Mcap_union_5x-averages-WGBS.bedgraph
```

I had to repeat this step a few times because when I would move forward, I learned that I didn't correctly save the columns! I saved columns 1-3 for all methods, which was actually row number, chromosome, and start instead of chromosome, start, end. Whoops. The counts for *M. capitata* are [here](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union_5x-averages-counts.txt) and the counts for *P. acuta* are [here]().

### CpG locations in the genome

Once I finalized my union bedgraphs, I went through and identified methylated, sparsely methylated, and unmethylated by method for *M. capitata* and *P. acuta*. Again, I had to mess around with it a few times because I wanted to use a loop to call all my relevant files, but needed to figure out what wildcard I needed. Which also involved going back and remaking my method bedgraphs when I wanted to test new filenames. Once I identified the loci, I saved counts for each file and added the intermediate files to the `.gitignore`.

**M. capitata**:

- [Methylated loci](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union_5x-Meth-counts.txt)
- [Sparsely methylated loci](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union_5x-sparseMeth-counts.txt)
- [Unmethylated loci](https://github.com/hputnam/Meth_Compare/blob/master/analyses/Characterizing-CpG-Methylation-5x-Union/Mcap/Mcap_union_5x-unMeth-counts.txt)

**P. acuta**:

- [Methylated loci]()
- [Sparsely methylated loci]()
- [Unmethylated loci]()

**M. capitata**:

- [Genes]()
- [CDS]()
- [Introns]()
- [Flanking regions]()
- [Intergenic regions]()

**P. acuta**:

- [Genes]()
- [CDS]()
- [Introns]()
- [Flanking regions]()
- [Intergenic regions]()

### Stacked barplots

### Going forward

4. Update methods
5. Update results
2. [Locate TE tracks](https://github.com/hputnam/Meth_Compare/issues/56)
2. [Characterize intersections between data and TE, and create summary tables](https://github.com/hputnam/Meth_Compare/issues/59)
3. [Create figures for CpG characterization in various genome features](https://github.com/hputnam/Meth_Compare/issues/60)
3. Figure out methylation island analysis

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
