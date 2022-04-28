---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 9
tags: killifish RRBS poseidon BAT
---

## General methylation landscape comparisons

The next step in the workflow is the [analysis module](https://www.bioinf.uni-leipzig.de/Software/BAT/analysis/#groups). It has three steps: summarize methylation information for each treatment (similar to `methylKit::unite`), produce an overview of methylome differences, and look at methylome differences at specific annotated elements.

### Defining contrasts

The first thing I wanted to do was test `BAT_summarize` for my samples. Immediately I ran into an issue. Similar to `methylKit`, `BAT_summarize` only allows for comparisons between two groups. Based on that, I came up with a list of contrasts to test:

- New Bedford vs. Scorton Creek, irrespsective of oyxgen conditions: This will provide me population-specific methylation differences. I hypothesize that many of these differences would be related to genetic differences between populations, as tolerant populations can have vastly different genome structures in certain pathways.
- Normoxia vs. Hypoxia, irrespective of population: This will provide me oxygen-induced methylation differences. I can remove any population-specific methylome differences from this list to get purely environmentally-influenced positions
- Normoxia vs. Hypoxia within each population: If there are vast differences in methylation between populations (should be able to visualize with a PCA), then that would justify examining each population separately. However, I don't know if I have the sample size for this
- Normoxia vs. Outside Control, irrespective of population: This contrast will give me captivity-related methylation differences I should remove from consideration for any DMR

I want to discuss these contrasts with Neel before finalizing my plan, but I decided to test `BAT_summarize` for the two populations since I know I'll need that information no matter what.

### Testing `BAT_summarize`

I created [this script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-analysis.sh) and [this variable file](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-analysis-envfile.txt) for the analysis module and started drafting the code for `BAT_summarize` based on the [handbook](https://www.bioinf.uni-leipzig.de/Software/BAT/analysis/#bat_summarize) and [examples](https://www.bioinf.uni-leipzig.de/Software/BAT/example_analysis/). I created a `singularity` container in an interactive mode to test the following code:

```
#Filtered output directory
FILTERED=/yaaminiv/killifish-hypoxia-RRBS/output/04-calling/filtered

#All population output directory
ALL_POP=/scratch/05-analysis/summarize/all_pop

BAT_summarize \
--in1 ${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-N4_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-N1_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-N2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_20-N2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_5-N3_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_20-N1_CG.bedgraph \
--in2 ${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S1_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S3_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S4_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-S3_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-S4_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_5-S2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_20-S2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_5-S1_CG.bedgraph \
--groups N,S \
--h1 20-N4,5-N1,5-N2,20-N2,5-N3,20-N1 \
--h2 20-S1,20-S3,20-S4,5-S3,5-S4,5-S2,20-S2,5-S1 \
--out ${ALL_POP}
```

This code produced an error that I was missing a required argument. Looking back at the handbook and some reference code Neel sent me, I discerned that I needed a file of chromosome lengths for this command. Based on [code I wrote for the *C. gigas* genome](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/08-Generating-Genome-Feature-Tracks.ipynb), I unzipped the Mummichog genome, then created a file with chromosome lengths:

```
(base) [yaamini.venkataraman@pn118 Killifish]$ gzip -d Fundulus_heteroclitus.Fundulus_heteroclitus-3.0.2.dna.toplevel.fa.gz
(base) [yaamini.venkataraman@pn118 Killifish]$ head Fundulus_heteroclitus.Fundulus_heteroclitus-3.0.2.dna.toplevel.fa
==> Fundulus_heteroclitus.Fundulus_heteroclitus-3.0.2.dna.toplevel.fa <==
>KN805525.1 dna:primary_assembly primary_assembly:Fundulus_heteroclitus-3.0.2:KN805525.1:1:6356336:1 REF
ATGGTGGGGACGGTTCTTTGCAAACCACAGAATGTGAGATTCTTACAGATTCTCAACCGT
CAAAGCTTGTGATTGGCTAAGATAAAATTAAACACTTATTTCCCATAAACACTTATTTCC
CATAAATGTCAAGAGATTATGATTGTCATGGCACTCTCATGTGATCTTGCCGTGCCATAC
CAAAGTTACCCCACCATAAAACAAAGTCCAGTGCCAATTTTTGTGCATAAAGCAAGACAG
TTTTTTTTTTCGTTTTTTTTTATTTCAAGATAAAGTTAGTCTTCAAACAGAGCAAAGAAA
GAAAAAGTTAAGGAAAAGTCATCTGAATGACATTTTGTTTCAGTTTGGTGATATATAATG
GCCCCAGCGTGACCTCACATTAGGCTTTGTAATAGATTTTATCCTGCAATGTGGTGTTGA
TACATTTTTAATAAAAGTCACGAAGCTAATTTCTGAGAAATGTTACAGAAGAATCAGGCA
TAACCAACGCCAACTTCTATGCTGCGCCACCTTTTTTTAAGGTACAGCAACACATCTCTT
```

```
awk '$0 ~ ">" {print c; c=0;printf substr($0,2,14) "\t"; } $0 !~ ">" {c+=length($0);} END { print c; }' \
Fundulus_heteroclitus.Fundulus_heteroclitus-3.0.2.dna.toplevel.fa \
| sed 's/dna//g' \
| awk '{print $1"\t"$2}' \
| tail -n +2 \
> mummichog.chrom.length
```

I then ran the following code:

```
BAT_summarize \
--in1 ${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-N4_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-N1_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-N2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_20-N2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_5-N3_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_20-N1_CG.bedgraph \
--in2 ${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S1_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S3_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S4_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-S3_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-S4_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_5-S2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_20-S2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_5-S1_CG.bedgraph \
--groups N,S \
--h1 20-N4,5-N1,5-N2,20-N2,5-N3,20-N1 \
--h2 20-S1,20-S3,20-S4,5-S3,5-S4,5-S2,20-S2,5-S1 \
--out ${ALL_POP} \
--cs ${CHROM_LENGTH}
```

While I no longer got an error about missing a required argument, I did get this fun new error!

```
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_mean_N.bedgraph to /scratch/05-analysis/summarize/all_pop_mean_N.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_mean_S.bedgraph to /scratch/05-analysis/summarize/all_pop_mean_S.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_diff_N_S.bedgraph to /scratch/05-analysis/summarize/all_pop_diff_N_S.bw
[INFO]	Wed Apr 27, 12:40:0, 2022	Writing bedgraph files and converting to bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_20-N4.bedgraph to /scratch/05-analysis/summarize/all_pop_20-N4.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_5-N1.bedgraph to /scratch/05-analysis/summarize/all_pop_5-N1.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_5-N2.bedgraph to /scratch/05-analysis/summarize/all_pop_5-N2.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_20-N2.bedgraph to /scratch/05-analysis/summarize/all_pop_20-N2.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_5-N3.bedgraph to /scratch/05-analysis/summarize/all_pop_5-N3.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_20-N1.bedgraph to /scratch/05-analysis/summarize/all_pop_20-N1.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_20-S1.bedgraph to /scratch/05-analysis/summarize/all_pop_20-S1.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_20-S3.bedgraph to /scratch/05-analysis/summarize/all_pop_20-S3.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_20-S4.bedgraph to /scratch/05-analysis/summarize/all_pop_20-S4.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_5-S3.bedgraph to /scratch/05-analysis/summarize/all_pop_5-S3.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_5-S4.bedgraph to /scratch/05-analysis/summarize/all_pop_5-S4.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_5-S2.bedgraph to /scratch/05-analysis/summarize/all_pop_5-S2.bw
invalid unsigned integer: ":primary"
##### AN ERROR has occurred: Could not convert /scratch/05-analysis/summarize/all_pop_20-S2.bedgraph to /scratch/05-analysis/summarize/all_pop_20-S2.bw
invalid unsigned integer: ":primary"
```

However, it is producing the bedGraph output. I'm not sure what the `.bw` files are necessary for especially if I can use IGV for visualization...maybe for the circos plot I'm not creating? In any case, I followed the instructions on [this page](https://anaconda.org/bioconda/ucsc-bedgraphtobigwig) to install `bedgraphtobigWig`, which is necessary for converting files:

```
(base) [yaamini.venkataraman@poseidon-l1 ~]$ conda install -c bioconda ucsc-bedgraphtobigwig

Collecting package metadata (current_repodata.json): done
Solving environment: done

## Package Plan ##

  environment location: /vortexfs1/home/yaamini.venkataraman/miniconda3

  added / updated specs:
    - ucsc-bedgraphtobigwig


The following packages will be downloaded:

    package                    |            build
    ---------------------------|-----------------
    certifi-2021.10.8          |   py39hf3d152e_2         145 KB  conda-forge
    conda-4.12.0               |   py39hf3d152e_0        1014 KB  conda-forge
    mysql-connector-c-6.1.11   |    h6eb9d5d_1007         2.7 MB  conda-forge
    openssl-1.1.1n             |       h166bdaf_0         2.1 MB  conda-forge
    ucsc-bedgraphtobigwig-377  |       h0b8a92a_2         155 KB  bioconda
    ------------------------------------------------------------
                                           Total:         6.1 MB

The following NEW packages will be INSTALLED:

  mysql-connector-c  conda-forge/linux-64::mysql-connector-c-6.1.11-h6eb9d5d_1007
  ucsc-bedgraphtobi~ bioconda/linux-64::ucsc-bedgraphtobigwig-377-h0b8a92a_2

The following packages will be UPDATED:

  certifi                          2021.10.8-py39hf3d152e_1 --> 2021.10.8-py39hf3d152e_2
  conda                               4.11.0-py39hf3d152e_0 --> 4.12.0-py39hf3d152e_0
  openssl                                 1.1.1l-h7f98852_0 --> 1.1.1n-h166bdaf_0


Proceed ([y]/n)?

Downloading and Extracting Packages
certifi-2021.10.8    | 145 KB    | ##################################### | 100%
conda-4.12.0         | 1014 KB   | ##################################### | 100%
openssl-1.1.1n       | 2.1 MB    | ##################################### | 100%
ucsc-bedgraphtobigwi | 155 KB    | ##################################### | 100%
mysql-connector-c-6. | 2.7 MB    | ##################################### | 100%
Preparing transaction: done
Verifying transaction: done
Executing transaction: done

(base) [yaamini.venkataraman@poseidon-l1 ~]$ which bedGraphToBigWig
~/miniconda3/bin/bedGraphToBigWig
```

Since `bedGraphToBigWig` is in my home directory, I can specify it in my `BAT_summarize` command:

```
BAT_summarize \
--in1 ${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-N4_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-N1_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-N2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_20-N2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_5-N3_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_20-N1_CG.bedgraph \
--in2 ${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S1_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S3_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S4_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-S3_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-S4_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_5-S2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_20-S2_CG.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_5-S1_CG.bedgraph \
--groups N,S \
--h1 20-N4,5-N1,5-N2,20-N2,5-N3,20-N1 \
--h2 20-S1,20-S3,20-S4,5-S3,5-S4,5-S2,20-S2,5-S1 \
--out ${ALL_POP} \
--cs ${CHROM_LENGTH} \
-bgbw /yaaminiv/miniconda3/bin/bedGraphToBigWig
```

Unfortunately I still got the error :/ When reviewing the script Neel sent me again, I noticed that bedGraphs were sorted before running `BAT_summarize`. Maybe this could be leading to the error about an invalid unsigned integer? I decided to sort the bedGraphs:

```
for f in *bedgraph
do
/vortexfs1/home/yaamini.venkataraman/bedtools2/bin/sortBed \
-i ${f} \
> $(basename ${f%.bedgraph}).sort.bedgraph
done
```

I then used the sorted bedGraphs in the command:

```
BAT_summarize \
--in1 ${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-N4_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-N1_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-N2_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_20-N2_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_5-N3_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_20-N1_CG.sort.bedgraph \
--in2 ${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S1_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S3_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_20-S4_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-S3_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L2_5-S4_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L3_5-S2_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_20-S2_CG.sort.bedgraph,${FILTERED}/190626_I114_FCH7TVNBBXY_L4_5-S1_CG.sort.bedgraph \
--groups N,S \
--h1 20-N4,5-N1,5-N2,20-N2,5-N3,20-N1 \
--h2 20-S1,20-S3,20-S4,5-S3,5-S4,5-S2,20-S2,5-S1 \
--out ${ALL_POP} \
--cs ${CHROM_LENGTH} \
-bgbw /yaaminiv/miniconda3/bin/bedGraphToBigWig
```

STILL GETTING THE CONVERSION ERROR. I officially give up and will ask Neel if it's important to get those files or not. In any case, it's probably good to use the sorted bedGraphs as input to `BAT_summarize`.

### Testing `BAT_overview`

I don't need the BigWig files for the next steps, so I moved on to `BAT_overview`:

```
Singularity> BAT_overview.R  \
> -i ${ALL_POP}_summary_N_S.bedgraph \
> -o ${ALL_POP} \
> --groups N,S
[INFO]	Thu Apr 28, 04:34:53 PM, 2022	BAT_overview v0.1 started
[INFO]	Thu Apr 28, 04:34:53 PM, 2022	Checking flags
[INFO]	Thu Apr 28, 04:34:53 PM, 2022	Reading input /scratch/05-analysis/summarize/all_pop_summary_N_S.bedgraph
Error in read.table(file = opt$input.filename, header = TRUE, stringsAsFactors = FALSE,  :
  more columns than column names
Calls: main -> read.table
Execution halted
```

R is unable to correctly parse the number of columns in the bedGraph. When I counted the number of columns in the file, it looks like there are empty columns in the file that do not have headers:

```
Singularity> awk '{print NF;exit}' ${ALL_POP}_summary_N_S.bedgraph
17
Singularity> awk '{print NF}' ${ALL_POP}_summary_N_S.bedgraph
17
20
20
```

I used `awk` to remove the last three columns from the file:

```
awk '{print $1"\t"$2"\t"$3"\t"$4"\t"$5"\t"$6"\t"$7"\t"$8"\t"$9"\t"$10"\t"$11"\t"$12"\t"$13"\t"$14"\t"$15"\t"$16"\t"$17}' ${ALL_POP}_summary_N_S.bedgraph >  ${ALL_POP}_summary_N_S.bedgraph.fix
```

Then I reran the command:

```
Singularity> BAT_overview.R  \
> -i ${ALL_POP}_summary_N_S.bedgraph.fix \
> -o ${ALL_POP} \
> --groups N,S
[INFO]	Thu Apr 28, 04:56:54 PM, 2022	BAT_overview v0.1 started
[INFO]	Thu Apr 28, 04:56:54 PM, 2022	Checking flags
[INFO]	Thu Apr 28, 04:56:54 PM, 2022	Reading input /scratch/05-analysis/summarize/all_pop_summary_N_S.bedgraph.fix
Error in rowMeans(data[, opt$g1], na.rm = TRUE) : 'x' must be numeric
Calls: main -> rowMeans
```

I must admit that at this point it's pretty frustrating to not be able to engage with the R script interactively. Anyways, I needed to figure out an `as.numeric` equivalent for the columns. Instead of reprinting the columns, what if I just removed certain columns from the original file?

```
cut -f1-17 ${ALL_POP}_summary_N_S.bedgraph > ${ALL_POP}_summary_N_S.bedgraph.fix
```

Turns out that only gives me the same error I had before about extra columns! So `awk` was the way to go, which still brings me back to my numeric data issue. Honestly it's weird that an output file produced with the BAT workflow is producing these errors...so I emailed the BAT creators. Between them and Neel hopefully I can figure out what the issue is.

### Going forward

1. Ask Neel about differences between L4 and L2/L3
3. Figure out `bedGraphToBigWig` error
4. Figure out `BAT_overview` error (add line of code to make all column data numerical to BAT_overview.R?)
2. Confirm contrasts with Neel
2. Run `BAT_analysis`

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
