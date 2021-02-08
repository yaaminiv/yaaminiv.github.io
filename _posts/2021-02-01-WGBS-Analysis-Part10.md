---
layout: post
comments: true
title: WGBS Analysis Part 10
tags: manchester gigas-broodstock fastqc mox
---

## Analyzing the full dataset

At the end of last week, I [got my samples back from ZymoResearch]()! These are WGBS samples of *C. gigas* female gonad tissue from my Manchester experiment. The purpose of sequencing these samples is to determine if the low pH exposure altered the methylome. If we find something interesting from these samples, we may sequence some larval samples, or look into gonad RNA.

### Obtaining sequences

The first thing I needed to do was download the data. ZymoResearch provided two URLs: one for sample fastq files, and another for sample checksums. Each URL directed to a text file. The file with fastq information contained a link that could be curled to access the individual files. In order to get the data, I needed a straightforward way to specify multiple URLs in a `curl` or `wget` command.

I found [this link](https://www.howtogeek.com/447033/how-to-use-curl-to-download-files-from-the-linux-command-line/) with information on how to do just that! Once I downloaded the text file, I could use `xargs` to specify the individual URls to download:

```
curl https://epiquest.s3.amazonaws.com/epiquest_zr3616/yZDEfJOtQKi6iHfwuDtQa3mUJiy4vC/download_fastq.txt > download_fastq.txt #Download file provided by ZymoResearch
xargs -n 1 curl -O < download_fastq.txt  #Download fastq files
```

Then, I verified checksums:

```
curl https://epiquest.s3.amazonaws.com/epiquest_zr3616/yZDEfJOtQKi6iHfwuDtQa3mUJiy4vC/zr3616_MD5.txt > zr3616_MD5.txt #Download MD5 checksums from ZymoResearch
find *fq.gz | md5sum -c zr3616_MD5.txt #Cross-reference original checksums with downloaded files. All files passed
```

Once I had the data, I needed to move it to `owl` and include metadata in the [`Nightingales Google Sheet`](https://b.link/nightingales). I used `mv` to move samples from my Desktop to `owl` mounted on my computer, but couldn't write to the directory. I posted [this issue](https://github.com/RobertsLab/resources/issues/1085) to get write access to both the `owl` folder and Google Sheet. As an aside, Sam reminded me I should use `rsync` to transfer the files:

```
rsync —archive —progress —verbose zr3616_* /Volumes/web-1/nightingales/C_gigas/ #Transfer to nightingales
```

While the files transferred over many hours, I updated the Google Sheet with sample metadata.

### Raw data quality

 When I received the data from ZymoResearch, the pdf implied that adapters were trimmed out of the data. Confused, I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1082) to determine if I needed to modify my trimming protocol. Sam informed me that the samples were likely not trimmed, and I would see this if I ran [`FastQC`](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/). Once the files were transferred to Nightingales, I needed to transfer them to `mox` and do just that.

 I used `rsync` to transfer the files:

```
rsync —archive —verbose —progress yaamini@172.25.149.226:/Volumes/web-1/nightingales/C_gigas/zr3616*gz /gscratch/scrubbed/yaaminiv/Manchester/data/ #Transfer to mox
```

Then, I set up [a `fastqc` script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/01-fastqc.sh). To streamline the repository, I moved my preliminary analyses to the [pooled subset](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/pooled-subset) subdirectory. This way, my scripts will remain linked to this repository, but it won't clutter it. Running `fastqc` seemed simple. I located the program on mox in the `srlab` programs directory. The command to run the program is `fastqc` + filenames, so I specified the directory with my data, and included code for `[`MultiQC`](https://multiqc.info/)` report compilation. When I ran this script, it obviously didn't work!

Figuring Sam must have run `fastqc` recently, I found [this lab notebook entry](https://robertslab.github.io/sams-notebook/2020/11/10/FastQC-MultiQC-C.gigas-Ploidy-WGBS-Raw-Sequence-Data-from-Ronits-Project-on-Mox.html) with a `fastqc` script for `mox.` I wasn't sure why Sam needed to work with the files in an array, instead of calling `fastqc,` but I figured it had to do with the JavaScript error I encountered when I tried to do just that. I copied the code into my script and modified the variables so they pointed to the correct directories and samples. With his code, `fastqc` will generate sequence quality reports, `multiqc` will compile them, and checksums will be generated. I probably don't need the checksum information since `rsync` verifies checksums when moving files, and I have the original checksum information from ZymoResearch. In any case, it couldn't hurt.

Once I finished modifying the script, I transferred the script and run it:

```
rsync —archive —verbose —progress yaamini@172.25.149.226:/users/yaamini/Documents/project-gigas-oa-meth/code/01-fastqc.sh . #Transfer script to user directory
sbatch 01-fastqc.sh #Run script
```

My script finished running in 40 minutes. It exited with an error message, but when I checked the [`multiqc` report](https://gannet.fish.washington.edu/spartina/project-gigas-oa-meth/output/fastqc/multiqc_report.html) all samples were accounted for. Looking at the report, I saw that there were no identified adapter sequences in any sample.

<img width="1084" alt="Screen Shot 2021-02-02 at 12 27 25 AM" src="https://user-images.githubusercontent.com/22335838/106573227-376fcd00-64ee-11eb-8b85-f4a45d69b1bd.png">

**Figure 1**. Summary of FastQC report parameters for all samples

When I dug into individual reports, I noticed that there were overrepresented PCR primers. And of course, poly-G tails were present in almost all R2 samples, leading to a spike in the tail end of the per sequence GC content distribution.

<img width="763" alt="Screen Shot 2021-02-02 at 12 32 13 AM" src="https://user-images.githubusercontent.com/22335838/106573237-3a6abd80-64ee-11eb-988a-803a5d570749.png">

<img width="623" alt="Screen Shot 2021-02-02 at 12 32 32 AM" src="https://user-images.githubusercontent.com/22335838/106573245-3ccd1780-64ee-11eb-9a4e-f2d46d3834cc.png">

**Figures 2-3**. Overrepresented sequences and per sequence GC content distribution for sample 2 R2.

The lack of adapter sequences was concerning, so I looked at the [Hawaii *C. gigas* MultiQC report](https://gannet.fish.washington.edu/Atumefaciens/20201206_cgig_fastqc_multiqc_ploidy-pH-wgbs/multiqc_report.html#fastqc_adapter_content). Although there were plenty of adapter and overrepresented sequences in that data, the summary at the bottom of the report doesn't indicate this!

**Figure 4**. Summary of FastQC report parameters for Hawaii samples

<img width="1107" alt="Screen Shot 2021-02-02 at 12 43 15 AM" src="https://user-images.githubusercontent.com/22335838/106574394-b6193a00-64ef-11eb-9b3e-bab3ace762d8.png">

This is a very odd classification convention, but I think it means my samples were not trimmed. I can proceed to the next step!

### Trimming samples

Since I saw poly-G tails in the FastQC reports, I know I need three rounds of trimming. Luckily, I already had my [Hawaii data `trimgalore` script](https://github.com/RobertsLab/project-oyster-oa/blob/master/code/Haws/01-trimgalore-2.sh) I could modify for this project. [For this project's script](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/02-trimgalore.sh), I updated file paths, directory paths, and included `echo` commands so I could easily search the slurm output and determine which steps were completed. I then transferred the script to `mox` and let it run:

```
rsync --archive --progress --verbose yaamini@172.25.149.226:/Users/yaamini/Documents/project-gigas-oa-meth/code/02-trimgalore.sh . #Transfer script to user directory
sbatch 02-trimaglore.sh #Run script
```

### Going forward

1. Check trimming output
2. Start `bismark`
3. Identify DML
2. Determine if RNA should be extracted
3. Determine if larval DNA/RNA should be extracted

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
