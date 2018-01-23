---
layout: post
comments: true
title: Remaining Analyses Part 2
tags: DNR SRM
---

## Everything at the peptide level

Steven suggested that I integrate my transition data and work only at the peptide level. To do this, I went back into Skyline and created a [duplicate Skyline document](http://owl.fish.washington.edu/spartina/DNR_SRM_20170728/Analyses/2017-11-05-Gigas-SRM-Good-Samples.zip). I removed the technical replicates for samples I [previously deemed poor quality]().

Under the Skyline settings, I selected "Integrate All."

![unnamed-1](https://user-images.githubusercontent.com/22335838/32456289-eefd0aec-c2d9-11e7-8c38-7c1280d0ff6d.png)

**Figure 1**. "Integrate All" settings found in Skyline.

When exporting my report, I added a "Total Area" column. This can be found under "Precursor Settings." It sums all of the area data for each transition associated with a precursor, eessentially integrating all the area data I have for each peptide.

![unnamed-3](https://user-images.githubusercontent.com/22335838/32456342-1bdcd2a4-c2da-11e7-9e17-fea3b9b4b49d.png)

![unnamed-2](https://user-images.githubusercontent.com/22335838/32456344-1cf93a92-c2da-11e7-946d-b70510c4e9a7.png)

**Figures 2-3**. "Total Area" data to export from Skyline.

I exported the data, which can be found [here](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-Gigas-SRM-Good-Samples-Total-Area.csv).

I then examined the technical replication of my samples [in this R script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-for-Technical-Replication.R). Again, some of my samples had better technical replication than others, but this looks like an improvement...? There were smaples with large ordination distances, so I wonder if removing those samples would be a good place to dive into my analyses.

![tech-rep-NMDS](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-TechnicalReplication-Normalized.jpeg)

![ord-dist](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-TechnicalReplication-Ordination-Distances.jpeg)

**Figures 4-5**. NMDS for technical replication and ordination distances between replicates.

I wrote out the [area data for my technical replicates](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-Technical-Replicates-Pivoted.csv) and fed it into [this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-ANOSIM-for-Cluster-Analysis.R) to average my replicates and perform cluster analyses.

![ave-NMDS](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-Analysis-Averaged.jpeg)

**Figure 6**. NMDS using all peptide areas for all samples. No significant clustering patterns were found based on site, eelgrass, or both criteria.

No patterns really leapt out at me, but I could more easily pick out Skokomish River Delta and Willapa Bay from the other sites. I wonder if there's some sort of regional effect here? Either way, here are my ANOSIM results:

**Site only**: R = 0.0504, Significance = 0.105 
**Eelgrass/Bare only**: R = 0.05031, Significance = 0.09 
**Both**: R = 0.05664, Significance = 0.169 

Based on Laura's suggestion, I also made NMDS plots grouping proteins together by function [in this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-ANOSIM-for-Individual-Proteins.R). None of them really looked interesting except for the oxidative stress and growth and maintenance plots, but I didn't jot down the ANOSIM results since they weren't significant at the site or eelgrass level.

![oxstress](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-Analysis-Averaged-Oxidative-Stress.jpeg)

**Figure 7**. Oxidative stress NMDS (4 proteins).

![hs](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-Analysis-Averaged-HeatShock.jpeg)

**Figure 8**. Heat shock NMDS (1 protein).

![acid-base](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-Analysis-Averaged-AcidBase.jpeg)

**Figure 9**. Acid-base balance NMDS (1 protein).

![drug-resistance](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-Analysis-Averaged-DrugResistance.jpeg)

**Figure 10**. Drug resistance NMDS (1 protein).

![fatty-acid](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-Analysis-Averaged-FattyAcid.jpeg)

**Figure 11**. Fatty acid metabolism NMDS (1 protein).

![carb](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-Analysis-Averaged-Carbohydrate.jpeg)

**Figure 12**. Carbohydrate metabolism NMDS (2 proteins).

![growth](https://raw.githubusercontent.com/RobertsLab/project-oyster-oa/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-05-NMDS-Analysis-Averaged-GrowthandMaintenance.jpeg)

**Figure 13**. Cell growth and maintenance NMDS (5 proteins).

Finally, I made boxplots for each peptide characterizing protein expression [with this script](https://github.com/RobertsLab/project-oyster-oa/blob/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-06-Boxplots/2017-11-06-Protein-Area-Boxplots-after-Integration.R). I only made the boxplots using site distinctions since my NMDS and ANOSIM analyses showed no significant differences in clustering based on habitat. Boxplots can be found [in this folder](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_SRM_20170902/2017-10-10-Troubleshooting/2017-11-05-Integrated-Dataset/2017-11-06-Boxplots). I found these boxplots to be less interesting than the ones I made [at the transition level](https://yaaminiv.github.io/Correlating-Technical-Replicates-Part10/).

Now I think I need to start looking into the significance of my protein expression by performing some t-tests or an ANOVA.

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
