---
layout: post
comments: true
title: WGBS Analysis Part 25
tags: manchester gigas-broodstock bedtools
---

## Generating genome feature tracks

Before I could determine the location of DML, I needed to make genome feature tracks for the Roslin genome! I figured I could pull from my experience (and code) making these tracks for the [*C. virginica*](https://github.com/epigeneticstoocean/paper-gonad-meth/blob/master/code/07-Generating-Genome-Feature-Tracks.ipynb) and the [coral methylation papers](https://github.com/hputnam/Meth_Compare/blob/master/code/03.01-Generating-Genome-Feature-Tracks.ipynb).

### RefSeq information

I started by creating [this Jupyter notebook](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/code/08-Generating-Genome-Feature-Tracks.ipynb) and downloading the [NCBI RefSeq annotation](https://www.ncbi.nlm.nih.gov/genome/annotation_euk/Crassostrea_gigas/102/). I briefly remember Steven mentioning the RefSeq annotation had more genome feature information than the Genbank version, so I figured I'd start there. After unzipping my downloaded file, I wanted to get an idea for which features were present in the file. I used `cut` to extract the column with genome feature information (column 3), then got the unique entries with `sort | uniq -c`:

```
!cut -f3 ncbi-genomes-2021-05-04/GCF_902806645.1_cgigas_uk_roslin_v1_genomic.gff | sort | uniq -c
```

There were a bunch of genome features in this annotation file! I decided to take the genes, CDS, exons, lncRNA, and mRNA tracks. I could then use that information for exon UTR, introns, intergenic regions, and flanking regions. The output also gave me the number of all of those features, so I could use that information to ensure my track extraction was correct! I started extracting the genes with the following `grep` code:

```
#Isolate gene entries from multiple annotation databses. Tab must be included between database and feature
!grep "Gnomon	gene" \
ncbi-genomes-2021-05-04/GCF_902806645.1_cgigas_uk_roslin_v1_genomic.gff \
> cgigas_uk_roslin_v1_gene.gff
```

When I used `wc -l` to check the number of genes, but I was missing ~1000 genes! I then decided to see which databases were used to create the annotation file:

```
#Database identifiers for extracting features
!cut -f2 ncbi-genomes-2021-05-04/GCF_902806645.1_cgigas_uk_roslin_v1_genomic.gff | sort | uniq -c | tail
```

Then, I used all four databases to extract all genes:

```
#Isolate gene entries from multiple annotation databses. Tab mus be included between database and feature
!grep -e "Gnomon	gene" -e "RefSeq	gene" -e "cmsearch	gene" -e "tRNAscan-SE	gene" \
ncbi-genomes-2021-05-04/GCF_902806645.1_cgigas_uk_roslin_v1_genomic.gff \
> cgigas_uk_roslin_v1_gene.gff
```

### Issues with `bedtools`

I proceeded to extract CDS, exon, lncRNA, and mRNA tracks. Before I could get the othere tracks, I needed to extract chromosome names and lengths. I used the following code to create the genome file required by `bedtools`:

```
!awk '$0 ~ ">" {print c; c=0;printf substr($0,2,100) "\t"; } $0 !~ ">" {c+=length($0);} END { print c; }' \
ncbi-genomes-2021-05-04/GCF_902806645.1_cgigas_uk_roslin_v1_genomic.fna \
| awk '{print $1"\t"$2}' \
> cgigas_uk_roslin_v1-sequence-lengths.txt
```

Unfortunately, the header for each sequence has extra descriptive text! I initially tried using `tr " " "\t"` to separate out the descriptive text into columns, then only save the first and last columns (hopefully the chromosome name and sequence length)l. However, the descriptive text separated into different numbers of columns depending on the row, so that wasn't an option. I used `tr " " "\t"` with the `awk` command and `cut` in one chunk to extract the chromosome name and save it in a text file. Then, I used the `awk` command and `cut` only to extract the sequence length. Finally, I used `paste` to combine the columns and create [my genome file](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/genome-feature-files/cgigas_uk_roslin_v1-sequence-lengths.txt)!

![Screen Shot 2021-05-05 at 10 40 11 AM](https://user-images.githubusercontent.com/22335838/117185043-459fe480-ad8e-11eb-916f-978c52781d8f.png)

![Screen Shot 2021-05-05 at 10 40 24 AM](https://user-images.githubusercontent.com/22335838/117185082-4d5f8900-ad8e-11eb-8a61-26fa822a7288.png)

![Screen Shot 2021-05-05 at 10 40 40 AM](https://user-images.githubusercontent.com/22335838/117185115-57818780-ad8e-11eb-84ba-f43b92b11e4a.png)

I then proceeded to use `complementBed` with the exon track to create a non-coding sequence track. However I encountered an error!

![Screen Shot 2021-05-05 at 10 37 53 AM](https://user-images.githubusercontent.com/22335838/117184772-f3f75a00-ad8d-11eb-9e9b-c23878a55548.png)

For some reason, `complementBed` wasn't accepting the genome file I created, even though it had the two required fields. I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1201) to get input while I did some troubleshooting. In referring to my old notebooks, I realized that I was using an older version of `bedtools` than I had previously! When I used the newer version, I was able to run `complementBed` successfully. After I created the non-coding sequence, intron, and intergenic tracks, I decided to create the flanking tracks. I then encountered the same error with `flankBed`!

![Screen Shot 2021-05-05 at 11 21 06 AM](https://user-images.githubusercontent.com/22335838/117190205-fd83c080-ad93-11eb-9ca1-6842670739be.png)

Sam suggested I make genome file again in one chunk, instead of pasting two columns together. Instead of pasting two columns together, I went down the rabbit hole of trying to cut the text string "Crassostrea.......shotgun sequence". I changed the `awk` command so it pulls out characters 2-14 of the sequence header. This worked well for the end of the file, but not the beginning:

![Screen Shot 2021-05-05 at 12 08 11 PM](https://user-images.githubusercontent.com/22335838/117195779-90bff480-ad9a-11eb-97db-63cc10d14a89.png)

I ended up adding `sed 's/Cr//g'` after the `awk` code (take characters 2-14 of the header) to remove the remaining "Cr" in certain entries!

![Screen Shot 2021-05-05 at 12 28 10 PM](https://user-images.githubusercontent.com/22335838/117198029-5b68d600-ad9d-11eb-900e-8b3948fa1b08.png)

I still got the same error with `flankBed`, so I was going to work from the index file like Sam suggested. Before I got around to doing that, I noticed that there was an empty first line in my file:

![Screen Shot 2021-05-05 at 12 33 35 PM](https://user-images.githubusercontent.com/22335838/117198640-1e511380-ad9e-11eb-8558-c2ffcab7b44d.png)

I added a final modification to my code to remove the first line of the file, and that worked!

```
!awk '$0 ~ ">" {print c; c=0;printf substr($0,2,14) "\t"; } $0 !~ ">" {c+=length($0);} END { print c; }' \
ncbi-genomes-2021-05-04/GCF_902806645.1_cgigas_uk_roslin_v1_genomic.fna \
| sed 's/Cr//g' \
| awk '{print $1"\t"$2}' \
| tail -n +2 \
> cgigas_uk_roslin_v1-sequence-lengths.txt
```

### Transposable elements

The last track I needed was for transposable elements. I initially posted [this issue](https://github.com/RobertsLab/resources/issues/1141), and Sam produced a RepeatMasker track. Both of us also realized there's RepeatMasker output on the NCBI annotation page! I downloaded the output. I noticed that there were several columns I didn't need, and tried to create a BEDfile with just the chromosome, start, and stop information. When I tried to `cut` the columns, I (of course) couldn't get it to work!

![Screen Shot 2021-05-05 at 4 07 38 PM](https://user-images.githubusercontent.com/22335838/117220410-05575b00-adbc-11eb-8ded-b81ce44cd521.png)

I posted [this discussion](https://github.com/RobertsLab/resources/discussions/1202), and Sam suggested I use the following code (which of course worked):

```
#Convert RepeatMasker output to a BEDfile
#Skip the first 4 lines
#Print columns 5-7 as a tab-delimited output
!tail -n +4 cgigas_uk_roslin_v1_rm.te \
| awk 'BEGIN{OFS= "\t"} {print $5, $6, $7}' \
> cgigas_uk_roslin_v1_rm.te.bed
```

### IGV

Finally, I wanted to look at these tracks in IGV. I created [this IGV session](https://github.com/RobertsLab/project-gigas-oa-meth/blob/master/genome-feature-files/genome-feature-files.xml), and breezed through the chromosomes to ensure the tracks were generated properly:

![Screen Shot 2021-05-06 at 12 57 59 PM](https://user-images.githubusercontent.com/22335838/117386761-49ba2800-ae9c-11eb-8cc0-5a98f7370c35.png)

![Screen Shot 2021-05-06 at 1 06 54 PM](https://user-images.githubusercontent.com/22335838/117386771-4fb00900-ae9c-11eb-8782-14293a9df0af.png)

![Screen Shot 2021-05-06 at 1 12 39 PM](https://user-images.githubusercontent.com/22335838/117386774-50e13600-ae9c-11eb-9c27-ab1e2bd90877.png)

![Screen Shot 2021-05-06 at 1 12 53 PM](https://user-images.githubusercontent.com/22335838/117386777-5179cc80-ae9c-11eb-8921-c00ce3caabeb.png)

I uploaded the tracks to `owl` and updated the [Roberts Lab Handbook](https://robertslab.github.io/resources/Genomic-Resources/#crassostrea-gigas-cgigas_uk_roslin_v1) with the tracks. Now I think I'm ready to use these genome feature tracks!

### Going forward

1. Finish running `methylKit` randomization tests on R Studio server
2. Write methods
3. Write results
2. Update `mox` handbook with R information
2. Obtain relatedness matrix and SNPs with [EpiDiverse/snp](https://github.com/EpiDiverse/snp)
4. Determine genomic location of DML
5. Identify overlaps between DML, SNP data, and other epigenomic data
6. Identify significantly enriched GOterms associated with DML
7. Identify methylation islands and non-methylated regions
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
