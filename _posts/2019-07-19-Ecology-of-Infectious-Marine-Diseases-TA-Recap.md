---
layout: post
comments: true
title: Ecology of Infectious Marine Diseases TA Recap
tags: EIMD
---

## Things I did at FHL that were not my research

For the past five weeks, I've been at Friday Harbor Laboratories as a Teaching Assistant for the Ecology of Infectious Marine Diseases course! I thought it would be good to recap what I did, since I was working but didn't have time for my own research. In addition to finding lab equipment and helping out with fieldwork, I gave some lectures, helped students with a genomics project, and spearheaded the formal science communication section of the curriculum.

### Teaching molecular methods

For my first lecture, I taught students about Github and basic Linux commands. I had students navigate to [this Github repository](https://github.com/eimd-2019/tutorials) and clone it to their machine. I then walked through the steps I laid out in [this document](https://github.com/eimd-2019/tutorials/blob/master/2019-07-05-Github-Linux-Tutorial.md). I had students learn to change their working directory and navigate through directories using the command line interface. I opened up a Terminal window and placed a Finder under it so students could see how the commands I typed in the Terminal changed directory structure and files. I also emphasized the use of tab-complete to make things easier and avoid typos in code. The next set of commands I walked through involved downloading FASTA files from web links. Looking back on it now, I probably should have taught them about checksums, but I think their brains may have exploded a bit. I had them create new directories, move FASTA files into directories, and remove extra files. Finally, I went through commands to explore files from the command line.

Later that day, I taught them how to `blast` from the command line! I wanted them to type the code themselves, but we were unable to download `blast` on the computers in the Computer Lab. I walked through the code in [this document](https://github.com/eimd-2019/tutorials/blob/master/2019-07-09-Uniprot-blastx-Tutorial.md) so they could try reviewing it themselves later. What was more useful for them was going to the [Uniprot SwissProt](https://www.uniprot.org/uniprot/?query=reviewed:yes) database and teaching them about the database and how to get GOterms.

At the end of our genomics block, I gave a lecture on my own work! Since Colleen focused on transcriptomics, I used my work as case studies on the use of proteomics and epigenetics to study how abiotic stressors affect organismal physiology. Students were really interested in specific methods, so I added in a lot of detail. I think those extra details may have prevented some students from seeing the big picture. I'll need to work on that if I give that lecture again.

A large component of the course was working on projects. For the project [examining NIX prevalence in Kalaloch Beach razor clams](https://github.com/eimd-2019/NIX-project), I assisted students with DNA extractions and qPCR. I spent most of my time assisting with the [transcriptomics project looking at eelgrass wasting disease host-pathogen interactions](https://github.com/eimd-2019/project-EWD-transcriptomics). I created [this Jupyter notebook](https://github.com/eimd-2019/project-EWD-transcriptomics/blob/master/scripts/2019-07-10-blastx-Uniprot-File-Merging.ipynb) to merge transcriptomic data with `blast` output and Uniprot Swiss-Prot annotations. I also streamlined isoforms into genes. In [this R Markdown file](https://github.com/eimd-2019/project-EWD-transcriptomics/blob/master/analyses/GO-MWU/2019-07-11-Gene-Enrichment-with-GO-MWU.md), I formatted input files for gene enrichment with [GO-MWU](https://github.com/z0on/GO_MWU). I used the GO-MWU pipeline for eelgrass and pathogen transcriptomic data, but only found two enriched GOterms for eelgrass. I also created [this R Markdown document](https://github.com/eimd-2019/project-EWD-transcriptomics/blob/master/analyses/EdgeR/2019-07-15-Gene-Expression-Heatmaps.md) to help students create heatmaps of differentially expressed genes. They used the code I created to create their own heatmaps for genes of interest. I think any molecular method is difficult to follow if you have little to no experience, and if you don't have a great understanding of R or Linux. One thing that (I think) helped while I was teaching was to constantly remind students the purpose of each step.

### Science communication

The other part of the course I helped with was science communication practice. Students were required to write one blog post for a public audience and create a short talk about a disease for the class. I worked with each student on their blog post and provided targetted feedback when editing their initial drafts. I also had students give practice presentations to me so I could help them if they needed. Most of my comments were directed at improving the organization of their pieces or talks. I took it upon myself to help them outline their final papers and reviewed their final presentations so they would not have similar issues.

Overall TAing at FHL was a lot of work but really rewarding! I learned a lot about what it means to be a good instructor and I cannot wait to flex those skills soon.

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
