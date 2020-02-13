---
layout: post
comments: true
title: Gigas and Virginica Comparison
tags: manchester gigas-broodstock virginica methylation-islands
---

## Comparing *Crassostrea spp.* methylation patterns

Guess I got to put together a poster for ASLO! My goal is to showcase some preliminary comparisons of methylation patterns in *C. gigas* and *C. virginica* gonad tissue, and DML in reponse to experimental ocean acidification.

### Method comparison

Although the experiments are similar, there are some key differences in the experimental design that I need to consider. I made some summary tables to easily compare the experiments.

The first takeaway is that the *C. gigas* experimental duration was longer (7 weeks vs. 4 weeks), and potentially more extreme. Although the treatment pH conditions were the same for both species, pCO<sub>2</sub> was higher for *C. gigas* in both experimental conditions.

**Table 1**. Carbonate chemistry parameters for both experiments. The *C. gigas* experiment took place over 7 weeks and had ambient inflow, while the *C. virginica* experiment was 4 weeks long and had controlled, not ambient, inflow.

| **Species** |    **pH**   | **pCO<sub>2</sub> (µatm)** |  **DIC**  | **A<sub>T</sub>** | **Ω<sub>calcite</sub>** |
|:---------------------------------:|:-----------:|:--------------------------:|:---------:|:-----------------:|:-----------------------:|
|        *C. gigas* (ambient)       | 7.82 ± 0.02 |          863 ± 42          | 2533 ± 35 |     2611 ± 31     |       2.13 ± 0.06       |
|       *C. gigas* (treatment)      | 7.29 ± 0.01 |          3344 ± 50         | 2920 ± 15 |     2808 ± 12     |       0.68 ± 0.01       |
|      *C. virginica* (control)     | 7.95 ± 0.01 |          492 ± 50          | 1960 ± 32 |     2140 ± 15     |       3.01 ± 0.25       |
|     *C. virginica* (treatment)    | 7.29 ± 0.01 |         2550 ± 211         | 2173 ± 37 |     2132 ± 42     |       0.72 ± 0.06       |

The *C. gigas* analysis had a smaller sample size. Pooled samples were used since I had lower DNA yield from histology blocks than from the tissue Katie and Alan sent. To compensate for the smaller sample size, I used more stringent analysis parameters (minimum sequencing depth used in downstream analyses and `bismark` alignment score). Another important note is that I only sequenced stage 2 female gonad for *C. gigas*, but sex and maturity are unknown for *C. virginica*.

**Table 2**. Sequencing and analysis parameters. For *C. gigas*, one pooled sample with two individuals of the same sex and reproductive sstage was created for each treatment. Alignment score refers to the `score_min` parameter used by `bismark`.

|   **Species**  |     **Sample size**     | **Sequencing type** | **Minimum Sequencing Depth** | **Alignment Score** |
|:--------------:|:-----------------------:|:-------------------:|:----------------------------:|:-------------------:|
|   *C. gigas*   | 2 (pooled; 1/treatment) |         WGBS        |              10x             |        0,-0.9       |
| *C. virginica* |     10 (5/treatment)    |      MBD-BSSeq      |              5x              |        0,-1.2       |


### Methylation island analysis

I want to replicate [this perl script](https://github.com/soojinyilab/Methylation-Islands/blob/master/methyl_island_sliding_window.pl) from [this paper](https://academic.oup.com/gbe/article/10/10/2766/5098531) to identify methylation islands (MI) in the *C. gigas* and *C. virginica* genomes. The goal is to create a MI track for each species that I can then compare with DML in response to ocean acidification.

My perl comprehension is pretty low…but from what I can tell, all I need to do is:

- Choose a window size: Probably going to start with 200 bp since that’s what the paper I’m replicating used, but I'll try 300 bp too
- Define mCpG fraction: Again, starting with 0.02 since that’s what the authors used, but I want to play around with this since insect genomes are less methylated than *C. virginica*.
- Choose a step size: 50 bp (again from the paper)
- Provide a list of mCpG in the genome (column 1: scaffold, column 2: bp): The paper used [Bis-class](http://bibs.snu.ac.kr/software/Bisclass/) to identify mCpG, but I already identified CpG as methylated using a 50% methylation cut-off for *C. virginica*, and Claire and Mac have done this previously with *C. gigas*

I'm going to start with *C. virginica*. Since my BEDfile of methylated loci has three columns instead of two, I started by creating a new file with only two columns: chromosome and mCpG position in [this Jupyter notebook](https://github.com/fish546-2018/yaamini-virginica/blob/master/notebooks/2019-03-18-Characterizing-CpG-Methylation.ipynb).

```
awk '{print $1"\t"$2}' 2019-04-09-All-5x-CpG-Loci-Methylated.bed > 2019-04-09-All-5x-CpG-Loci-Methylated-Reduced.bed #Create new tab-delimited file with only chromosome and mCpG position
```

Then, I cloned the script to my repo and saved it [here](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-18-Characterizing-CpG-Methylation/methyl_island_sliding_window.pl). Finally, I ran the script in my Jupyter notebook based on the README.md instructions! I ran several iterations of the script with different mCpG fractions.

```
#Run script with 200 bp window, 0.02 mCpG fraction, and 50 bp step size
./methyl_island_sliding_window.pl 200 0.02 50 2019-04-09-All-5x-CpG-Loci-Methylated-Reduced.bed > 2020-02-06-Methylation-Islands-200_0.02_50.tab
```

I tried this with various mCpG fractions and initial widow sizes and saved the output [in this folder](https://github.com/fish546-2018/yaamini-virginica/tree/master/analyses/2019-03-18-Characterizing-CpG-Methylation). I really should have found a way to generate all these files programmatically...but here we are!

**Table 3**. MI statistics for various parameters. In general, MI number decreases as mCpG fraction and window size increase.

| **Initial Window Size (bp)** | **mCpG Fraction** | **Number of MI** | **Max mCpG in MI** | **Min mCpG in MI** |
|:----------------------------:|:-----------------:|:----------------:|:------------------:|:------------------:|
|              200             |        0.02       |      119705      |        24777       |          4         |
|              200             |        0.03       |      129006      |        8305        |          6         |
|              200             |        0.04       |      113806      |        1682        |          8         |
|              200             |        0.05       |       93229      |        1452        |         10         |
|              200             |        0.10       |       18719      |        1167        |         20         |
|              200             |        0.15       |       2453       |         177        |         30         |
|              200             |        0.20       |        320       |         94         |         40         |
|              200             |        0.25       |        37        |         63         |         50         |
|              200             |        0.27       |         8        |         57         |         54         |
|              200             |        0.30       |         0        |          0         |          0         |
|              300             |        0.02       |       91756      |        24777       |          6         |
|              300             |        0.03       |       91833      |        8305        |          9         |
|              300             |        0.04       |       74497      |        1682        |         12         |
|              300             |        0.05       |       53510      |        1452        |         15         |
|              300             |        0.10       |       6629       |        1167        |         30         |
|              300             |        0.15       |        546       |         177        |         45         |
|              300             |        0.20       |        20        |         94         |         60         |
|              300             |        0.25       |         0        |          0         |          0         |       |

Now that I have all of these different MI tracks, I need to visualize them in IGV to 1) make sure methylation islands make sense in general and 2) see which set of parameters best fits my data. I created a [new IGV session](https://github.com/fish546-2018/yaamini-virginica/blob/master/analyses/2019-03-07-IGV-Verification/2020-02-06-Methylation-Island-Verification.xml) and added in bedgraphs with the locations of all CpGs from the concatenated 5x data, as well as just the methylated CpGs, using `gannet` links so someone else could view my session easily. My tab-delimited files have an extra column that counts the number of mCpG in each MI. Unfortunately IGV wasn't too happy about this extra column when I tried to import and view it. I used the following loop to create BEDfiles from my tab-delimited output:

```
%%bash
for f in *.tab
do
    awk '{print $1"\t"$2"\t"$3}' ${f} > ${f}.bed
done
```
I loaded all my BEDfiles in IGV! And looked at the MI tracks at various resolutions.

![Screen Shot 2020-02-06 at 7 34 32 PM](https://user-images.githubusercontent.com/22335838/73999046-5efe7a80-4918-11ea-95de-529dc4840e29.png)

![Screen Shot 2020-02-06 at 7 34 53 PM](https://user-images.githubusercontent.com/22335838/73999050-60c83e00-4918-11ea-94c2-cfb37814a043.png)

![Screen Shot 2020-02-06 at 7 35 25 PM](https://user-images.githubusercontent.com/22335838/73999053-61f96b00-4918-11ea-968a-8ac34b604e80.png)

![Screen Shot 2020-02-06 at 7 36 05 PM](https://user-images.githubusercontent.com/22335838/73999055-632a9800-4918-11ea-826c-f056aaa88e4f.png)

![Screen Shot 2020-02-06 at 7 36 44 PM](https://user-images.githubusercontent.com/22335838/73999059-658cf200-4918-11ea-82bf-f3a21a7db07b.png)

**Figures 1-5**. MI tracks generated using various parameters. Blue MI tracks are those from 200 bp windows, and purple MI tracks are those from 300 bp windows. The mCpG fraction increases down the screen.

After creating BEDfiles and putting them on gannet, I added them to IGV. Based on my IGV session, it seems like the 0.02 mCpG fraction actually does a pretty good job of capturing the methylation islands. I just don't know if I should choose the 200 or 300 bp windows. I'm leaning towards 200 bp just because that's the original method and I don't really have a better way to choose. I posted [this issue](https://github.com/RobertsLab/resources/issues/834) to get feedback.


### Going forward

1. Generate *C. gigas* methylation islands
2. Charaterize *C. gigas* and *C. virginica* DML in relation to methylation islands
3. Compare general methylation landscapes
3. Complete a GO-MWU and DMG analysis with *C. gigas*
2. Compare *C. gigas* and *C. virginica* DML
3. Draft poster for ASLO and get feedback
4. Finalize ASLO poster and print!

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
