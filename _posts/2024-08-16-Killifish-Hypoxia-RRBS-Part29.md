---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 29
tags: killifish RRBS
---

## `BAT_correlating`

This is has the potential to be very exciting or a huge letdown.

With BAT, you can provide methylation and gene expression data to [`BAT_correlating`](http://legacy.bioinf.uni-leipzig.de/Software/BAT/dmrs/#bat_correlating) to get correlation information for the mean methylation rate of the DMR and expression of the associated gene. Even though this is part of the BAT DMR module, I decided to set up a new set of scripts for the analysis, since I had to run a couple of analyses in between to get to this point:

- [singularity code](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/08-correlating.sh)
- [envfile](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/08-correlating-envfile.txt)
- [BAT_correlating code](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/08-BAT-correlating.sh)

I needed the following information:

> -b	BED file containing coordinates of region (for overlapping with methylation bigWig file) and name of associated gene identifier (to connect to expression file). Format: chr <tab> start <tab> end <tab> identifier. Identifier need to match gene identifiers of the expression files (option -e).
> -e	File containing a list of expression files (one per sample). The content of each file must follow this format identifier <tab> something <tab> expression_value.
> -m	File containing a list of methylation bigWig files (one per sample).
> -g	File containing the association of sample identifiers to groups. Format: sample <tab> group. The list of samples in the file must be ordered analogously as in the list of files given with options -e and -m.
> -i	Comma-separated list of group 1 and group2 as well as an optional additional third group for plotting. Groups need to match the group names in the file provide with option -g.
> -o	Path/prefix for output (path for correlation plots and path/prefix.txt for statistics).

All of them seemed pretty straightforward to make. I got the gene expression data and a list of file mappings from Neel and proceeded to follow the example code to make input files:

```
#Create BAT_correlating input

mkdir ${CORR}/20_OC_N

echo "Create BEDfile with DMR and gene coordinates"

cat ${ANNOT}/20_OC_N_DMR.closestGene.bed \
| cut -f1-3,14 \
| tr ";" "\t" \
| cut -f1-4 \
| sed 's/ID=gene-//g' \
> ${CORR}/20_OC_N/20_OC_N_DMR_gene.bed

echo "Create methylation bigWig file list"

ls ${BIGWIG}/20_OC_N/*bw \
| grep -v "mean" \
| grep -v "diff" \
| sed 's/\t/\n/' \
> ${CORR}/20_OC_N/20_OC_N_methylation_files.list

echo "Create gene expression file list"

ls ${RNA}/*threeCol.txt \
| grep -e "20-N" -e "OC-N" \
| sed 's/\t/\n/' \
> ${CORR}/20_OC_N/20_OC_N_expression_files.list

echo "Create sample identifier list"

echo -e '20-N1\tNO\n20-N2\tNO\n20-N4\tNO\nOC-N1\tOC\nOC-N2\tOC\nOC-N3\tOC\nOC-N4\tOC\nOC-N5\tOC' > ${CORR}/20_OC_N/20_OC_N_sample_to_group.txt
```

A couple of notes about how I made the gene expression files. I got [this new metadata table](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/metadata/Fh_RRBS_RNAseq-sampledescription.xlsx) from Neel that maps the RNA-Seq and RRBS data files to eachother. The RNA-Seq and RRBS files have to have the same sample identifier. I renamed files and saved them in [this folder](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/data/RNA-Seq-renamed).

Initially I didn't modify the RNA-Seq data. Then when I was running my code I got the following error:

> DMR Correlating Module
Oxygen within NB: Outside Control vs. Hypoxia
Create BEDfile with DMR and gene coordinates
Create methylation bigWig file list
Create gene expression file list
Create sample identifier list
[INFO]  Fri Aug 16, 15:8:24, 2024       BAT_correlating v0.1 started
[INFO]  Fri Aug 16, 15:8:24, 2024       Checking input
[INFO]  Fri Aug 16, 15:8:24, 2024       Calculating average methylation
/bin/bash: /usr/bin/lesspipe.sh: No such file or directory
/bin/bash: /usr/bin/lesspipe.sh: No such file or directory
/bin/bash: /usr/bin/lesspipe.sh: No such file or directory
/bin/bash: /usr/bin/lesspipe.sh: No such file or directory
/bin/bash: /usr/bin/lesspipe.sh: No such file or directory
/bin/bash: /usr/bin/lesspipe.sh: No such file or directory
/bin/bash: /usr/bin/lesspipe.sh: No such file or directory
[INFO]  Fri Aug 16, 15:8:25, 2024       Getting expression values
[INFO]  Fri Aug 16, 15:8:25, 2024       Started plotting correlation plots
Error in scan(file = file, what = what, sep = sep, quote = quote, dec = dec,  :
  line 1 did not have 4 elements
Calls: main -> read.table -> scan
Execution halted
AN ERROR has occurred: problem with calling R
Done with DMR correlating

I reviewed the input file information and realized the RNA-Seq data needed three columns (`identifier <tab> something <tab> expression_value`), and that the second column didn't need to be anything informative! I used `awk` to modify the columns of each RNA-Seq file:

```
#Navigate to bedGraph directory
cd /vortexfs1/home/yaamini.venkataraman/killifish-hypoxia-RRBS/data/RNA-Seq-renamed

#Sort bedGraphs
for f in *txt
do
awk -F "\t" '{ print $1, $1, $2 }' ${f} \
> $(basename ${f%.txt}).threeCol.txt
done

head *threeCol.txt
```
Modifying the files got rid of that error!

Once I had my input files, I ran the code:

```
#Create output directory
mkdir ${CORR}/20_OC_N/correlation_output

#Run BAT_DMRcalling
#-b: BED file with coordinates of methylation region and name of identifier: chr <tab> start <tab> end <tab> identifier
#-e: List of expression files, one per sample. Each sample's file: identifier <tab> something <tab> expression_value
#-m: File containing a list of methylation bigWig files (one per sample).
#-g: File containing the association of sample identifiers to groups. The list of samples in the file must be ordered analogously as in the list of files given with options -e and -m.: sample <tab> group
#-i: Comma-separated list of group1 and group2
#-o Path/prefix for output

BAT_correlating \
-b ${CORR}/20_OC_N/20_OC_N_DMR_gene.bed \
-e ${CORR}/20_OC_N/20_OC_N_expression_files.list \
-m ${CORR}/20_OC_N/20_OC_N_methylation_files.list \
-g ${CORR}/20_OC_N/20_OC_N_sample_to_group.txt \
-i NO,OC \
-o ${CORR}/20_OC_N/correlation_output/
```

The input and output files are saved [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/08-annotate-DMR/new-genome/BAT_correlating). In both statistical output files, I noticed that NA was used for correlation coefficients and p-values where the DMR did not overlap with the gene. When the DMR and gene overlapped, there were numerical values.

The [normoxia vs. outside control comparison](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/08-annotate-DMR/new-genome/BAT_correlating/20_OC_N/correlation_output) had no significant DMR-gene expression correlations.  However, the [hypoxia vs. outside control comparison](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/08-annotate-DMR/new-genome/BAT_correlating/5_OC_N/correlation_output/5_OC_N_correlation.txt) did! None of the genes significantly correlated with DMR are differentially expressed. Given that non-mammalian systems don't have clear correlations between methylation and gene expression (even in vertebrates), I think it's still worth exploring what genes had significant correlations.

**Table 1**. Genes where expression was significantly correlated with methylation

|   **ENSG_ID**  |     **Methylation Region**    | **adj.R2** | **rho** | **rho.p-val** |
|:--------------:|:-----------------------------:|:----------:|:-------:|:-------------:|
|  LOC105937346  | NC_046383.1:39015627-39015700 |    0.61    |   0.86  |     0.012     |
| zmp:0000001200 | NC_046367.1:27169459-27169669 |    0.26    |   0.86  |     0.0238    |
|  LOC105923802  |   NW_023397202.1:16469-16563  |    0.62    |   0.77  |     0.0408    |

None of these genes are annotated! Classic. Another letdown with this workflow is that it creates pdfs......but I can't access them. The code creates pdfs plotting gene expression against region methylation to show the strength of the correlation. While I can see these plots were created, I can't open or move them! Interestingly, I got an error when I tried to move the files with `rsync` saying they disappeared. These plots are probably easy enough to create myself, so I'll do that when I make some DMR plots anyways.

### Going forward

1. Create DMR figures
1. Update methods and results
5. Identify known SNP/DMR overlaps
6. Update OSF repository for all intermediate files

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
