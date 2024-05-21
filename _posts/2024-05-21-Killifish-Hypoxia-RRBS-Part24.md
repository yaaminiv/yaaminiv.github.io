---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 24
tags: killifish RRBS BAT
---

## Returning to the killifish project (again)

Okay! Neel and I both have more time and momentum so we're picking this up again. My current task is to clarify the methylation analysis. When Neel was investigating the RNA data, he saw that there were more DEG, and more hypoxia-related DEG, when comparing the outside control to the normoxia and hypoxia treatments. It seems like the normoxia treatment wasn't actually normoxia, and instead a poorly mixed intermediate low oxygen treatment. In light of this, Neel suggested I examine differences between the outside control and hypoxia treatments in the methylation data.

### `BAT_summarize` and `BAT_overview`

Back to my old frenemy, `BAT`. I returned to [this script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-analysis.sh) to reorient myself to what needed to be done. I had [previously](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part21/) run the analysis with the new genome, so I just needed to add in this new comparison. Originally, Neel and I agreed we didn't need it. Whoops.

I added in code to make the hypoxia versus outside control comparisons for each population in the [`BAT_summarize`](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-BAT-summarize.sh) script:

```
echo "Oxygen within NB: Hypoxia vs. OC"

#Run BAT_summarize
#in1: comma-separated bedGraphs from NBH hypoxia
#in2: comma-separated bedGraphs from NBH OC
#groups: comma-separated list of gorup identifiers
#h1: comma-separated list of sample identifiers for group 1
#h2: comma-seaprated list of sample identifiers for group 2
#out: prefix for output files

BAT_summarize \
--in1 ${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-N1_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-N2_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_5-N3_CG.sort.bedgraph \
--in2 ${FILTERED}/190626_I114_FCH7TVNBBXY_L3_OC-N5_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_OC-N1_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_OC-N2_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_OC-N4_CG.sort.bedgraph \
--groups HY,OC \
--h1 5-N1,5-N2,5-N3  \
--h2 OC-N5,OC-N1,OC-N2,OC-N4 \
--out ${SUMMARIZE}/N \
--cs ${CHROMLENGTH}

#Move files to a new directory

mkdir ${SUMMARIZE}/5_OC_N
mv ${SUMMARIZE}/N_* ${SUMMARIZE}/5_OC_N/.

echo "Oxygen within SC: Hypoxia vs. OC"

#Run BAT_summarize
#in1: comma-separated bedGraphs from SC hypoxia
#in2: comma-separated bedGraphs from SC OC
#groups: comma-separated list of gorup identifiers
#h1: comma-separated list of sample identifiers for group 1
#h2: comma-seaprated list of sample identifiers for group 2
#out: prefix for output files

BAT_summarize \
--in1 ${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-S3_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-S4_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_5-S2_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_5-S1_CG.sort.bedgraph \
--in2 ${FILTERED}/190626_I114_FCH7TVNBBXY_L2_OC-S1_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_OC-S2_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_OC-S3_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_OC-S5_CG.sort.bedgraph \
--groups HY,OC \
--h2 5-S3,5-S4,5-S2,5-S1 \
--h2 OC-S1,OC-S2,OC-S3,OC-S5 \
--out ${SUMMARIZE}/S \
--cs ${CHROMLENGTH}

#Move files to a new directory

mkdir ${SUMMARIZE}/5_OC_S
mv ${SUMMARIZE}/S_* ${SUMMARIZE}/5_OC_S/.
```
And then the [`BAT_overview`](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-BAT-overview.sh) script:

```
echo "Oxygen within NB: Hypoxia vs. OC"

#Run BAT_overview
#-i: Input bedGraph from BAT_summarize
#-o: Output prefix
#--groups: comma-separated list of group identifiers

BAT_overview.R  \
-i ${SUMMARIZE}/5_OC_N/N_summary_HY_OC.bedgraph \
-o ${OVERVIEW}/5_OC_N \
--groups HY,OC

echo "Oxygen within SC: Hypoxia vs. OC"

#Run BAT_overview
#-i: Input bedGraph from BAT_summarize
#-o: Output prefix
#--groups: comma-separated list of group identifiers

BAT_overview.R  \
-i ${SUMMARIZE}/5_OC_S/S_summary_HY_OC.bedgraph \
-o ${OVERVIEW}/5_OC_S \
--groups HY,OC
```

I then ran the [`BAT_analysis`]() script. Of course, I encountered several errors. Mainly typos, but one that actually required troubleshooting:

> less /vortexfs1/scratch/yaamini.venkataraman/05-analysis/yrv_analysis753159.log
Oxygen within NB: Hypoxia vs. OC
[INFO]  Tue May 21, 13:39:0, 2024       BAT_summarize v0.1 started
[INFO]  Tue May 21, 13:39:0, 2024       Checking flags
[INFO]  Tue May 21, 13:39:0, 2024       Calculation of summary files
Input error, failed to extract end value from 'MU-UCD_Fhet_4.1,' (column 3) in /yaaminiv/killifish-hypoxia-RRBS/output/04-calling/new-genome/filtered/190626_I114_FCH7TVNBBXY_L3_OC-N2_CG.sort.bedgraph line 1
[INFO]  Tue May 21, 13:39:0, 2024       Writing bedgraph files and converting to bw

Since `BAT_summarize` failed, there was no input for `BAT_overview`. Seems like there was some sort of issue with the bedgraphs. I picked one file and looked at the `head` and `tail`.

> (base) [yaamini.venkataraman@poseidon-l1 filtered]$ head 190626_I114_FCH7TVNBBXY_L3_5-N3_CG.bedgraph
NC_046361.1 Fundulus heteroclitus isolate FHET01 chromosome 1, MU-UCD_Fhet_4.1, whole genome shotgun sequence	649	650	1.00
NC_046361.1 Fundulus heteroclitus isolate FHET01 chromosome 1, MU-UCD_Fhet_4.1, whole genome shotgun sequence	664	665	1.00
NC_046361.1 Fundulus heteroclitus isolate FHET01 chromosome 1, MU-UCD_Fhet_4.1, whole genome shotgun sequence	904	905	0.33
NC_046361.1 Fundulus heteroclitus isolate FHET01 chromosome 1, MU-UCD_Fhet_4.1, whole genome shotgun sequence	923	924	0.75
NC_046361.1 Fundulus heteroclitus isolate FHET01 chromosome 1, MU-UCD_Fhet_4.1, whole genome shotgun sequence	930	931	1.00
NC_046361.1 Fundulus heteroclitus isolate FHET01 chromosome 1, MU-UCD_Fhet_4.1, whole genome shotgun sequence	30793080	0.75
NC_046361.1 Fundulus heteroclitus isolate FHET01 chromosome 1, MU-UCD_Fhet_4.1, whole genome shotgun sequence	30803081	0.92
NC_046361.1 Fundulus heteroclitus isolate FHET01 chromosome 1, MU-UCD_Fhet_4.1, whole genome shotgun sequence	30873088	0.87
NC_046361.1 Fundulus heteroclitus isolate FHET01 chromosome 1, MU-UCD_Fhet_4.1, whole genome shotgun sequence	30883089	0.92
NC_046361.1 Fundulus heteroclitus isolate FHET01 chromosome 1, MU-UCD_Fhet_4.1, whole genome shotgun sequence	31143115	0.28

>(base) [yaamini.venkataraman@poseidon-l1 filtered]$ tail 190626_I114_FCH7TVNBBXY_L3_5-N3_CG.bedgraph
NC_012312.1 Fundulus heteroclitus mitochondrion, complete genome	15546	15547	0.00
NC_012312.1 Fundulus heteroclitus mitochondrion, complete genome	15547	15548	0.00
NC_012312.1 Fundulus heteroclitus mitochondrion, complete genome	15662	15663	0.00
NC_012312.1 Fundulus heteroclitus mitochondrion, complete genome	15772	15773	0.00
NC_012312.1 Fundulus heteroclitus mitochondrion, complete genome	16418	16419	0.00
NC_012312.1 Fundulus heteroclitus mitochondrion, complete genome	16419	16420	0.00
NC_012312.1 Fundulus heteroclitus mitochondrion, complete genome	16449	16450	0.01
NC_012312.1 Fundulus heteroclitus mitochondrion, complete genome	16450	16451	0.06
NC_012312.1 Fundulus heteroclitus mitochondrion, complete genome	16482	16483	0.00
NC_012312.1 Fundulus heteroclitus mitochondrion, complete genome	16483	16484	0.00

Throughout the bedgraph, there were different numbers of columns due to some very unfortunate genome annotation. When I looked at my `BAT-analysis` script, I saw that I had previously wrote code to deal with this issue:

```
Sort bedGraphs
for f in *bedgraph
do
/vortexfs1/home/yaamini.venkataraman/bedtools2/bin/sortBed \
-i ${f} \
| awk '{print $1"\t"$5"\t"$6"\t"$7}' \
> $(basename ${f%.bedgraph}).sort.bedgraph
done
```

While this may have worked for the old genome, it certainly doesn't work for the new genome. What I really needed was the first column of every bedgraph (chromosome), and then the last three columns for CpG start, stop, and percent methylation. A [quick Stack Overflow search](https://stackoverflow.com/questions/47969708/extract-the-last-three-columns-from-a-text-file-with-awk) showed me how to grab the last three columns with `awk` (oddly specific). By using `NF` (number of fields) as a variable, I could tell `awk` to pull the last column (NF), penultimate column (NF-1), and third from the end (NF-2). Super handy!

```
for f in *bedgraph
do
/vortexfs1/home/yaamini.venkataraman/bedtools2/bin/sortBed \
-i ${f} | \
awk 'NF{print $1,$(NF-2),$(NF-1),$NF}'  OFS="\t" \
> $(basename ${f%.bedgraph}).sort.bedgraph
done
```

This solved my issues so I was actually able to get output. The `BAT_summarize` output can be found [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/05-analysis/new-genome/summarize) and the `BAT_overview` output can be found [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/05-analysis/new-genome/overview).

Looking at the `BAT_overview` output, I see some potentially higher levels of methylation when comparing the outside control with the hypoxia treatment for both populations. It's a good reminder to me that for each comparison, `BAT` pulls common CpG with a certain coverage threshold. So I'll need to do my own analysis on the side apart from every common CpG if I want to dig into the methylation landscape.

### `BAT_DMRcalling`

Now to the main event! I returned to [this script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/06-BAT-DMR.sh) and fixed some stuff in my [environmental variable file](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/06-DMR-envfile.txt). I added the hypoxia vs. outside control comparisons in [this script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/06-BAT-DMRcalling.sh):

```
echo "Oxygen within NB: Hypoxia vs. OC"

#Run BAT_DMRcalling

BAT_DMRcalling \
-q ${SUMMARIZE}/5_OC_N/N_metilene_HY_OC.txt  \
-o ${DMR}/5_OC_N \
-a HY \
-b OC

#Move files to a new directory

mkdir ${DMR}/5_OC_N
mv ${DMR}/5_OC_N_* ${DMR}/5_OC_N/.
mv ${DMR}/5_OC_N.* ${DMR}/5_OC_N/.

echo "Oxygen within SC: Hypoxia vs. OC"

#Run BAT_DMRcalling

BAT_DMRcalling \
-q ${SUMMARIZE}/5_OC_S/S_metilene_HY_OC.txt  \
-o ${DMR}/5_OC_S \
-a NO \
-b OC

#Move files to a new directory

mkdir ${DMR}/5_OC_S
mv ${DMR}/5_OC_S_* ${DMR}/5_OC_S/.
mv ${DMR}/5_OC_S.* ${DMR}/5_OC_S/.
```

It ran without a hitch!

**Table 1**. Number of DMR for each comparison.

| **Population** |  **Treatment**  | **Number of DMR** |
|:--------------:|:---------------:|:-----------------:|
|    NB vs. SC   |       All       |         0         |
|    NB vs. SC   | Outside Control |         0         |
|    NB vs. SC   |     Normoxia    |         0         |
|    NB vs. SC   |     Hypoxia     |         0         |
|   New Bedford  |    OC vs. NO    |         0         |
|   New Bedford  |    OC vs. HY    |         59        |
|  New Bedford   |    NO vs. HY    |         0         |
|  Scorton Creek |    OC vs. NO    |         0         |
|  Scorton Creek |    OC vs. HY    |         0         |
|  Scorton Creek |    NO vs. HY    |         0         |

So, things are getting interesting. The 0 DMR for all the other comparisons I expected, but it's interesting to see that my only comparison with DMR was the outside control vs. hypoxia for the New Bedford fish. The NBH fish are more resilient to toxicants, so it's not only interesting that they're mounting a methylation response to hypoxia, but that they are only mounting a response to the most extreme treatment. Perhaps this means that methylation response is yet another tool these fish have when responding to stressors? I'll need to dig into the genomic locations of these DMR to learn more.

### Going forward

1. Update methods and results
2. Characterize the general methylation landscape
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
