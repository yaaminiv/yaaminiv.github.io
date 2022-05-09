---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 11
tags: killifish RRBS poseidon BAT
---

## Figuring out my `BAT_overview` issues

Since I had [`BAT_summarize` output for all contrasts](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part10/), my next step is to identify DMR. I'm using the default settings for [`BAT_DMRcalling`](https://www.bioinf.uni-leipzig.de/Software/BAT/dmrs/#bat_dmrcalling):

- Report DMR with an adjusted p-value (q-value) less than 0.05
- At least 10 Cs per DMRs
- Minimum difference of 0.1 in mean methylation rate
- No minium nucleotide length

### Source of issues

I ran the following code interactively to identify DMR between the two populations irrespective of oxygen condition:

```
BAT_DMRcalling \
-q /yaaminiv/killifish-hypoxia-RRBS/output/05-analysis/summarize/all_pop/all_pop_metilene_N_S.txt  \
-o /scratch/06-DMR/all_pop \
-a N \
-b S
```

When I looked at the output, I had 0 DMR! This seemed ridiculous, so I did some digging into the input file. Similar to the output bedGraph from `BAT_summarize`, there were three extra columns in data rows that weren't present in the header row. I decided to create an output file that didn't have these extra columns, similar to what I tried for the bedGraphs. For some reason my `awk` code wasn't running and I tried pulling out subsets of columns. Thank goodness, because I realized what the real issue was:

```
(base) [yaamini.venkataraman@pn035 all_pop]$ awk '{print $1"\t"$2"\t"$3"\t"$4"\t"$5}' all_pop_metilene_N_S.txt | head
chr	pos	N_20-N4	N_5-N1	N_5-N2
KN805535.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:KN805535.1:1:799646:1	REF	414192
KN805610.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:KN805610.1:1:629617:1	REF	285262
KN805610.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:KN805610.1:1:629617:1	REF	285275
KN805610.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:KN805610.1:1:629617:1	REF	285278
KN805697.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:KN805697.1:1:580679:1	REF	375041
KN805750.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:KN805750.1:1:549272:1	REF	122053
KN805750.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:KN805750.1:1:549272:1	REF	122065
KN805750.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:KN805750.1:1:549272:1	REF	122071
KN805750.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:KN805750.1:1:549272:1	REF	122078
```

My chromosome information was being broken up into four columns, which is why I had three additional columns in my `BAT_summarize` output files! I tracked this down to the output of my filtered files:

```
(base) [yaamini.venkataraman@pn035 filtered]$ awk '{print $1"\t"$2"\t"$3"\t"$4"\t"$5}' 190626_I114_FCH7TVNBBXY_L2_5-S3_CG.sort.bedgraph | head
JXMV01047516.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1	REF	1872
JXMV01047516.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1	REF	2221
JXMV01047516.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1	REF	3092
JXMV01047516.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1	REF	6204
JXMV01047516.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1	REF	14978
JXMV01047516.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1	REF	14999
JXMV01047516.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1	REF	15005
JXMV01047516.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1	REF	15034
JXMV01047516.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1	REF	15039
JXMV01047516.1	dna:primary_assembly	primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1	REF	15044

(base) [yaamini.venkataraman@pn035 filtered]$ head 190626_I114_FCH7TVNBBXY_L2_5-S3_CG.sort.bedgraph
JXMV01047516.1 dna:primary_assembly primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1 REF	1872	1873	1.00
JXMV01047516.1 dna:primary_assembly primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1 REF	2221	2222	0.15
JXMV01047516.1 dna:primary_assembly primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1 REF	3092	3093	1.00
JXMV01047516.1 dna:primary_assembly primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1 REF	6204	6205	1.00
JXMV01047516.1 dna:primary_assembly primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1 REF	14978	14979	1.00
JXMV01047516.1 dna:primary_assembly primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1 REF	14999	15000	1.00
JXMV01047516.1 dna:primary_assembly primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1 REF	15005	15006	1.00
JXMV01047516.1 dna:primary_assembly primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1 REF	15034	15035	1.00
JXMV01047516.1 dna:primary_assembly primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1 REF	15039	15040	1.00
JXMV01047516.1 dna:primary_assembly primary_assembly:Fundulus_heteroclitus-3.0.2:JXMV01047516.1:1:31188:1 REF	15044	15045	1.00
```

Looks like I needed to remove the second, third, and fourth columns. It was at this point I missed working in a Jupyter notebook because it's much easier to see that kind of stuff in chunks instead of in the Terminal output. BUT I DIGRESS. I added a line of code to my bedGraph sorting command and tested it on one file:

```
(base) [yaamini.venkataraman@pn035 filtered]$ /vortexfs1/home/yaamini.venkataraman/bedtools2/bin/sortBed \
> -i 190626_I114_FCH7TVNBBXY_L2_5-S3_CG.bedgraph \
> | awk '{print $1"\t"$5"\t"$6"\t"$7}' \
> | head
JXMV01047516.1	1872	1873	1.00
JXMV01047516.1	2221	2222	0.15
JXMV01047516.1	3092	3093	1.00
JXMV01047516.1	6204	6205	1.00
JXMV01047516.1	14978	14979	1.00
JXMV01047516.1	14999	15000	1.00
JXMV01047516.1	15005	15006	1.00
JXMV01047516.1	15034	15035	1.00
JXMV01047516.1	15039	15040	1.00
JXMV01047516.1	15044	15045	1.00
```

The output looked in-line with what the `BAT_filtering` output looked like in the example, so that's a good sign! I remade my sorted bedGraphs with this change.

```
#Sort bedGraphs
#Keep columns 1, 5, 6, 7 (remove extra chromosome columns)
for f in *bedgraph
do
/vortexfs1/home/yaamini.venkataraman/bedtools2/bin/sortBed \
-i ${f} \
| awk '{print $1"\t"$5"\t"$6"\t"$7}' \
> $(basename ${f%.bedgraph}).sort.bedgraph
done
```

```
(base) [yaamini.venkataraman@poseidon-l2 filtered]$ head 190626_I114_FCH7TVNBBXY_L3_OC-N4_CG.sort.bedgraph
JXMV01047516.1	356	357	1.00
JXMV01047516.1	371	372	1.00
JXMV01047516.1	382	383	1.00
JXMV01047516.1	6012	6013	1.00
JXMV01047516.1	15106	15107	1.00
JXMV01047516.1	15109	15110	1.00
JXMV01047516.1	15241	15242	1.00
JXMV01047516.1	19994	19995	1.00
JXMV01047516.1	19995	19996	1.00
JXMV01047516.1	20022	20023	1.00
```

The revised sorted bedGraphs can be found in [this directory](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/04-calling/filtered).

### Running `BAT_summarize` (again)

The next step was to run `BAT_summarize` to see if summary bedGraph and text files would play well with `BAT_overview.R` and `BAT_DMRcalling`. I simply ran [my script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-analysis.sh) again and checked the number of columns in the output files:

```
(base) [yaamini.venkataraman@poseidon-l2 5_pop_diff]$ awk '{print NF}' *summary*bedgraph | head
10
10
10
10
10
10
10
10
10
10
```

There is no difference in the number of columns between the header and data rows! The output files can be found in [this directory](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/05-analysis/summarize).

### Trying `BAT_overview` (again)

Moment of truth! I wanted to use one of the output bedGraphs to try `BAT_overview`. If I end up with the same errors, then there is an another issue I am not aware of yet.

```
Singularity> BAT_overview.R  \
> -i /scratch/05-analysis/summarize/all_pop/all_pop_summary_N_S.bedgraph \
> -o /scratch/05-analysis/overview/all_pop \
> --groups N,S
[INFO]	Mon May 09, 11:37:42 AM, 2022	BAT_overview v0.1 started
[INFO]	Mon May 09, 11:37:42 AM, 2022	Checking flags
[INFO]	Mon May 09, 11:37:42 AM, 2022	Reading input /scratch/05-analysis/summarize/all_pop/all_pop_summary_N_S.bedgraph
[INFO]	Mon May 09, 11:37:42 AM, 2022	Plot statistics to /scratch/05-analysis/overview/all_pop.pdf
```

IT WORKS. I drafted a [new script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-BAT-overview.sh) for `BAT_overview` and updated the [environmental variable information](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-analysis-envfile.txt). I then ran [my SLURM script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-analysis.sh). Half of the contrasts produced output, while the other half didn't:

```
Oxygen within NB: Normoxia vs. Hypoxia
[INFO]  Mon May 09, 12:04:56 PM, 2022   BAT_overview v0.1 started
[INFO]  Mon May 09, 12:04:56 PM, 2022   Checking flags
[INFO]  Mon May 09, 12:04:56 PM, 2022   Reading input /scratch/05-analysis/summarize/20_5_N/N_summary_20_5.bedgraph
Error in main() :
  Required group identfier for group 1 not found in input.
Execution halted
Oxygen within NB: Normoxia vs. OC
[INFO]  Mon May 09, 12:04:56 PM, 2022   BAT_overview v0.1 started
[INFO]  Mon May 09, 12:04:56 PM, 2022   Checking flags
[INFO]  Mon May 09, 12:04:56 PM, 2022   Reading input /scratch/05-analysis/summarize/20_OC_N/N_summary_20_OC.bedgraph
Error in main() :
  Required group identfier for group 1 not found in input.
Execution halted
Oxygen within SC: Normoxia vs. Hypoxia
[INFO]  Mon May 09, 12:04:56 PM, 2022   BAT_overview v0.1 started
[INFO]  Mon May 09, 12:04:56 PM, 2022   Checking flags
[INFO]  Mon May 09, 12:04:56 PM, 2022   Reading input /scratch/05-analysis/summarize/20_5_S/S_summary_20_5.bedgraph
Error in main() :
  Required group identfier for group 1 not found in input.
Execution halted
Oxygen within SC: Normoxia vs. OC
[INFO]  Mon May 09, 12:04:57 PM, 2022   BAT_overview v0.1 started
[INFO]  Mon May 09, 12:04:57 PM, 2022   Checking flags
[INFO]  Mon May 09, 12:04:57 PM, 2022   Reading input /scratch/05-analysis/summarize/20_OC_S/S_summary_20_OC.bedgraph
Error in main() :
  Required group identfier for group 1 not found in input.
Execution halted
```

It seems like "20" may not be an appropriate group identifier? I went back to [my `BAT_summarize` code](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-BAT-summarize.sh) and used "NO" and "HY" to specify normoxia and hypoxia, respectively. I made the same change in [my `BAT_overview` code](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-BAT-overview.sh). I reran `BAT_summarize` and `BAT_overview` for these contrasts and it worked! The `BAT_overview` output can be found in [this directory](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/05-analysis/overview).

### `BAT_overview` results

I looked through the pdfs produced by `BAT_overview ` and there were some things I noticed:

1. The hierarchical clustering didn't perfectly distinguish between the groups I was comparing (population or oxygen condition). This isn't a big problem when comparing normoxia vs. outside control, but strange when comparing the two populations or normoxia vs. hypoxia (Note: clustering was done using a euclidean distance matrix and Ward method).
2. In OC and normoxia, NBH had more variable methylation rates when compared to SC, but less variable methylation in hypoxia.
3. Both populations had higher methylation in response to hypoxia.

Questions I now have:

1. Should I use the dendograms to identify outlier samples?
2. What distribution of methylation should I expect?

### Going forward

1. Identify DMR
2. Write methods and results
4. Obtain other important methylation landscape information
5. Start RNA-Seq analysis
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
