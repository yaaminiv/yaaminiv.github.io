---
layout: post
comments: true
title: Final Virginica Tasks
tags: virginica maintenance IGV
---

## Tying up loose ends for the *C. virginica* gonad paper

Although I submitted the draft manuscript last month, I told myself I would finish up some small tasks for the *C. virginica* gonad methylation paper once I got back from vacation. I took care of those tasks over the past week!

The first thing I did was uploaded our submitted manuscript to bioRXiv. The paper, found [here](https://www.biorxiv.org/content/10.1101/2020.01.07.897934v1), is now a citable product! I also copied the entire repository into [this `gannet` folder](https://gannet.fish.washington.edu/spartina/paper-gonad-meth/). The only other thing I wanted to do was clean up [the paper repository](https://github.com/epigeneticstoocean/paper-gonad-meth). I thought it would just involve adding details to each directory README.md, but was I wrong. Converting a file dump to a usable repository — and chunking out small tasks to complete — took me about a week. It probably would have taken me less time if the work didn't put me to sleep :grimacing:

Steven posted issues for me to work on, so I started with those issues. I started by [updating the genome feature track README.md](https://github.com/epigeneticstoocean/paper-gonad-meth/issues/7). Before I could update the README, I wanted to make sure all of the genome feature tracks were represented. I cross-referenced the repository with [my large file folder on gannet](https://gannet.fish.washington.edu/spartina/2018-10-10-project-virginica-oa-Large-Files/). I moved over GFFs for each feature track to the repository, and updated the path to this directory in [this Jupyter notebook](https://github.com/epigeneticstoocean/paper-gonad-meth/blob/master/code/07-Generating-Genome-Feature-Tracks.ipynb). When updating the README, I included links to each feature track's GFF and the code used to generate that track.

Next I worked on [adding description to the code README.md](https://github.com/epigeneticstoocean/paper-gonad-meth/issues/9) (also referenced [here](https://github.com/epigeneticstoocean/paper-gonad-meth/issues/4)). Before I updated the code descriptions, I thought I'd check and see if all the code made sense. For all R Markdown files and Jupyter notebooks, I changed file paths to reflect the paper repository's structure. I also removed lots of redundant code, including those for generating DMR and downloading genome feature tracks. Since the paper repository has a separate folder for genome feature tracks, I didn't need to keep downloading them. Instead, I made sure the paths for the necessary genome feature tracks were updated to a relative path within the repository. I updated all header information and filenames for code so they accurately reflected what each script did. It's a good thing I did too, because some descriptions were confusing and filenames were completley incorrect. For example, one chunk of code said it was creating a "scaled DML distribution" instead of a "scaled methylated CpG distribution," causing me to have a mini freak-out because I thought I used the incorrect figure in my final paper (don't worry I did not use the wrong figure). I thought about the order the files would be used and changed it a few times before finalizing filenames and the README.

Even though it wasn't an explicit ask, I thought I would ensure the data README.md was also clear. Locally, I updated links and made sure the file structure made sense. When I tried to commit my changes, I realized that the entire data folder got added to the [.gitignore](https://github.com/epigeneticstoocean/paper-gonad-meth/blob/master/.gitignore)! I've [igored previously committed files](https://genefish.wordpress.com/2019/04/04/ignoring-previously-committed-files/) before, and I thought that fixing this would be similar. I couldn't find a workaround on Google, so I simply deleted my .gitignore, pushed [the data subdirectory](https://github.com/epigeneticstoocean/paper-gonad-meth/tree/master/data) to my cloud repository, and added the large files back into the .gitignore. An annoying, but easy, fix.

The last major task I had involved cleaning the [analyses subdirectory](https://github.com/epigeneticstoocean/paper-gonad-meth/tree/master/analyses). Steven noted that [all of the files in the repository weren't needed to reproduce the analyses in the paper](https://github.com/epigeneticstoocean/paper-gonad-meth/issues/8). Going through each subdirectory within analyses, I removed all files from defunct analyses. These files included anything with DMR, old versions of DML overlaps with various genome feature tracks, and anything that used the *C. gigas* transposable element track. I had to go back-and-forth between my lab notebook, the paper repository, and [my old repository](https://github.com/fish546-2018/yaamini-virginica) a few times to make sure I didn't remove files I needed and to add files I missed in my initial file dump. Once I felt like I removed most unecessary files, I started updating the [analyses README.md](https://github.com/epigeneticstoocean/paper-gonad-meth/tree/master/analyses). For each analysis subdirectory, I indicated which scripts the files came from, and highlighted important files with descriptions. As I went through each folder, I found even more redundant files that I no longer needed. For example, there were several GOterm annotation files that I never ended up using once I annotated GO-MWU output with GOSlim terms. I removed these files, as well as the lines of code used to generate them. Additionally, I still had code annotating differentially methylated genes with uncorrected p-values. Since we decided to only use results with corrected p-values, I didn't need the annotation files or associated code. I promptly removed these files and the code. Once I did this, I realized the only folder that used `blastx` to GOSlim output from [this code](https://github.com/epigeneticstoocean/paper-gonad-meth/blob/master/code/12-blastx-to-GOslim.ipynb) was the [gene enrichment analysis folder](https://github.com/epigeneticstoocean/paper-gonad-meth/blob/master/code/12-blastx-to-GOslim.ipynb). I moved these files from the [differentially methylated gene folder](https://github.com/epigeneticstoocean/paper-gonad-meth/tree/master/analyses/2019-08-14-Differentially-Methylated-Genes) to the gene enrichment folder, and changed all necessary file paths in other scripts.

While updating the analyses README.md, I realized I needed to update all file links in my IGV sessions. I first opened the [genome feature track IGV session](https://github.com/epigeneticstoocean/paper-gonad-meth/blob/master/genome-feature-tracks/2019-05-13-Genome-Track-Verification.xml). I was able to change the path to the genome on `gannet` in TextWrangler, but I couldn't change paths in TextWrangler without breaking the entire session. I painstakingly deleted and added new file URLs (there has to be a better way...) for the genome feature tracks on `gannet`. I wasn't able to display gffs for the intergenic, intron, or noncoding feature tracks, so I used BEDfiles for those. Since the BEDfiles weren't in my repository to begin with, I added them to the repository and to gannet. I also added the CG motif track to `gannet` and included a link in the genome feature track README.md. For the [DML verification IGV session](https://github.com/epigeneticstoocean/paper-gonad-meth/blob/master/analyses/2019-03-07-IGV-Verification/2019-03-07-DML-Visualization.xml), I used a path to the DML list within the repository. I also added the 5x sample bedgraphs to the repository and included repository paths for those files. The only files I had to use `gannet` links for were the *C. virginica* genome and CG motif track. I updated the analyses and genome feature track README.md files with information about the file links used for the IGV sessions.

After checking all the README.md files and repository one more time, I copied the entire local repository to `gannet`. That's the last of my tasks!

### Going forward

...just wait for reviewer comments I guess.

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
