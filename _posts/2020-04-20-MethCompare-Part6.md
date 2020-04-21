---
layout: post
comments: true
title: MethCompare Part 6
tags: MethCompare checksums
---

## Retroactively obtaining checksums

Based on our conversations, we still don't trust our data. While we run some alignments on a combined C1 and *P. acuta* genome to discard any reads we have that do not solely map to *P. acuta*, I decided to do something I should have done in the beginning: verify checksums.

Since I `wget` 10x bedgraphs for CpG classification, I need to look at checksums. When transferring files to or from `gannet` or Mox, I can use `rsync` which automatically verifies file integrity. I initially thought I would use Shelly's `awk` script to compare `md5sum` hashes for `gannet` files and those I downloaded to my computer, but after some discussion at Science Hour I learned I could just use `md5sum -c` with a list of hashes and paths.

In [this Jupyter notebook](https://github.com/hputnam/Meth_Compare/blob/master/scripts/Characterizing-CpG-Methylation.ipynb), I downloaded the [master list of checksums](https://gannet.fish.washington.edu/metacarcinus/FROGER_meth_compare/20200410/all_031520-TG-bs_files_GANNET_md5sum.txt) Shelly generated for `gannet` files on `ostrich` using `md5sum`. This file has hashes for files I am not using for my analysis. To see how `md5sum` would handle this, I navigated to my folder with *M. capitata* 10x bedgraphs and ran `md5sum -c all_031520-TG-bs_files_GANNET_md5sum.txt`. Since I was using `genefish` which had `gannet` mounted, FASTQC files with `gannet` paths were being checked! I realized I needed to pare down my list of files.

After navigating to the folder with the checksum file, the first thing I tried was using `grep` to extract all 10x bedgraphs for either *M. capitata* or *P. acuta*:

```
#Get all lines from original checksum document
#Extract information for 10x bedgraphs
#Extract information for Mcap data only
!cat all_031520-TG-bs_files_GANNET_md5sum.txt \
| grep 10x.bedgraph \
| grep Mcap \
> Mcap-10xbedgraph-GANNET-md5sum.txt
```

I then ran `md5sum -c` with the file I generated. All of the paths were still `gannet` paths so it did not verify checksums for the files I had. I unmounted `gannet` and tried again with a file that only had hashes. I initially thought I could use `cut -f1` to save the first column of data (hashes), but I realized that the hashes and file paths were all in one column! I used `cut -c` to identify characters I wanted to keep in my file.

```
#Get all lines from original checksum document
#Extract information for 10x bedgraphs
#Extract information for Mcap data only
#Only keep the first 32 characters in each line (md5sum hashes)
#Save hashes
!cat all_031520-TG-bs_files_GANNET_md5sum.txt \
| grep 10x.bedgraph \
| grep Mcap \
| cut -c1-32 \
> Mcap-10xbedgraph-GANNET-md5sum-hashes.txt
```

When I ran `md5sum -c` with only the hashes, I got an error that said I did not have properly formatted `md5sum` file. That made sense because there were no file paths. The file paths in the original file were `gannet` paths. Since the files had the same name on `genefish`, I just needed a way to get rid of the first part of each file path. I did many searches for code that would remove characters from the middle of a line so I could keep both the hashes and correct file names. I tried using a variation of `sed` or `tr` that would delete `/Volumes/web/seashell/bu-mox/scrubbed/031520-TG-bs/Mcap_tg/dedup` or `/Volumes/web/seashell/bu-mox/scrubbed/031520-TG-bs/Mcap_tg/nodedup`, but neither command worked. I decided to try using `cut -c` again to keep the last part of the path (the filename) and save it as a new file. I could then `paste` the hashes and filenames together to create a checksum file that only had the files I was interested in.

First, I used a combination of `rev` and `cut -c` to keep the last 48 characters at the end of each row:

```
#Get all lines from original checksum document
#Extract information for 10x bedgraphs
#Extract information for Mcap data only
#Reverse order of characters in each line
#Only keep the first 48 characters in each line
#actually the last 48 characters in the original file, which maps to paths locally
#Reverse characters
#Save paths
!cat all_031520-TG-bs_files_GANNET_md5sum.txt \
| grep 10x.bedgraph \
| grep Mcap \
| rev \
| cut -c1-48 \
| rev \
> Mcap-10xbedgraph-GANNET-md5sum-paths.txt
```

Then I pasted the paths with the previously-generated hashes:

```
#Paste hashes and paths to create a md5sum file
#Save checksum file
#Check output
!paste Mcap-10xbedgraph-GANNET-md5sum-hashes.txt Mcap-10xbedgraph-GANNET-md5sum-paths.txt \
> Mcap-10xbedgraph-GANNET-md5sum.txt
!head Mcap-10xbedgraph-GANNET-md5sum.txt
```

I navigated back to my *M. capitata* folder and ran `md5sum`:

<img width="479" alt="Screen Shot 2020-04-21 at 1 54 43 PM" src="https://user-images.githubusercontent.com/22335838/79912942-af458e80-83d7-11ea-9656-d94b50afddbf.png">

It worked! I repeated this process with *P. acuta* files and got the all-clear:

<img width="460" alt="Screen Shot 2020-04-21 at 1 55 40 PM" src="https://user-images.githubusercontent.com/22335838/79913030-cdab8a00-83d7-11ea-9f2d-fe7cb7b3eabd.png">

Once we trust our data, I can run this pipeline knowing that there are some internal checks.

### Going forward

5. [Look into exon annotations in Liew and Li papers](https://github.com/hputnam/Meth_Compare/issues/40)
6. Check code for union bedgraph files
4. Figure out how to meaningfully concatenate data for each method
5. [Generate repeat tracks for each species](https://github.com/hputnam/Meth_Compare/issues/23)
6. Rerun the pipeline with full samples once pan-genome output is assessed and find a way to generate tables programmatically
3. Create figures for CpG characterization in various genome features
6. [Update code for methylation frequency distribution figure](https://github.com/hputnam/Meth_Compare/issues/39)
3. Figure out methylation island analysis

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
