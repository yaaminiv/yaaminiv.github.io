---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 21
tags: killifish RRBS poseidon BAT RNA-Seq
---

## Continuing with the new genome

I'm going through my previous code, update paths to the files for the new genome, and evaluating the output to eventually compare my results with those from the old genome!

### RNA-Seq results

But first...I looked through the RNA-Seq results Neel generated. Overall, SC fish had a much larger response to hypoxia. There were more DEG between SC fish exposed to hypoxia and normoxia versus their NBH counterparts. When I perused the list of annotations associated with DEG for SC, I saw classical hypoxia response genes such as HIFs and cytochrome P450 subfamily genes. In SC fish, several DEG were involved in the AHR signaling pathway that underwent evolution after exposure to persistent organic pollutants such as HSP 90s, CYP1As, AND TIPARP. None of these genes were differentially expressed for NBH. None of the [genes closest to DMR](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part19/) were differentially expressed.

### `BAT_mapping` and `BAT_mapping_stat`

I created [this spreadsheet](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/03-mapping/new-genome/stat/new-genome-mapping-statistics.csv) to evaluate alignment to the new genome. Mapping efficiency was still quite high, ranging between 88% to 94%. There doesn't seem to be any difference in mapping rate by treatment or library.

### `BAT_calling`

As expected, the L4 libraries still had a weird coverage pattern. All output can be found [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/04-calling/new-genome).

**Figures 1-2**. Coverage for filtered CpGs in L3 and L4 samples

<img width="686" alt="Screen Shot 2022-09-19 at 3 05 00 AM" src="https://user-images.githubusercontent.com/22335838/190966292-882b12bd-ffda-4396-8e1d-cad0d3f97bf1.png">

<img width="686" alt="Screen Shot 2022-09-19 at 3 05 04 AM" src="https://user-images.githubusercontent.com/22335838/190966294-df6972b6-25c2-4489-8c87-29d36abf4de6.png">

### `BAT_summarize`

Prior to analysis, I needed to clean up and sort the filtered bedgraphs. I ran the following code to do so:

```
for f in *bedgraph
do
/vortexfs1/home/yaamini.venkataraman/bedtools2/bin/sortBed \
-i ${f} \
| awk '{print $1"\t"$5"\t"$6"\t"$7}' \
> $(basename ${f%.bedgraph}).sort.bedgraph
done
head *sort.bedgraph
```

When I ran [my `BAT_summarize` script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-BAT-summarize.sh), I got the following error:

```
Input error, failed to extract start value from 'complete' (column 2) in /yaaminiv/killifish-hypoxia-RRBS/output/04-calling/new-genome
/filtered/190626_I114_FCH7TVNBBXY_L2_20-N4_CG.sort.bedgraph line 1
needLargeMem: trying to allocate 0 bytes (limit: 100000000000)
```

I saw the following when I looked at the head of a sorted bedgraph:

```
==> 190626_I114_FCH7TVNBBXY_L4_OC-S5_CG.sort.bedgraph <==
NC_012312.1	complete	genome	296
NC_012312.1	complete	genome	970
NC_012312.1	complete	genome	990
NC_012312.1	complete	genome	1017
NC_012312.1	complete	genome	1061
NC_012312.1	complete	genome	1071
NC_012312.1	complete	genome	1075
NC_012312.1	complete	genome	1095
NC_012312.1	complete	genome	1208
NC_012312.1	complete	genome	1401
```

I realized with the new genome that I likely had different columns I needed to remove! I modified the code as such:

```
for f in *bedgraph
do
/vortexfs1/home/yaamini.venkataraman/bedtools2/bin/sortBed \
-i ${f} \
| awk '{print $1"\t"$7"\t"$8"\t"$9}' \
> $(basename ${f%.bedgraph}).sort.bedgraph
done
head *sort.bedgraph
```

```
#Resulting bedgraph
==> 190626_I114_FCH7TVNBBXY_L4_OC-S5_CG.sort.bedgraph <==
NC_012312.1	296	297	0.03
NC_012312.1	970	971	0.00
NC_012312.1	990	991	0.03
NC_012312.1	1017	1018	0.01
NC_012312.1	1061	1062	0.04
NC_012312.1	1071	1072	0.01
NC_012312.1	1075	1076	0.00
NC_012312.1	1095	1096	0.00
NC_012312.1	1208	1209	0.00
NC_012312.1	1401	1402	0.00
```

I also noticed that the files from SC 20 vs. OC contrast did not get moved to the proper folder because the correct output directory was not defined. I replaced `mv /scratch/05-analysis/summarize/S_* ${SUMMARIZE}/20_OC_S/.` with `mv ${SUMMARIZE}/S_* ${SUMMARIZE}/20_OC_S/.` so the files would be moved to the proper directory.

I reran the analysis successfully! All output can be found [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/05-analysis/new-genome/summarize).

### `BAT_overview`

The summary pdfs can be found [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/05-analysis/new-genome/overview). Interestingly, mapping to the new genome leads to almost zero levels of methylation across all samples and treatments:

**Figure 3**. Low methylation rate identified in new genome mapping (left) versus higher methylation rate identified in old genome mapping (right).

![Screen Shot 2022-09-19 at 1 38 37 PM](https://user-images.githubusercontent.com/22335838/191079958-7a53b312-18bc-484c-82db-eff520a8d430.png)

I don't know why mapping to the new genome caused such large shifts in methylation. Maybe only unmethylated positions have coverage across all samples?

### Going forward

1. Figure out why there is such low methylation with the new genome
2. Continue BAT workflow with new genome
1. Revise methylation landscape information
2. Dig into differences in general methylation by population and treatment
3. Match DMR with RNA-Seq information
4. Conduct pathway analysis for RNA-Seq data by population
5. Identify known SNP/DMR overlaps
1. Update methods and results
6. Create OSF repository for all intermediate files

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
