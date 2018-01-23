---
layout: post
comments: true
title: Correlating Technical Replicates Part 10
tags: DNR SRM
---

## The end of a saga

I think I'm at a point where I kind of trust my [new dataset]() with bad samples removed. Within each peptide, all transitions show a consistent expression pattern between sites. Peptides within a protein, however, have different expression. This could be due to alternative splicing. 

There are three different patterns I saw with my protein expression data:

- Puget Sound sites had similar expression values, but Willapa Bay had higher expressoin
- There's a U-shape (CI --> FB --> PG (minimum) --> SK --> WB)
- All five expression values are similar

For my future reference, I'm going to paste my boxplots from one transition per peptide below. I've sorted proteins by their functions.

### Oxidative Stress (4 proteins)

**CHOYP_BRAFLDRAFT_119799.1.1|m.23765**: Peroxiredoxin-5, mitochondrial (EC 1.11.1.15)

![choyp_brafldraft_119799 1 1 m 23765 vnsgelfgk y4](https://user-images.githubusercontent.com/22335838/32288312-bfaa3c98-bef0-11e7-9a4d-22d295699f30.jpeg)

![choyp_brafldraft_119799 1 1 m 23765 vpavdlfek y6](https://user-images.githubusercontent.com/22335838/32288315-c04c54b0-bef0-11e7-8c6d-ca119abaa8e1.jpeg)

**CHOYP_BRAFLDRAFT_122807.1.1|m.3729**: Thioredoxin reductase 3 (EC 1.8.1.9)

![choyp_brafldraft_122807 1 1 m 3729 daiqdyigslnwgyr y8](https://user-images.githubusercontent.com/22335838/32288336-ceda3fd8-bef0-11e7-9c7e-2ec4194ed730.jpeg)

![choyp_brafldraft_122807 1 1 m 3729 igdymevegvk y7](https://user-images.githubusercontent.com/22335838/32288339-cf3284fe-bef0-11e7-9f12-01179817a930.jpeg)

![choyp_brafldraft_122807 1 1 m 3729 viglhvlgpnageitqgyavamr y5](https://user-images.githubusercontent.com/22335838/32288340-cf485b44-bef0-11e7-92d3-3d25c5360df3.jpeg)

**CHOYP_CATA.1.3|m.11120**: Catalase (EC 1.11.1.6)

![choyp_cata 1 3 m 11120 iqimtpeqaek y6](https://user-images.githubusercontent.com/22335838/32288401-0429afac-bef1-11e7-9d06-3cd8789834e7.jpeg)

![choyp_cata 1 3 m 11120 lqahldsvsnvsk y5](https://user-images.githubusercontent.com/22335838/32288403-04516006-bef1-11e7-92b9-e810b064ad8e.jpeg)

![choyp_cata 1 3 m 11120 lvenignhlintqk y4](https://user-images.githubusercontent.com/22335838/32288404-0469d2d0-bef1-11e7-80c0-96a0e292e8e7.jpeg)

**CHOYP_CATA.3.3|m.21642**: Catalase (EC 1.11.1.6)

![choyp_cata 3 3 m 21642 tdqgiqnlsaaeanr y7](https://user-images.githubusercontent.com/22335838/32288418-169a3d8c-bef1-11e7-8ca2-5446a4b8e236.jpeg)

### Heat shock (1 protein)

**CHOYP_HS12A.25.33|m.60352**: Heat shock 70 kDa protein 12B

![choyp_hs12a 25 33 m 60352 apttlllepdgk y4](https://user-images.githubusercontent.com/22335838/32288475-4b0b7fc2-bef1-11e7-8893-d4925f807059.jpeg)

![choyp_hs12a 25 33 m 60352 giaeaisssk y4](https://user-images.githubusercontent.com/22335838/32288476-4b242266-bef1-11e7-8f62-33c47652a1b8.jpeg)

### Acid-base balance (1 protein)

**CHOYP_CAH2.1.1|m.42306**: Carbonic anhydrase 2 (EC 4.2.1.1)

![choyp_cah2 1 1 m 42306 iqeagssvk y4](https://user-images.githubusercontent.com/22335838/32288374-ed658b1a-bef0-11e7-8608-b13aceb51700.jpeg)

![choyp_cah2 1 1 m 42306 naevsntgssik y7](https://user-images.githubusercontent.com/22335838/32288375-ed8950a4-bef0-11e7-8da1-a5dfeb477bef.jpeg)

![choyp_cah2 1 1 m 42306 qspidistk y4](https://user-images.githubusercontent.com/22335838/32288376-eda7a928-bef0-11e7-97b9-5385d06b6276.jpeg)

### Drug resistance (1 protein)

**CHOYP_MRP1.5.10|m.34368**: Multidrug resistance-associated protein 1 (ATP-binding cassette sub-family C member 1)

![choyp_mrp1 5 10 m 34368 dstvltiahr y4](https://user-images.githubusercontent.com/22335838/32288562-8c0aad90-bef1-11e7-8281-5de46c6a734a.jpeg)

![choyp_mrp1 5 10 m 34368 lyawepsfqdk y6](https://user-images.githubusercontent.com/22335838/32288563-8c21c9f8-bef1-11e7-824e-031196a8f4a7.jpeg)

### Fatty acid metabolism (1 protein)

**CHOYP_ACAA2.1.1|m.30666**: 3-ketoacyl-CoA thiolase, mitochondrial (EC 2.3.1.16)

![x elglnnditnmnggaialghplaasgtr y8](https://user-images.githubusercontent.com/22335838/32288654-d7750384-bef1-11e7-9a27-494e03931a94.jpeg)

![x itghlahelqr y5](https://user-images.githubusercontent.com/22335838/32288655-d79ff4b8-bef1-11e7-8c38-d5605c154aec.jpeg)

![x qynltplar y4](https://user-images.githubusercontent.com/22335838/32288656-d7b3daaa-bef1-11e7-830f-0dd90441ce23.jpeg)

### Carbohydrate metabolism (2 proteins)

**CHOYP_G6PD.2.2|m.46923**: Glucose-6-phosphate 1-dehydrogenase (G6PD) (EC 1.1.1.49)

![choyp_g6pd 2 2 m 46923 sdelyeawr y4](https://user-images.githubusercontent.com/22335838/32288425-2302e006-bef1-11e7-8e31-a3feb2d76f87.jpeg)

![choyp_g6pd 2 2 m 46923 tyfigyar y3](https://user-images.githubusercontent.com/22335838/32288427-23b49a80-bef1-11e7-9642-6ce4e5e020d7.jpeg)

**CHOYP_LOC100883864.1.1|m.41791**: Glycogen phosphorylase, muscle form (EC 2.4.1.1)

![choyp_loc100883864 1 1 m 41791 dyylslahtvr y5](https://user-images.githubusercontent.com/22335838/32288533-78dcdcde-bef1-11e7-91b1-a4b7de31036b.jpeg)

![choyp_loc100883864 1 1 m 41791 iedgwqveepdewlr y6](https://user-images.githubusercontent.com/22335838/32288534-78f07d02-bef1-11e7-915b-b88efbc0fb01.jpeg)

![choyp_loc100883864 1 1 m 41791 vlypndnffsgk y7](https://user-images.githubusercontent.com/22335838/32288535-7903b728-bef1-11e7-9967-1e534929fc87.jpeg)

### Cell growth and maintenance (5 proteins)

**CHOYP_BRAFLDRAFT_275870.1.1|m.12895**: Protein phosphatase 1B (EC 3.1.3.16)

![choyp_brafldraft_275870 1 1 m 12895 gmpemvsgedk y5](https://user-images.githubusercontent.com/22335838/32288352-dceb6214-bef0-11e7-96b7-5557ad74ebf8.jpeg)

![choyp_brafldraft_275870 1 1 m 12895 tgflqldek y4](https://user-images.githubusercontent.com/22335838/32288355-ddb659b0-bef0-11e7-8ea9-4cd3dcded829.jpeg)

**CHOYP_LOC100633041.1.1|m.35428**: NAD(P) transhydrogenase, mitochondrial (EC 1.6.1.2)

![choyp_loc100633041 1 1 m 35428 avveaannfgr y5](https://user-images.githubusercontent.com/22335838/32288503-5ea6cac8-bef1-11e7-91dd-1d391f90deec.jpeg)

![choyp_loc100633041 1 1 m 35428 sltnvilggygtk y7](https://user-images.githubusercontent.com/22335838/32288506-5f43983a-bef1-11e7-9b8f-30e55609104f.jpeg)

**CHOYP_PDIA1.1.1|m.5297**: Protein disulfide-isomerase (PDI) (EC 5.3.4.1)

![choyp_pdia1 1 1 m 5297 gsqqvvdynger y6](https://user-images.githubusercontent.com/22335838/32288595-a78b885a-bef1-11e7-9751-b6b88509a88d.jpeg)

![choyp_pdia1 1 1 m 5297 litlgedmtk y3](https://user-images.githubusercontent.com/22335838/32288596-a7b7a89a-bef1-11e7-9349-f66297d58e6d.jpeg)

![choyp_pdia1 1 1 m 5297 tspeivnwlr y5](https://user-images.githubusercontent.com/22335838/32288597-a7cb8e5a-bef1-11e7-86ad-46127e3ba41b.jpeg)

**CHOYP_PDIA3.1.1|m.60223**: Protein disulfide-isomerase A3 (EC 5.3.4.1)

![choyp_pdia3 1 1 m 60223 agefseeyggpr y7](https://user-images.githubusercontent.com/22335838/32288608-b10f26a2-bef1-11e7-91e9-a5e0b8d9e3f2.jpeg)

![choyp_pdia3 1 1 m 60223 iffavsnsk y4](https://user-images.githubusercontent.com/22335838/32288609-b1241170-bef1-11e7-8b49-96d153e0a886.jpeg)

![choyp_pdia3 1 1 m 60223 vadamstdlk y5](https://user-images.githubusercontent.com/22335838/32288610-b13878a4-bef1-11e7-8e4e-2cf4aa4088ae.jpeg)

**CHOYP_PSA.1.1|m.27259**: Puromycin-sensitive aminopeptidase (PSA) (EC 3.4.11.14)

![choyp_psa 1 1 m 27259 agmistvdvlk y3](https://user-images.githubusercontent.com/22335838/32288627-c097e636-bef1-11e7-91d5-7d312075df3f.jpeg)

![choyp_psa 1 1 m 27259 sendlpedstwk y4](https://user-images.githubusercontent.com/22335838/32288628-c0b13064-bef1-11e7-891b-789a9e2a2095.jpeg)

![choyp_psa 1 1 m 27259 yfqiayplpk y4](https://user-images.githubusercontent.com/22335838/32288629-c0c34b5a-bef1-11e7-9313-35a0eb51f83c.jpeg)

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
