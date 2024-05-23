---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 25
tags: killifish RRBS BAT
---

## Characterizing the general methylation landscape with the new genome

When looking through the output from the new genome to characterize the methylation landscape in [this Jupyter notebook](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-methylation-landscape-analysis.ipynb), I noticed very few common loci used in comparisons. This was a red flag for me: if the algorithim doesn't have >100,000 loci to run comparisons, then I wouldn't trust its conclusions! So here we are, troubleshooting.

### `BAT_summarize` and `BAT_overview`

There are two things I can mess around with: missingness and coverage. Right now, my coverage is 10 reads per locus. I could drop down to five, but I'd like to keep 10 if possible. So that bring us to missingness. The [default for `BAT_summarize`](http://legacy.bioinf.uni-leipzig.de/Software/BAT/analysis/) is to only consider loci with data for all samples. I relaxed this to allow for one sample missing data per group for a comparison (`--mis1 1` and `--mis2 1`) in [this script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-BAT-summarize.sh). I can't go above this since some of my population x oxygen treatment conditions only have three samples. I then reran my [`BAT_analysis`](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-analysis.sh) script. I ended up with some weird extra files with incorrect naming conventions, like `5_OC_N/N_metilene_NO_OC.txt` and `5_OC_S/S_metilene_NO_OC.txt`. I deleted all my output and reran my script and didn't end up with those files! Who knows what happened. With my remaining files, I looked at the number of common loci used for each comparison.

**Table 1**. Number of common loci for various comparisons

| **Population** |  **Treatment**  | **Old Genome** | **New Genome** | **New Genome, Missing 1** |
|:--------------:|:---------------:|:--------------:|:--------------:|:-------------------------:|
|    NB vs. SC   |       All       |      14896     |        2       |           148753          |
|    NB vs. SC   | Outside Control |     116535     |        6       |           782514          |
|    NB vs. SC   |     Normoxia    |      74029     |        5       |           852762          |
|    NB vs. SC   |     Hypoxia     |     197124     |        6       |          1008741          |
|   New Bedford  |    OC vs. NO    |     152299     |        8       |           913977          |
|   New Bedford  |    OC vs. HY    |       N/A      |     550768     |          1117814          |
|  New Bedford   |    NO vs. HY    |     176287     |        6       |          1071858          |
|  Scorton Creek |    OC vs. NO    |      59358     |        3       |           758536          |
|  Scorton Creek |    OC vs. HY    |       N/A      |     108815     |           729115          |
|  Scorton Creek |    NO vs. HY    |      82306     |        5       |           871574          |

WAY more loci when allowing for missingness! As expected, the comparison with all samples has the smallest number of common loci. I'll move forward with this output.

### Methylation landscape

I then returned to [this Jupyter notebook](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-methylation-landscape-analysis.ipynb) to recharacterize the methylation landscape with revised output. I also modified my approach. I created the union bedGraph to get an accurate count of unique loci used in the analysis. However, to actually calculate percent methylation, I decided to only look at the [`BAT_summarize` output for the all sample comparison](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/05-analysis/summarize/all_pop/all_pop_metilene_N_S.txt). This is a file with all common loci across all samples. By using this file, I can conservatively gauge the landscape, since the union bedgraph has loci with missing data for several samples (sometimes all but one!).

I imported the output file into `pandas`, then calculated average methylation for each population and treatment within each population at each locus:

```
#Import data into pandas
#Check head
df = pd.read_table("../../summarize/missing_1/all_pop/all_pop_metilene_N_S.txt")
df.head(5)
```

```
#Average all NBH samples for total genome methylation information and save as a new column
#Average all OC samples and save as a new column
#Average all NO samples and save as a new column
#Average all HY samples and save as a new column
#NA are not included in averages
#Check output
df['NBH.average'] = df[['N_OC-N5' ,'N_OC-N1' ,'N_OC-N2', 'N_OC-N4', 'N_20-N4', 'N_20-N2', 'N_20-N1', 'N_5-N1', 'N_5-N2', 'N_5-N3']].mean(axis=1)
df['NBH.OC.average'] = df[['N_OC-N5' ,'N_OC-N1' ,'N_OC-N2', 'N_OC-N4']].mean(axis=1)
df['NBH.NO.average'] = df[['N_20-N4', 'N_20-N2', 'N_20-N1']].mean(axis=1)
df['NBH.HY.average'] = df[['N_5-N1', 'N_5-N2', 'N_5-N3']].mean(axis=1)

#Average all SC samples for total genome methylation information and save as a new column
#Average all OC samples and save as a new column
#Average all NO samples and save as a new column
#Average all HY samples and save as a new column
#NA are not included in averages
#Check output
df['SC.average'] = df['average'] = df[['S_OC-S1', 'S_OC-S2', 'S_OC-S3', 'S_OC-S5', 'S_20-S1', 'S_20-S3', 'S_20-S4', 'S_20-S2', 'S_5-S3', 'S_5-S4', 'S_5-S2', 'S_5-S1']].mean(axis=1)
df['SC.OC.average'] = df[['S_OC-S1', 'S_OC-S2', 'S_OC-S3', 'S_OC-S5']].mean(axis=1)
df['SC.NO.average'] = df[['S_20-S1', 'S_20-S3', 'S_20-S4', 'S_20-S2']].mean(axis=1)
df['SC.HY.average'] = df[['S_5-S3', 'S_5-S4', 'S_5-S2', 'S_5-S1']].mean(axis=1)
```

I wrote out this file [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/05-analysis/new-genome/methylation-landscape/missing_1/all-common-loci-averages.bedgraph). Then, I used a series of `awk` commands to calculate percent methylation for each population and population x treatment:

```
#Remove header
#Find CpGs with > 0% methylation
#Count number of CpGs
! tail -n+2 all-common-loci-averages.bedgraph \
| awk -F'\t' -v OFS='\t' '{if ($26 > 0) { print $26 }}' ${f} \
| wc -l
```

```
#Remove header
#Count number of CpGs
! tail -n+2 all-common-loci-averages.bedgraph \
| wc -l
```

```
#Calculate average methylation
! tail -n+2 all-common-loci-averages.bedgraph \
| awk '{ total += $26; count++ } END { print total/count }'
```

**Table 2**. Average methylation for various treatments

| **Population** |  **Treatment**  | **Number of Methylated CpGs** | **Percent Methylation** |
|:--------------:|:---------------:|:-----------------------------:|:-----------------------:|
|   New Bedford  |       All       |             121017            |           28.3          |
|   New Bedford  | Outside Control |             101532            |           27.9          |
|   New Bedford  |     Normoxia    |             92498             |           27.2          |
|   New Bedford  |     Hypoxia     |             96541             |           29.7          |
|  Scorton Creek |       All       |             124001            |           28.8          |
|  Scorton Creek | Outside Control |             124001            |           28.8          |
|  Scorton Creek |     Normoxia    |             99786             |           28.8          |
|  Scorton Creek |     Hypoxia     |             101036            |           29.2          |

Overall, I wouldn't say there's a clear difference in average methylation by population and treatment. This doesn't surprise me since methylation in this species seems so minimal to begin with when compared with humans or mammals. I'll run this by Neel before I finalize any figures.

### `BAT_DMRcalling`

Since it takes literally two minutes, I decided to call DMR with the revised output! Now that there were more loci, I wondered if there would algorithmically be more DMR. Apart from a small issue where I couldn't call DMR comparing New Bedford and Scorton Creek outside control samples because the files were not in the right place, this step was pretty chill. The output can be found [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/06-DMR/new-genome/missing_1).

Just as a reminder to myself: DMR have at least 10 CpGs, a minimum absolute difference ≥ 0.1, and an adjusted p-value < 0.05. Previously [I also messed around with my `BAT_filter_vcf` settings such that my maximum read count was 500 instead of the default 100](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/04-BAT-calling-filtering.sh). [This was because I initially got 0 DMR for all contrasts with the old genome so we wanted to have more lenient filtering restrictions](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part14/).

**Table 3**. DMR for various comparisons

| **Population** |  **Treatment**  | **Old Genome** | **New Genome** | **New Genome, Missing 1** |
|:--------------:|:---------------:|:--------------:|:--------------:|:-------------------------:|
|    NB vs. SC   |       All       |        0       |        0       |             11            |
|    NB vs. SC   | Outside Control |        1       |        0       |             1             |
|    NB vs. SC   |     Normoxia    |        1       |        0       |             0             |
|    NB vs. SC   |     Hypoxia     |        1       |        0       |             13            |
|   New Bedford  |    OC vs. NO    |        2       |        0       |             10            |
|   New Bedford  |    OC vs. HY    |       16       |       59       |             59            |
|   New Bedford  |    NO vs. HY    |       N/A      |        0       |             23            |
|  Scorton Creek |    OC vs. NO    |        2       |        0       |             0             |
|  Scorton Creek |    OC vs. HY    |        1       |        0       |             0             |
|  Scorton Creek |    NO vs. HY    |        1       |        0       |             0             |

It's interesting to see that none of the oxygen treatment comparisons within Scorton Creek have any DMR! This could suggest that the more toxicant-sensitive Scorton Creek doesn't mount an adaptive response to hypoxia, whereas the more toxicant-resistant New Bedford Harbor has a fine-tuned molecular regulation system that includes methylation responses. It will be interesting to see where the DMR are located and if they overlap with any interesting pathways or DEG.

Another thing I see is that there are 11 DMR that are different between all New Bedford and all Scorton Creek samples. These could shed light on additional population-level differences. There are also DMR associated with hypoxia exposure between the two populations. I wonder if any of these DMR are in interesting regions, or if they overlap with the within-population oxygen treatment DMR. Lots of good stuff to investigate!

### Going forward

1. Update methods and results
2. Create methylation landscape figure
3. Determine the genomic locations of DMR
3. Create DMR figure
6. Update OSF repository for all intermediate files
1. Map STAR transcript names to Ensembl IDs from genome
2. Conduct pathway analysis for RNA-Seq data by population
5. Identify known SNP/DMR overlaps

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
