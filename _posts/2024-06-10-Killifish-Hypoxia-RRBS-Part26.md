---
layout: post
comments: true
title: Killifish Hypoxia RRBS Part 26
tags: killifish RRBS
---

## General methylation landscape figure

Last time I met with Neel, we decided that the general methylation figure should include a circos plot of methylation, something to visualize the proportion of methylated and unmethylated CpGs, and perhaps a methylation density plot. So I returned to [this R Markdown script](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-methylation-landscape-analysis.Rmd) to modify existing figures.

### Circos plot

The first thing I did was modify the circos plot. Neel suggested I only visualize chromosomes with interesting genes, like HIF-1a and AHR. I downloaded the [CDS file from NCBI](https://www.ncbi.nlm.nih.gov/datasets/genome/GCF_011125445.2/) and used `grep` to search for my genes of interest:

```
grep "hypoxia" ../../../../genome-features/GCF_011125445.2_MU-UCD_Fhet_4.1_cds_from_genomic.fna
```

>lcl|NC_046378.1_cds_XP_036006209.1_31529 [gene=hif1al] [db_xref=GeneID:105930192] [protein=hypoxia inducible factor 1 subunit alpha, like isoform X2] [protein_id=XP_036006209.1] [location=join(36151289..36151326,36161662..36161852,36162897..36163060,36163327..36163411,36165555..36165670,36166886..36167070,36167162..36167268,36168221..36168368,36168467..36168720,36170263..36170420,36170513..36170608,36171397..36171560,36172882..36172993,36173587..36173701,36174827..36174894)] [gbkey=CDS]
>lcl|NC_046378.1_cds_XP_036006208.1_31530 [gene=hif1al] [db_xref=GeneID:105930192] [protein=hypoxia inducible factor 1 subunit alpha, like isoform X1] [protein_id=XP_036006208.1] [location=join(36157155..36157207,36161662..36161852,36162897..36163060,36163327..36163411,36165555..36165670,36166886..36167070,36167162..36167268,36168221..36168368,36168467..36168720,36170263..36170420,36170513..36170608,36171397..36171560,36172882..36172993,36173587..36173701,36174827..36174894)] [gbkey=CDS]
>lcl|NC_046379.1_cds_XP_012719971.2_31930 [gene=hif1aa] [db_xref=GeneID:105927646] [protein=hypoxia inducible factor 1 subunit alpha a] [protein_id=XP_012719971.2] [location=complement(join(6741545..6741708,6741793..6741883,6741975..6742074,6742716..6742975,6743279..6743401,6743482..6743684,6743772..6744094,6744234..6744381,6744478..6744584,6745891..6746087,6747304..6747413,6747631..6747715,6748601..6748752,6749712..6749902,6757314..6757345))] [gbkey=CDS]
>lcl|NC_046382.1_cds_XP_012736020.1_38087 [gene=hif1an] [db_xref=GeneID:105938712] [protein=hypoxia-inducible factor 1-alpha inhibitor] [protein_id=XP_012736020.1] [location=complement(join(24161907..24161963,24162259..24162369,24162468..24162531,24163412..24163518,24163616..24163761,24164789..24164937,24165352..24165602,24171950..24172129))] [gbkey=CDS]
>lcl|NW_023396522.1_cds_XP_012712373.1_41591 [gene=hyou1] [db_xref=GeneID:105921221] [protein=hypoxia up-regulated protein 1 isoform X1] [protein_id=XP_012712373.1] [location=join(30052..30118,30289..30382,30496..30574,30712..30866,35661..35737,35828..36009,36112..36227,36308..36494,37565..37699,37880..37962,38061..38193,38513..38697,39511..39649,40015..40072,40411..40487,40629..40781,40884..41035,41113..41200,41300..41556,44217..44301,46094..46355,48023..48133,50258..50307)] [gbkey=CDS]
>lcl|NW_023396522.1_cds_XP_012712374.1_41592 [gene=hyou1] [db_xref=GeneID:105921221] [protein=hypoxia up-regulated protein 1 isoform X2] [protein_id=XP_012712374.1] [location=join(30052..30118,30289..30382,30496..30574,30712..30866,35661..35737,35828..36009,36112..36227,36308..36494,37565..37699,37880..37962,38061..38193,38513..38697,39511..39649,40015..40072,40411..40487,40629..40781,40884..41035,41113..41200,41300..41556,44217..44301,46094..46355,48026..48133,50258..50307)] [gbkey=CDS]
>lcl|NW_023396522.1_cds_XP_035984586.1_41593 [gene=hyou1] [db_xref=GeneID:105921221] [protein=hypoxia up-regulated protein 1 isoform X3] [protein_id=XP_035984586.1] [location=join(30052..30118,30289..30382,30496..30574,30712..30866,35661..35737,35828..36009,36112..36227,36308..36494,37565..37699,37880..37962,38061..38193,38513..38697,39511..39649,40015..40072,40411..40487,40629..40781,40884..41035,41113..41200,41300..41556,44217..44301,44481..44624)] [gbkey=CDS]

```
grep "aryl hydrocarbon receptor" ../../../../genome-features/GCF_011125445.2_MU-UCD_Fhet_4.1_cds_from_genomic.fna
```

>lcl|NC_046362.1_cds_XP_036006965.1_3310 [gene=arntl2] [db_xref=GeneID:105935564] [protein=aryl hydrocarbon receptor nuclear translocator-like protein 2 isoform X2] [protein_id=XP_036006965.1] [location=complement(join(22407360..22407517,22410173..22410272,22417685..22417775,22419825..22419936,22421281..22421351,22421457..22421607,22421810..22421916,22423771..22424014,22424098..22424247,22424352..22424469,22427375..22427454,22428501..22428658,22428748..22428789,22428883..22428936,22429022..22429145,22441109..22441175))] [gbkey=CDS]
>lcl|NC_046362.1_cds_XP_036006960.1_3311 [gene=arntl2] [db_xref=GeneID:105935564] [protein=aryl hydrocarbon receptor nuclear translocator-like protein 2 isoform X1] [protein_id=XP_036006960.1] [location=complement(join(22407360..22407517,22410173..22410272,22417685..22417775,22419825..22419936,22421281..22421351,22421457..22421607,22421810..22421916,22423771..22424014,22424098..22424247,22424352..22424469,22427070..22427162,22427375..22427454,22428501..22428658,22428748..22428789,22428883..22428936,22429022..22429145,22441109..22441175))] [gbkey=CDS]
>lcl|NC_046362.1_cds_XP_036006969.1_3312 [gene=arntl2] [db_xref=GeneID:105935564] [protein=aryl hydrocarbon receptor nuclear translocator-like protein 2 isoform X3] [protein_id=XP_036006969.1] [location=complement(join(22407360..22407517,22410173..22410272,22417685..22417775,22419825..22419936,22421281..22421351,22421457..22421607,22421810..22421916,22423771..22424014,22424098..22424247,22424352..22424469,22427070..22427162,22427375..22427454,22428501..22428658,22428748..22428789,22428883..22428936,22429022..22429074))] [gbkey=CDS]
>lcl|NC_046362.1_cds_XP_035980657.1_3571 [gene=arntl1a] [db_xref=GeneID:105929688] [protein=aryl hydrocarbon receptor nuclear translocator-like 1a] [protein_id=XP_035980657.1] [location=complement(join(30555349..30555506,30555616..30555709,30557384..30557477,30560492..30560600,30560716..30560795,30560903..30561053,30561144..30561250,30563571..30563817,30569859..30570008,30570479..30570596,30571525..30571617,30571723..30571802,30574598..30574755,30575167..30575196,30576790..30576822,30577024..30577044,30577582..30577729,30579954..30579999))] [gbkey=CDS]
>lcl|NC_046363.1_cds_XP_012721228.2_5092 [gene=arnt] [db_xref=GeneID:105928492] [protein=aryl hydrocarbon receptor nuclear translocator isoform X1] [protein_id=XP_012721228.2] [location=join(26531048..26531084,26531722..26531812,26531932..26531970,26533817..26533867,26534744..26534957,26539140..26539353,26539441..26539543,26539649..26539714,26539833..26539918,26540046..26540122,26542423..26542557,26542646..26542720,26544287..26544438,26544664..26544783,26545221..26545314,26550527..26550708,26552470..26552659,26552761..26552929,26555868..26556001,26556090..26556173)] [gbkey=CDS]
>lcl|NC_046363.1_cds_XP_012721229.2_5093 [gene=arnt] [db_xref=GeneID:105928492] [protein=aryl hydrocarbon receptor nuclear translocator isoform X2] [protein_id=XP_012721229.2] [location=join(26531048..26531084,26531722..26531812,26531932..26531970,26533817..26533867,26534744..26534957,26539140..26539353,26539441..26539543,26539649..26539714,26539833..26539918,26540046..26540122,26542423..26542557,26542646..26542720,26544287..26544438,26544667..26544783,26545221..26545314,26550527..26550708,26552470..26552659,26552761..26552929,26555868..26556001,26556090..26556173)] [gbkey=CDS]
>lcl|NC_046363.1_cds_XP_012721230.2_5094 [gene=arnt] [db_xref=GeneID:105928492] [protein=aryl hydrocarbon receptor nuclear translocator isoform X3] [protein_id=XP_012721230.2] [location=join(26531048..26531084,26531722..26531812,26531932..26531970,26533817..26533867,26534744..26534957,26539140..26539353,26539441..26539543,26539649..26539714,26539833..26539918,26540046..26540122,26542423..26542557,26542646..26542720,26544287..26544438,26545221..26545314,26550527..26550708,26552470..26552659,26552761..26552929,26555868..26556001,26556090..26556173)] [gbkey=CDS]
>lcl|NC_046367.1_cds_NP_001296878.1_11608 [gene=LOC105937232] [db_xref=GeneID:105937232] [protein=aryl hydrocarbon receptor-like] [exception=annotated by transcript or proteomic data] [protein_id=NP_001296878.1] [location=complement(join(823651..823800,835471..837010,837198..837339,839479..839588,839771..839973,846200..846333,849308..849440,866350..866439,882312..882415,904092..904279,920977..921017))] [gbkey=CDS]
>lcl|NC_046367.1_cds_NP_001296875.1_11610 [gene=LOC105937229] [db_xref=GeneID:105937229] [protein=aryl hydrocarbon receptor-like] [exception=annotated by transcript or proteomic data] [protein_id=NP_001296875.1] [location=complement(join(939102..939161,941815..943474,943585..943726,945621..945730,946008..946210,946287..946414,946691..946805,950669..950758,952485..952582,957066..957256,972737..972795))] [gbkey=CDS]
>lcl|NC_046367.1_cds_XP_035994536.1_11611 [gene=LOC105937229] [db_xref=GeneID:105937229] [protein=aryl hydrocarbon receptor-like isoform X1] [protein_id=XP_035994536.1] [location=complement(join(939102..939164,941815..943474,943585..943726,945621..945730,946008..946210,946287..946414,946691..946805,950669..950758,952485..952582,957066..957256,972737..972795))] [gbkey=CDS]
>lcl|NC_046381.1_cds_NP_001296891.1_36523 [gene=ahrr] [db_xref=GeneID:105920361] [protein=aryl hydrocarbon receptor repressor] [exception=annotated by transcript or proteomic data] [protein_id=NP_001296891.1] [location=join(32860813..32860874,32862421..32862602,32940396..32940514,32945110..32945199,32951622..32951766,32951857..32951990,32953265..32953467,32957742..32958849)] [gbkey=CDS]
>lcl|NC_046381.1_cds_XP_035980658.1_36524 [gene=ahrr] [db_xref=GeneID:105920361] [protein=aryl hydrocarbon receptor repressor isoform X1] [protein_id=XP_035980658.1] [location=join(32880630..32880691,32882241..32882422,32940396..32940514,32945110..32945199,32951622..32951766,32951857..32951990,32953265..32953467,32957742..32958849)] [gbkey=CDS]
>lcl|NC_046384.1_cds_XP_035983895.1_40510 [gene=ahr2] [db_xref=GeneID:105917025] [protein=aryl hydrocarbon receptor 2] [exception=annotated by transcript or proteomic data] [protein_id=XP_035983895.1] [location=join(6267796..>6267854,<6285138..6285286,6285363..6285452,6289984..6290098,6292187..6292326,6292407..6292609,6292686..6292795,6294695..6294836,6294916..6296371,6297348..6297434)] [gbkey=CDS]
>lcl|NC_046384.1_cds_XP_012707145.2_40511 [gene=LOC105917026] [db_xref=GeneID:105917026] [protein=aryl hydrocarbon receptor] [protein_id=XP_012707145.2] [location=join(6301924..6301976,6305081..6305268,6310884..6310996,6313820..6313909,6321263..6321386,6321686..6321816,6326309..6326508,6327340..6327449,6329662..6329803,6329887..6330961,6338618..6338776)] [gbkey=CDS]
>lcl|NW_023396740.1_cds_XP_035986196.1_43580 [gene=LOC118559655] [db_xref=GeneID:118559655] [protein=aryl hydrocarbon receptor nuclear translocator 2-like isoform X1] [protein_id=XP_035986196.1] [location=join(71139..71169,103218..103329,106238..106285,110630..110677,111722..111935,132377..132590,141016..141152)] [gbkey=CDS]
>lcl|NW_023396740.1_cds_XP_035986197.1_43581 [gene=LOC118559655] [db_xref=GeneID:118559655] [protein=aryl hydrocarbon receptor nuclear translocator 2-like isoform X2] [protein_id=XP_035986197.1] [location=join(71139..71169,103218..103329,106238..106285,111722..111935,132377..132590,141016..141152)] [gbkey=CDS]
>lcl|NW_023396923.1_cds_XP_021165767.2_46206 [gene=LOC105916972] [db_xref=GeneID:105916972] [protein=aryl hydrocarbon receptor nuclear translocator-like protein 2] [protein_id=XP_021165767.2] [location=join(1195161..1195203,1230317..1230413,1230571..1230609,1230690..1230731,1230844..1231001,1231105..1231187,1232937..1233029,1236475..1236592,1236686..1236835,1240019..1240256,1240679..1240785,1240908..1241058,1241138..1241193,1241414..1241525,1242376..1242454,1242585..1242684,1245427..1245575)] [gbkey=CDS]
>lcl|NW_023397000.1_cds_NP_001296912.1_47107 [gene=arnt2] [db_xref=GeneID:105921182] [protein=aryl hydrocarbon receptor nuclear translocator 2] [frame=3] [partial=5'] [transl_except=(pos:22..24,aa:Gln)] [exception=annotated by transcript or proteomic data] [protein_id=NP_001296912.1] [location=complement(join(1537052..1537150,1537227..1537372,1538831..1538981,1540823..1540961,1541079..1541181,1541277..1541400,1546887..1546959,1552652..1552803,1557006..1557080,1559057..1559191,1560721..1560797,1563207..1563292,1568627..1568692,1575716..1575818,1584224..1584441,1584443..>1584464))] [gbkey=CDS

Based on these results, I decided to plot chromosomes NC_046379.1 and NC_046384.1. I took the data with all common loci across samples and filtered it down to just my chromosomes of interest for each population x treatment group:

```
#Select relevant columns and filter out the two chromosomes with HIF and AHR.

NavgMethOCCircos <- avgMethCommonLoci %>%
  select(., chr, pos, NBH.OC.average) %>%
  filter(chr == c("NC_046379.1", "NC_046384.1"))
NavgMethNOCircos <- avgMethCommonLoci %>%
  select(., chr, pos, NBH.NO.average) %>%
  filter(chr == c("NC_046379.1", "NC_046384.1"))
NavgMethHYCircos <- avgMethCommonLoci %>%
  select(., chr, pos, NBH.HY.average) %>%
  filter(chr == c("NC_046379.1", "NC_046384.1"))
```

I then created the circos plot! Thankfully I had already done a lot of work creating the circos plot. I changed the track height to accomodate all six data tracks. I also *finally* looked at the response to [this Github issue](https://github.com/jokergoo/circlize/issues/345). [Previously](https://yaaminiv.github.io/Killifish-Hypoxia-RRBS-Part22/), I had trouble with data plotting outside of the defined chromosome region. Turns out, the chromosome length needs to be redefined for each new chromosome. Using the suggestion in the Github issue, I was able to fix my plotting issue!

```
pdf("N-S-circos.pdf", width = 11, height = 8.5)

circos.clear() #Create a blank circos plot
col_text <- "grey20" #Add text color
circos.par(gap.degree = 10,
           cell.padding = c(0, 0, 0, 0)) #par information for plot
circos.initialize(factors = c("NC_046379.1", "NC_046384.1"),
                  xlim = matrix(c(rep(0, 2),
                                  33976892, 28860465),
                                ncol = 2)) #Initialize circos

# Add chromosome delinations
circos.track(ylim = c(0, 1), panel.fun = function(x, y) {
  chr = CELL_META$sector.index
  xlim = CELL_META$xlim
  ylim = CELL_META$ylim
  circos.text(mean(xlim), mean(ylim), chr, cex = 0.75, col = col_text,
              facing = "bending.inside", niceFacing = TRUE)
}, bg.col = "grey90", bg.border = FALSE, track.height = 0.06)

# Genome x axis
brk <- c(0, 0.5, 1, 1.5, 2, 2.5, 3, 3.5)*10^7 #Set axis breaks
circos.track(track.index = get.current.track.index(), panel.fun = function(x, y) {
  circos.axis(h = "top", major.at = brk, labels = round(brk/10^7, 1), labels.cex = 1,
              col = col_text, labels.col = col_text, lwd = 1, labels.facing = "clockwise")
}, bg.border = FALSE)

# NBH Outside Control
circos.track(factors = NavgMethOCCircos$chr, ylim = c(0,1), panel.fun=function(x, y) {
  l = NavgMethOCCircos$chr == CELL_META$sector.index
  value = NavgMethOCCircos$NBH.OC.average[l]
  pos = NavgMethOCCircos$pos[l]
  circos.barplot(value, pos,
                 border = plotColors[1], col = plotColors[1])
}, track.height = 0.13, bg.border = FALSE) #Add a barplot specifying chr as a factor (sector), and ylim. Add mean as values for barplot and start bp as pos.

circos.yaxis(at = c(0, 0.5, 1),
             labels.cex = 1, lwd = 1, tick.length = 1, labels.col = col_text, col = "grey20") #Add y-axis
```

### Methylated vs. unmethylated CpGs

Once I had my circos plot, I decided I needed to make the other two plots using data from all CpGs with 10x coverage, not just those common across all samples. I returned to [this Jupyter notebook](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/code/05-methylation-landscape-analysis.ipynb) to average my 10x union bedgraph by treatments and populations:

```
#Average all NBH samples and NBH x treatment samples
#NA are not included in averages
#Check output
df['NBH.average'] = df[['N_20-N4', 'N_5-N1', 'N_5-N2', 'N_20-N2', 'N_5-N3', 'N_20-N1', 'N_OC-N5', 'N_OC-N1', 'N_OC-N2', 'N_OC-N4']].mean(axis=1)
df['NBH.OC.average'] = df[['N_OC-N5', 'N_OC-N1', 'N_OC-N2', 'N_OC-N4']].mean(axis=1)
df['NBH.NO.average'] = df[['N_20-N4', 'N_20-N2', 'N_20-N1']].mean(axis=1)
df['NBH.HY.average'] = df[['N_5-N1', 'N_5-N2', 'N_5-N3']].mean(axis=1)

#Average all SC samples and SC x treatment samples
#NA are not included in averages
df['SC.average'] = df[['S_20-S1', 'S_20-S3', 'S_20-S4', 'S_5-S3', 'S_5-S4', 'S_5-S2', 'S_20-S2', 'S_5-S1', 'S_OC-S1', 'S_OC-S2', 'S_OC-S3', 'S_OC-S5']].mean(axis=1)
df['SC.OC.average'] = df[['S_OC-S1', 'S_OC-S2', 'S_OC-S3', 'S_OC-S5']].mean(axis=1)
df['SC.NO.average'] = df[['S_20-S1', 'S_20-S3', 'S_20-S4', 'S_20-S2']].mean(axis=1)
df['SC.HY.average'] = df[['S_5-S3', 'S_5-S4', 'S_5-S2', 'S_5-S1']].mean(axis=1)
```

I wrote out the new file [here](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/05-analysis/new-genome/methylation-landscape/missing_1/all-samples-averages-union.bedgraph) then imported it into my R Markdown script.

```
avgMethTreatLong <- avgMethUnion %>%
  select(., chrom, start,
         NBH.OC.average, NBH.NO.average, NBH.HY.average,
         SC.OC.average, SC.NO.average, SC.HY.average) %>%
  pivot_longer(., cols = 3:8, names_to = "treatment") %>%
  mutate(., population = gsub(x = treatment, pattern = ".OC.average", replacement = "")) %>%
  mutate(., population = gsub(x = population, pattern = ".NO.average", replacement = "")) %>%
  mutate(., population = gsub(x = population, pattern = ".HY.average", replacement = "")) #Take union data and select relevant columns. Pivot longer so treatment x population information is in a separate column. Create a new column with population using a series of gsub commands.
```

I then modified this further using a series of summarize commands to count the total number of CpGs with data, number of methylated CpGs, and number of unmethylated CpGs...

```
stackedBarplotData <- avgMethTreatLong %>%
  group_by(., treatment) %>%
  summarize(.,
            total = sum(is.na(value) == FALSE),
            methylated = sum(value > 0, na.rm = TRUE),
            unmethylated = sum(value <= 0, na.rm = TRUE)) #Take long format data and group by treatment. For each treatment, use summarize to count the number of CpG with data, the number of methylated CpG, and the number of unmethylated CpG
stackedBarplotData %>%
  mutate(sanityCheck = total - methylated) #Sanity check: total - methylated should equal the unmethylated column. It does, so we're good!
```

...then piped the data into a stacked barplot! Before plotting with the data, I needed to reorder treatment information and pivot the data longer.

```
stackedBarplotData %>%
  select(., -total) %>%
  mutate(., treatment2 = case_when(treatment == "NBH.HY.average" ~ "NBH5",
                                   treatment == "NBH.NO.average" ~ "NBH20",
                                   treatment == "NBH.OC.average" ~ "NBH0",
                                   treatment == "SC.HY.average" ~ "SC5",
                                   treatment == "SC.NO.average" ~ "SC20",
                                   treatment == "SC.OC.average" ~ "SC0")) %>%
  pivot_longer(., cols = 2:3, names_to = "CpGstatus", values_to = "count") %>%
  ggplot(., mapping = aes(x = treatment2, y = count, fill = CpGstatus)) +
  geom_bar(position = "fill", stat = "identity") +
  labs(x = "", y = "% CpGs") +
  scale_x_discrete(name = "",
                   labels = c("NBH OC",
                              "NBH NO",
                              "NBH HY",
                              "SC OC",
                              "SC NO",
                              "SC HY")) +
  scale_y_continuous(breaks = seq(0,1.10,0.25),
                     labels = seq(0,110,25)) +
  scale_fill_manual(name = "",
                    labels = c("Methylated", "Unmethylated"),
                    values = c("grey20", "grey80")) +
  theme_classic(base_size = 20) + theme(legend.position = "bottom") #Take stacked barplot data and remove total column. Create a new column, treatment2, that reorganized treatment information in a logical order. Pivot dataframe longer to use for plotting. Create a stacked barplot (position = "fill"). Modify x- and y-axes.
```

I contemplated coding in the number of CpGs in various categories, but since I needed to assemble the plot in InDesign I decided it wasn't worth my time.

### Methylation density plot

Last plot for the multipanel! I took the data for all CpGs with 10x data from the union bedgraphs and put it straight into my previously-written density plot. I made sure to use different line types for NBH and SC.

```
#Create main plot
#Create inset plot
#Plot together

mainPlot <- ggplot(avgMethTreatLong) +
  geom_density(mapping = aes(x = value, color = treatment, linetype = treatment)) +
  labs(x = "Proportion CpG Methylation", y = "Density") +
  scale_color_manual(name = "",
                     breaks = c("NBH.OC.average",
                                "NBH.NO.average",
                                "NBH.HY.average",
                                "SC.OC.average",
                                "SC.NO.average",
                                "SC.HY.average"),
                        labels = c("NBH OC",
                                   "NBH NO",
                                   "NBH HY",
                                   "SC OC",
                                   "SC NO",
                                   "SC HY"),
                     values = c("NBH.OC.average" = plotColors[1],
                                "NBH.NO.average" = plotColors[2],
                                "NBH.HY.average" = plotColors[3],
                                "SC.OC.average" = plotColors[4],
                                "SC.NO.average" = plotColors[5],
                                "SC.HY.average" = plotColors[6])) +
  scale_linetype_manual(name = "",
                        breaks = c("NBH.OC.average",
                                   "NBH.NO.average",
                                   "NBH.HY.average",
                                   "SC.OC.average",
                                   "SC.NO.average",
                                   "SC.HY.average"),
                        labels = c("NBH OC",
                                   "NBH NO",
                                   "NBH HY",
                                   "SC OC",
                                   "SC NO",
                                   "SC HY"),
                        values = c("NBH.OC.average" = 1,
                                   "NBH.NO.average" = 1,
                                   "NBH.HY.average" = 1,
                                   "SC.OC.average" = 2,
                                   "SC.NO.average" = 2,
                                   "SC.HY.average" = 2),
                        aesthetics = c("linetype")) +
  theme_classic(base_size = 20) + theme(legend.position = "bottom")

insetPlot <- mainPlot +
  scale_x_continuous(limits = c(0,0.25)) +
  scale_y_continuous(limits = c(0,3)) +
  theme_classic() + theme(legend.position = "none")

methWithInset <- ggdraw() +
  draw_plot(mainPlot) +
  draw_plot(insetPlot, x = 0.2, y = 0.5, width = 0.65, height = 0.5)
```

Interestingly, using all 10x CpG data shows more of a bimodal density plot which is typical of these data, as opposed to what I had previously.

### Plot assembly

I smashed things together in [this InDesign file](https://github.com/yaaminiv/killifish-hypoxia-RRBS/blob/main/output/05-analysis/new-genome/methylation-landscape/methylation-landscape-multipanel.indd). In addition to resizing, I also added some plot annotations to panel A. The text needs to be bigger on the inset, but for now I think it's good enough to send for feedback!

<img width="1472" alt="Screenshot 2024-06-11 at 10 18 51 AM" src="https://github.com/yaaminiv/killifish-hypoxia-RRBS/assets/22335838/72a8c690-3ceb-4bae-91b8-37b4788b9adf">

**Figure 1**. General methylation landscape figure

### Going forward

1. Update methods and results
3. Determine the genomic locations of DMR
3. Create DMR figure
6. Update OSF repository for all intermediate files
1. Map STAR transcript names to Ensembl IDs from genome
2. Conduct pathway analysis for RNA-Seq data by population
5. Identify known SNP/DMR overlaps

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
