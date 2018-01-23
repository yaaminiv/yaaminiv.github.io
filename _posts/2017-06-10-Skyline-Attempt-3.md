---
layout: post
comments: true
title: Skyline Attempt 3
---

## Remember that Skyline fail? 

We think it's fixed! Based on my [previous error rates](https://yaaminiv.github.io/Another-Skyline-Fail/), Emma and her team were able to figure out that brecan (a version of `pecanpie` produced by Brian Searle in Genome Sciences) "produces blib files that have a certain number of incompatibilities with Skyline (one of which is redundant peptide identifications). Jarrett Egertson wrote a script to get rid of these incompatibilities so that the brecan blib files work," (Timmins-Schiffman, pers. comm.). I saved the .blib file [here in OWL](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170524/2017-05-23-oyster-desearleinated.blib).

Using Skyline document settings from my [test document](https://yaaminiv.github.io/Skyline-Test-2/) and [first full Skyline attempt](https://yaaminiv.github.io/Full-Skyline-Analysis/), and results specifications from my [first try at SRM target selection](https://yaaminiv.github.io/Selecting-SRM-Targets/), I created a new Skyline document.

### Step 1: Peptide settings

I used the proteome file from my [Gigas-4-27 document](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170505/Gigas-4-27-oyster1-test.sky.zip):

![image-1](https://user-images.githubusercontent.com/22335838/27004381-482f26dc-4dbd-11e7-9c36-6db75023d19f.png)

![image-2](https://user-images.githubusercontent.com/22335838/27004382-48340986-4dbd-11e7-989d-911df53055bd.png)

![image-3](https://user-images.githubusercontent.com/22335838/27004380-482c53f8-4dbd-11e7-86ba-1fada1a47626.png)

![image-4](https://user-images.githubusercontent.com/22335838/27004378-482bbeac-4dbd-11e7-9375-7d6890054dc5.png)

Added the new "desearlinated" .blib:

![image-5](https://user-images.githubusercontent.com/22335838/27004379-482bd630-4dbd-11e7-8e1c-8396145c14c4.png)

![image-6](https://user-images.githubusercontent.com/22335838/27004383-4840a736-4dbd-11e7-9070-de3053540497.png)

![image-7](https://user-images.githubusercontent.com/22335838/27004384-48427ac0-4dbd-11e7-92bf-8d4d5595d157.png)

### Step 2: Import FASTA file

File >> Import >> FASTA >> Combined-gigas-QC.txt

### Step 3: Transition settings

![image-8](https://user-images.githubusercontent.com/22335838/27004485-bb7d5946-4dbe-11e7-99c9-f10772f9c2dc.png)

![image-9](https://user-images.githubusercontent.com/22335838/27004481-bb69e28a-4dbe-11e7-9d90-34a83d6731ea.png)

![image-10](https://user-images.githubusercontent.com/22335838/27004484-bb6c570e-4dbe-11e7-8320-7d35434887fa.png)

![image-11](https://user-images.githubusercontent.com/22335838/27004483-bb6c3486-4dbe-11e7-9c68-0bd3dd7469c1.png)

![image-12](https://user-images.githubusercontent.com/22335838/27004482-bb6b5b56-4dbe-11e7-8c80-fd8927009460.png)

### Step 4: Import demultiplexed .mzmL files

![image-13](https://user-images.githubusercontent.com/22335838/27004495-e73e1462-4dbe-11e7-9650-c0a3ef39fa84.png)

![image-14](https://user-images.githubusercontent.com/22335838/27004498-e742c70a-4dbe-11e7-9dd8-3fdb06ac13e3.png)

![image-16](https://user-images.githubusercontent.com/22335838/27004497-e7427fa2-4dbe-11e7-9061-643f0d53c7cd.png)

I didn't upload files 17, 21, 22 and 23 because oyster 21 was my blank (so there should be no data in it!) and oyster 17, 22 and 23 corresponded with my original 0107 sample had a bubble and did not read properly in the mass spec.

![image-17](https://user-images.githubusercontent.com/22335838/27004500-e7430350-4dbe-11e7-94a3-7fd5c23ff2a9.png)

![image-18](https://user-images.githubusercontent.com/22335838/27004499-e7431fac-4dbe-11e7-898f-7d4909e54729.png)

![image-19](https://user-images.githubusercontent.com/22335838/27004496-e741535c-4dbe-11e7-9f8e-8bc2559d9c8e.png)

Once my data files finish loading, I'm going to start error checking again to see how much of a difference the new .blib made.

### Step 5: Add Condition and BioReplicate information

Based on information from [this lab notebook](https://github.com/RobertsLab/project-oyster-oa/blob/master/notebooks/2017-05-12-Selecting-SRM-Targets-with-MSstats.ipynb).

![image-1](https://user-images.githubusercontent.com/22335838/27007800-8b58b1d8-4e14-11e7-88cb-b3d9405ca0e7.png)

![image-2](https://user-images.githubusercontent.com/22335838/27007798-8b54dd10-4e14-11e7-8362-2496d1a2f3f4.png)

![image-3](https://user-images.githubusercontent.com/22335838/27007799-8b56f2b2-4e14-11e7-9596-2d4bdda2edfa.png)

![image-4](https://user-images.githubusercontent.com/22335838/27007797-8b538d34-4e14-11e7-88ad-921b7b4f009d.png)

### Step 6: Export Skyline report three separate ways

![image-5](https://user-images.githubusercontent.com/22335838/27007796-8b5379b6-4e14-11e7-98de-37bacee7af15.png)

#### Just proteins and peak areas, with replicate pivoted

![image-1](https://user-images.githubusercontent.com/22335838/27007812-bd170a6c-4e14-11e7-896a-8f1c0728601b.png)

File found on OWL [here](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170524/2017-06-10-protein-areas-only.csv)

#### For MSstats

Edit "Transition Results" >> "Transition Results"

![image-1](https://user-images.githubusercontent.com/22335838/27007815-06d7b778-4e15-11e7-8004-b36e4e680845.png)

![image-2](https://user-images.githubusercontent.com/22335838/27007816-06d86ce0-4e15-11e7-8b6e-5cff70e4a3c3.png)

Replicate pivoted:

![image-3](https://user-images.githubusercontent.com/22335838/27007818-0f142a84-4e15-11e7-9aa3-a7dcc4e125cf.png)

File found [here](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170524/2017-06-10-peptide-transition-results-MSstats.csv). This was actually a mistake, but I saved it anyways.

Replicate not pivoted:

![image-4](https://user-images.githubusercontent.com/22335838/27007819-0f174ce6-4e15-11e7-8a97-dd3fa7d6362b.png)

File found [here](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170524/2017-06-10-peptide-transition-results-MSstats-no-pivot.csv). This is what I can use for MSstats.

Now I'm ready to error check.

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
