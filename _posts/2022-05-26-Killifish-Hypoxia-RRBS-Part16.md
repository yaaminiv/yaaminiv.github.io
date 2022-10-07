---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 16
tags: killifish RRBS poseidon BAT
---

## Methylation landscape information

I have two small tasks I need to accomplish: characterize the methylation landscape and generate genome feature tracks. Turns out these small tasks are big tasks that brought about more questions (#classic).

### Methylation landscape

Characterizing the methylation landscape is fueled by a need to update the results section of my manuscript. To determine what methylation landscape information I needed, I reviewed Neel's zebrafish preprint. Based on that results section, I needed global methylation percentages and the number of methylated and unmethylated CpGs for each treatment. I created [this Jupyter notebook](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-methylation-landscape-analysis.ipynb) to get the information I needed.

The first thing I did was create a union bedGraph with sorted and filtered bedGraphs for each sample so I could count the number of CpGs with 10x-500x coverage in at least one sample. The `metilene` output only considered CpGs with data in all samples for DMR analysis, so I couldn't just look at the number of CpGs from my all populations comparison. Which brought up a question...should I allow for missing values with `--mis1` and `--mis2` in `BAT_summarize`? Within populations, I don't think it makes sense to allow for missing values since the sample size is small to begin with. But perhaps for the all populations comparison, I could account for missing values for a few samples at a CpG locus.

Next, I calculated global methylation level and the number of methylated and unmethylated CpGs. I set the cutoff for methylated CpGs as > 10% methylation rate, since I'm using that same cutoff to define a DMR. I need to run this by Neel since that information wasn't in the preprint. I used this code to count CpGs in each category:

```
#Remove header
#Find CpGs with > 10% methylation
#Count number of CpGs
! tail -n+2 all-samples-averages.bedgraph \
| awk -F'\t' -v OFS='\t' '{if ($4 > .1) { print $26 }}' ${f} \
| wc -l

#Remove header
#Find CpGs with > 10% methylation
#Count number of CpGs
! tail -n+2 all-samples-averages.bedgraph \
| awk -F'\t' -v OFS='\t' '{if ($4 <= .1) { print $26 }}' ${f} \
| wc -l
```

I used this code to average methylation rate across all loci to get global methylation level:

```
#Calculate average methylation
! tail -n+2 all-samples-averages.bedgraph \
| awk '{ total += $26; count++ } END { print total/count }'
```

Interestingly, there were more unmethylated than methylated CpGs, and global methylation rate was ~20%. This was pretty consistent across populations and population x treatment combinations. My understanding of vertebrates is that they have higher levels of methylation. I wonder if accommodating missing loci would boost methylation rate, or if killifish just have lower constitutive levels of methylation?

### Genome feature struggles

I went to download the GFF for the genome version (3.0.2) I was using from NCBI but realized I couldn't! I also couldn't get the RepeatMasker output. I was only able to download the GFF for the newest genome (4.1). Neel had the GTF associated with the genome version I was using on `vortex`, but it poses larger questions: 1) does Neel have a GFF for v.3.0.2 and 2) should I be using v.4.1 instead?

I decided to extract genome feature tracks from the GTF I had in [this Jupyter notebook](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/07-generating-genome-feature-tracks.ipynb). I pulled out genes, exons, CDS, 5' UTR, and 3' UTR from the GTF. The GFF would have had lncRNA information as well, so that would be nice to have. When I tried using `complementBed` to generate a non-coding track, I encountered this error:

```
#Find the complement to the exon track (non-coding sequences)
#Create a BEDfile of IGV
!{bedtoolsDirectory}/complementBed \
-i Fundulus_heteroclitus-3.0.2.105-exon.gff \
-g mummichog.chrom.length \
> Fundulus_heteroclitus-3.0.2.105-nonCDS.bed

***** ERROR: illegal number ":primary". Exiting...
```

I wondered if this had anything to do with the file format, so I extracted the chromosome, start, and stop information and fed that into `complementBed` and still got the same error. I commented on [this recently posted Github issue](https://github.com/arq5x/bedtools2/issues/998), and was shown that my length for the mitochondrial chromosome was ":primary!" Something must have gone wrong when I generated the genome file. I went to the [NCBI genome page](https://www.ncbi.nlm.nih.gov/assembly/GCA_000826765.1#/def) and opened the [full sequence report](https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/826/765/GCF_000826765.1_Fundulus_heteroclitus-3.0.2/GCF_000826765.1_Fundulus_heteroclitus-3.0.2_assembly_report.txt) to find that the mitochondrial sequence is 16,526 bp long. I updated the genome text file with this information and was able to create non-coding and intron tracks. I'm not sure how to proceed with flanking regions in killifish, and I don't know how to extract CpG island information. Without this information, I can't create an accurate intergenic region track, so I stopped there.

### Visualizing DMR and genome features

The last thing I did was create an IGV session for DMR and genome feature tracks so I could see where DMR were located before I used `bedtools` to quantify overlaps. I added the DML bedGraphs and genome feature files to [this IGV session](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/06-DMR/DMR.xml). I need to create an OSF repo soon so I can use links instead of file paths!

### Going forward

1. Determine if I should use the new genome for analysis
2. Ask Neel about missing values from group 1 and 2 for `BAT_summarize`
2. Confirm methylated and unmethylated definitions
3. Finish generating genome feature tracks
3. Annotate DMR locations
1. Update methods and results
2. Try DMR identification with `bismark` and `methylKit`
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
