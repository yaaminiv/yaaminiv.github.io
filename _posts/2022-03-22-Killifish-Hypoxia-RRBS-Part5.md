---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 5
tags: killifish RRBS poseidon BAT
---

## Full sample alignment

Today's the day! I'm going to start my full sample aligment. I filled out the form to request an exception to the WHOI HPC fair usage policy, since the maximum default wall time for users is 24 hours, and I need to run the mapping script for 240 hours (20 days).

### Re-trimming 190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz

Neel re-uploaded 190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz, so I ran [my `trimgalore` script]() to determine if re-upload fixed the trimming issues I was having. TL;DR it didn't. The sample must be truncated somewhere else, so Neel told me to remove that sample and proceed with the full alignment.

### Merging samples

Neel provided me [sample metadata](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/metadata/Fh_RRBS_sampledescription.xlsx) so I could write the code for `BAT_merging`. There are two populations — Scorton Creek and New Bedford Harbor — and two oxygen treatments — 5% (hypoxia) and 20% (normoxia). Since oxygen experiments were conducted in chambers, there was an outside-chamber control to correct for any effects within the chamber.

I used the [BAT_merging manual](https://www.bioinf.uni-leipzig.de/Software/BAT/mapping/#bat_merging) and [example](https://www.bioinf.uni-leipzig.de/Software/BAT/example_mapping/#example_bat_merging) to write the merging code. This step functions similarly to `methylKit::unite`, where methylation information acros samples in a treatment is combined.

```
#New Bedford Harbor, hypoxia
singularity exec --bind /vortexfs1/home/naluru/:/naluru,/vortexfs1/scratch/yaamini.venkataraman:/scratch /vortexfs1/home/naluru/bat_latest.sif \
BAT_merging \
-o ${SINGMERGED}/05-N.bam \
--bam ${SINGMAPPED}/190626_I114_FCH7TVNBBXY_L2_5-N1.bam,${SINGMAPPED}/190626_I114_FCH7TVNBBXY_L2_5-N2.bam,${SINGMAPPED}/190626_I114_FCH7TVNBBXY_L2_5-N3.bam
```

### More singularity particularities

Once I got permission from IS, I queued my [full alignment script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-alignment.sh). And of course, it immediately failed. I got the following error:

```
Mapping Module
[INFO]  Thu Mar 17, 13:57:18, 2022      BAT_mapping v0.1 started
[INFO]  Thu Mar 17, 13:57:18, 2022      Checking flags

    usage:  perl BAT_mapping  -g <string> -q <string> -i <string> -o <string> [-p <string>] [-t <number>] [-F <number>] [--tmp <string>] [-a <string>] [--segemehl <string>] [--samtools <string>] [--exclude <number>] [--stdout]

    [INPUT]     -g          path/filename of reference genome fasta
                -q          path/filename of query sequences (reads)
                -p          path/filename of mate pair sequences (default: none)
                -i          path/prefix of database indices

    [GENERAL]   -o          path/prefix of outfiles
                -t          start <num_threads> threads (default: 1)
                -F          bisulfite mapping with methylC-seq/Lister et al. (=1) or bs-seq/Cokus et al. protocol (=2) (default: 1)
                -a          quoted string of additional segemehl parameters (default: none)
                --tmp       path of temporary directory (default: result directory)
                --exclude   if XF_flag>number, mapping is excluded from regular bam file (default: 3)
                --segemehl  path/filename of segemehl executable (default: installed)
                --samtools  path/filename of samtools (default: installed)
##### AN ERROR has occurred: required option -p missing
```

I was able to access the `singularity` container and start running `BAT_mapping`, but it couldn't find my files. After messing around with `singularity` interactively, I realized I needed to set variables within the `singularity` container if I wanted to use them with `singularity exec`. [Proceed internet digging](https://sylabs.io/guides/latest/user-guide/environment_and_metadata.html?highlight=variable#env-option). I found I could use `--env` when calling the container to define variables. I tested this with `TRIMMED=/scratch/02-trimgalore` and it worked! But (for obvious reasons) I was unable to set `FASTQ=ls /vortexfs1/scratch/yaamini.venkataraman/02-trimgalore/*gz  | rev | cut -c15- | rev | sort | uniq` with `--env`. I decided to define variables by calling a file with `--env-file`, saved [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-alignment-envfile.txt). To create a list of file prefixes to use, I created an array.

The script failed again because it couldn't identify the files. So something was wrong with my array, or the way my array was being called in the for loop. Based on [this Stack Overflow suggestion](https://stackoverflow.com/questions/52901012/what-is-a-list-in-bash), I used `"${FASTQ[@]}"` instead of `$FASTQ` to get the for loop to go through my array. Unfortunately this didn't solve my issue. I tried working with `singularity` interactively to see if I could figure out the problem.

I was able to create the array! But I realized the directories were all wrong, and that's probably why the files couldn't be found! I re-made my array with the proper paths based on the bound directories in the container:

```
#List of files to process
FASTQ=(/scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-N4 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-S1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-S3 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-S4 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_5-N1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_5-N2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_5-S3 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_5-S4 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_OC-S1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_20-N2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_5-N3 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_5-S2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-N1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-N2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-N4 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-N5 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-S2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-S3 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L4_20-N1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L4_20-S2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L4_5-S1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L4_OC-S5)
```

Unfortunately, that didn't solve my problem of actually having the files called properly in a loop. I posted [this issue](https://github.com/RobertsLab/resources/discussions/1436). After walking him through my issues, Sam suggested I create a new script with all of my mapping commands. I can then call that bash script within my `singularity exec` command. The bash script would also have the command to create the `FASTQ` array, since I was unable to pass the array with `-env_file`. I created the separate [`BAT_mapping`](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-BAT-mapping.sh) and [`BAT_mapping_stat`](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-BAT-mapping-stat.sh) scripts. I then tested that I was able to pass files through with the array interactively:

```
#Populate FASTQ array with prefixes of files to process
FASTQ=(/scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-N4 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-S1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-S3 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-S4 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_5-N1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_5-N2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_5-S3 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_5-S4 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_OC-S1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_20-N2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_5-N3 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_5-S2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-N1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-N2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-N4 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-N5 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-S2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L3_OC-S3 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L4_20-N1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L4_20-S2 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L4_5-S1 /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L4_OC-S5)

for f in "${FASTQ[@]}"
do
  echo ${TRIMMED}/${f}_1_val_1.fq.gz
done
```

This worked! I moved the scripts over to Poseidon and made them executable:

```
chmod +x 03-BAT-mapping.sh
chmod +x 03-BAT-mapping-stat.sh
```

I then modified my mapping and statistics code to call my scripts. I tried running the script and found out I did need to bind my home directory, so I did that:

```
echo "Mapping Module"

#Run BAT_mapping script
singularity exec --env-file /vortexfs1/home/yaamini.venkataraman/03-alignment-envfile.txt \
--bind /vortexfs1/home/naluru/:/naluru,/vortexfs1/scratch/yaamini.venkataraman:/scratch,/vortexfs1/home/yaamini.venkataraman/:/yaaminiv \
/vortexfs1/home/naluru/bat_latest.sif \
/yaaminiv/03-BAT-mapping.sh

echo "Done with mapping"

echo "Statistics Module"

#Run BAT_mapping_stat script
singularity exec --env-file /vortexfs1/home/yaamini.venkataraman/03-alignment-envfile.txt \
--bind /vortexfs1/home/naluru/:/naluru,/vortexfs1/scratch/yaamini.venkataraman:/scratch,/vortexfs1/home/yaamini.venkataraman/:/yaaminiv \
/vortexfs1/home/naluru/bat_latest.sif \
/yaaminiv/03-BAT-mapping-stat.sh

echo "Done with statistics"
```

Of course, more errors. I kept getting the error that `-p` file was not found, so I thought I'd check to make sure that the variables passed with `--env-file` were still valid with `ls $TRIMMED`. I was able to print all the files in that directory in my SLURM output, so I know the variables still work. I tried one more thing: printing the full file paths used.

```
for f in "${FASTQ[@]}"
do
echo ${TRIMMED}/${f}_1_val_1.fq.gz
echo ${TRIMMED}/${f}_2_val_2.fq.gz
done
```

```
/scratch/02-trimgalore//scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-N4_1_val_1.fq.gz
/scratch/02-trimgalore//scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-N4_2_val_2.fq.gz
```

...whoops. I fixed the array by removing directory paths. Now it's running!

### Going forward

1. Check on alignment
4. Review next BAT module

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
