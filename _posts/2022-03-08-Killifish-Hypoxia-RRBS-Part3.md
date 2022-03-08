---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 3
tags: killifish RRBS poseidon
---

## Mapping RRBS data

I still don't know what's going on with [my potentially truncated data](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part2/), but I decided to take the samples I already trimmed and test mapping code. I'm using the [Bisulfite Analysis Toolkit](https://www.bioinf.uni-leipzig.de/Software/BAT/), and their mapping procedure has two steps: aligning reads and getting the mapping statics.

### Understanding singularity

Based on the [BAT_mapping usage page](https://www.bioinf.uni-leipzig.de/Software/BAT/mapping/#basic_usage) and [examples](https://www.bioinf.uni-leipzig.de/Software/BAT/example_mapping/), I put together [this code](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-alignment.sh). When I ran the shell script, it failed almost immediately!

```
/cm/local/apps/slurm/var/spool/job2370991/slurm_script: line 25: BAT_mapping: command not found
```

This was weird, because I was opening a `singularity` container that had `BAT_mapping` in the previous command. A quick Google search showed me that I should use `singularity exec` instead of `singularity run`. This would require a command right after I loaded a container.

```
singularity exec /vortexfs1/home/naluru/bat_latest.sif \
BAT_mapping \
-g /vortexfs1/home/naluru/Killifish/Fundulus_heteroclitus.Fundulus_heteroclitus-3.0.2.dna.toplevel.fa.gz \
-q /vortexfs1/scratch/yaamini.venkataraman/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-N4_1_val_1.fq.gz \
-p /vortexfs1/scratch/yaamini.venkataraman/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-N4_2_val_2.fq.gz \
-i /vortexfs1/home/naluru/Killifish/Fundulus_heteroclitus.Fundulus_heteroclitus-3.0.2.dna.toplevel \
-o /vortexfs1/scratch/yaamini.venkataraman/03-mapping/190626_I114_FCH7TVNBBXY_L2_20-N4_nondirectional \
-t 16 \
-F 2
```

Using `singularity exec` solved my issue of having `BAT_mapping` available, but then I ran into a new issue:

```
mkdir /vortexfs1/scratch: Read-only file system at /usr/local/bin/BAT_mapping line 106.
```

I shot off a quick email to Neel with the error to see if I knew what was happening. Neel suggested there was an issue with the output folder, and in running the code interactively I found the problem fixed when I used a folder in my home directory for the output prefix instead of the scratch directory. But of course, there was another issue:

```
##### AN ERROR has occurred: required option -g missing or nonexistent
```

The program couldn't find my genome file! I knew it existed, but when I tried to find it within the `singularity` container I couldn't. Another quick search lead me to [this page](https://sylabs.io/guides/3.0/user-guide/bind_paths_and_mounts.html). Essentially, I access a `singularity` container by "swapping" file systems with my host operating system, so I can't access anything in my host system unless I bind it to my container. I was able to bind Neel's home directory (with the genome files) and my `scratch` directory (where all my files are located and where I want to put the output) to my container:

```
singularity run --bind /vortexfs1/home/naluru/:/naluru,/vortexfs1/scratch/yaamini.venkataraman:/scratch /vortexfs1/home/naluru/bat_latest.sif
```

Once I loaded the container, I ran the following code to test mapping parameters on one sample:

```
BAT_mapping \
-g /naluru/Killifish/Fundulus_heteroclitus.Fundulus_heteroclitus-3.0.2.dna.toplevel.fa.gz \
-q /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-N4_1_val_1.fq.gz \
-p /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-N4_2_val_2.fq.gz \
-i /naluru/Killifish/Fundulus_heteroclitus.Fundulus_heteroclitus-3.0.2.dna.toplevel \
-o /scratch/03-mapping/190626_I114_FCH7TVNBBXY_L2_20-N4_nondirectional \
-t 16 \
-F 2
```

I wanted to test one sample so I could build the genome indices and not have to go through that time-intensive step when I go through this process for all samples.

### Testing mapping parameters

Once I built the genome indices, I returned to my [SBATCH script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/03-alignment.sh) to modify paths based on where I'm mounting directories. I also had to figure out one more `singularity ` issue: how to open a module within my script and run the commands. I settled on a loop with `singularity exec`:

```
#Assuming non-directional (- F 2)
for f in $FASTQ
do
  singularity exec --bind /vortexfs1/home/naluru/:/naluru,/vortexfs1/scratch/yaamini.venkataraman:/scratch /vortexfs1/home/naluru/bat_latest.sif \
  BAT_mapping \
  -g $GENOME \
  -q ${f}_1_val_1.fq.gz \
  -p ${f}_2_val_2.fq.gz \
  -i $INDICES  \
  -o ${MAPPED}/${f} \
  -t 16 \
  -F 2
done
```

This should work when I'm ready to process all samples in a list `$FASTQ`. I can use the same technique when running samples individually:

```
#Test sample 1
singularity exec --bind /vortexfs1/home/naluru/:/naluru,/vortexfs1/scratch/yaamini.venkataraman:/scratch /vortexfs1/home/naluru/bat_latest.sif \
BAT_mapping \
-g /naluru/Killifish/Fundulus_heteroclitus.Fundulus_heteroclitus-3.0.2.dna.toplevel.fa.gz \
-q /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-N4_1_val_1.fq.gz \
-p /scratch/02-trimgalore/190626_I114_FCH7TVNBBXY_L2_20-N4_2_val_2.fq.gz \
-i /naluru/Killifish/Fundulus_heteroclitus.Fundulus_heteroclitus-3.0.2.dna.toplevel \
-o /scratch/03-mapping/190626_I114_FCH7TVNBBXY_L2_20-N4_nondirectional \
-t 16 \
-F 2
```

I started running the script with 2 test samples for directional and non-directional mapping. Fingers crossed this gives good information about mapping parameters and if the libraries were directional or not!

### Going forward

1. Get mapping statistics for test samples
2. Figure out what's happening with sample 190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz
3. Trim sample 190626_I114_FCH7TVNBBXY_L4_OC-N3_1.fq.gz appropriately
3. Start alignment with all samples

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
