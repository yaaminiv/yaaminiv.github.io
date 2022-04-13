---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 6
tags: killifish RRBS poseidon BAT
---

## Continuing sample alignment

I'm chugging along with the [full sample alignment and mapping](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part5/). I ran into a few issues with the SLURM scheduler timing out even though I modified the SLURM header the way IS told me to.

### Errors in mapping code

```
/yaaminiv/03-BAT-mapping-stat.sh: line 15: /vortexfs1/scratch/yaamini.venkataraman/03-mapping/stat/190626_I114_FCH7TVNBBXY_L2_20-N4.stat: No such file or directory
```

Since I'm running a script inside a `singularity exec` call, I realized I ran into the reverse of a previous problem. I needed to include the path to the output file using the path to the mounted directories, not the path on the host system!

```
#Get alignment statistics
for f in "${FASTQ[@]}"
do
  BAT_mapping_stat \
  --bam ${SINGMAPPED}/${f}.bam \
  --excluded ${SINGMAPPED}/${f}.excluded.bam \
  --fastq ${TRIMMED}/${f}_1_val_1.fq.gz \
  > /scratch/03-mapping/stat/${f}.stat
done
```

I obtained the mapping statistics for all 22 samples, but still needed to merge files before proceeding with the [`BAT_calling`](https://www.bioinf.uni-leipzig.de/Software/BAT/calling/) module. That's when I ran into this error:

```
Error in tempfile() using template /XXXXXXXXXX: Could not create temp file /2rcjMIFtOS: Read-only file system at /usr/local/bin/BAT_merging line 220.
```

Since I hadn't tried `BAT_merging` previously, I didn't know what the error was. Looking at [line 220 of `BAT_merging`](https://github.com/helenebioinf/BAT/blob/master/BAT_merging#L220) didn't give me any insight either. I decided to test out the code interactively before running the script through SLURM. I started a new container with `singularity run` and tried my `BAT_merging` code. I was able to make it past the header step before I ran into another error:

```
Singularity> BAT_merging \
> -o ${SINGMERGED}/05-N.bam \
> --bam ${SINGMAPPED}/190626_I114_FCH7TVNBBXY_L2_5-N1.bam,${SINGMAPPED}/190626_I114_FCH7TVNBBXY_L2_5-N2.bam,${SINGMAPPED}/190626_I114_FCH7TVNBBXY_L2_5-N3.bam
[INFO]	Tue Apr 12, 10:37:28, 2022	BAT_merging v0.1 started
[INFO]	Tue Apr 12, 10:37:28, 2022	Checking flags
[INFO]	Tue Apr 12, 10:37:28, 2022	Build header
Use of uninitialized value $readgroup in concatenation (.) or string at /usr/local/bin/BAT_merging line 231.
[INFO]	Tue Apr 12, 10:37:29, 2022	Merge BAMs
##### AN ERROR has occurred: Please view the log file
```

The command started to create 05-N.bam, so I know there are no issues with my paths. I'm not sure if the error in line 231 is related to the error in line 220. Out of curiosity, I tried running the command when only merging two files, since that's what the [example shows](https://www.bioinf.uni-leipzig.de/Software/BAT/example_mapping/#example_bat_merging).

```
Singularity> BAT_merging \
> -o ${SINGMERGED}/05-N.bam \
> --bam ${SINGMAPPED}/190626_I114_FCH7TVNBBXY_L2_5-N1.bam,${SINGMAPPED}/190626_I114_FCH7TVNBBXY_L2_5-N2.bam
```

That command finished with a merged file and no errors. I referred to a BAT script Neel sent and didn't see the merging step, and the website only refers to it being used when the same sample is sequenced multiple times. I was confused as to whether or not I even needed to merge the samples, so I emailed Neel to 1) clarify I should proceed with `BAT_merging` and 2) see if he had code that worked for merging more than two files.

Neel emailed me back said I need to use `BAT_summarize` to merge biological replicates, which falls in the `BAT_calling` module. So no need to use `BAT_merging`! I updated my [SLURM script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-alignment.sh) and [variable file](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-alignment-envfile.txt) to reflect this change. Since I was done with the alignment module, I moved my output to my home directory on `vortex`.

### Mapping results

I uploaded all mapping results [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/tree/main/output/03-mapping). I looked through the pdfs the `BAT_mapping` produced and it seems like most paired-end reads had < 25 hits, which could indicate that there wasn't huge duplication in sequenced reads. To summarize the mapping statistics, I created [this table](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/03-mapping/stat/mapping-statistics.csv). I should have created an R script to pull the results out but...oh well. Alignment ranged from 69-80% across all samples, with mean alignment at 74%. My quick scan didn't see any batch or treatment impacts on alignment, but that can certainly be checked later.

### Going forward

1. Review next BAT module
2. Run `BAT_calling`

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
