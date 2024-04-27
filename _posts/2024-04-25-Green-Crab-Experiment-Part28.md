---
layout: post
comments: true
title: Green Crab Experiment Part 28
tags: green-crab metabolomics lipidomics
---

## Investigating specific metabolite and lipid pathways

It's been a while since I actively analyzed my metabolomics and lipidomics data! I've messed around with a few things here and there and done a lot of reading, but let's try and play around with some data again.

### Metabolomics

I started by thinking about my not-so-problem child, my metabolomics data. Based on previous literature, I identified a few pathways of interest:

- Glycolysis and TCA cycle: The WCNA trends suggest an increase in energy production at 30ºC. My respiration data also suggests that I have more oxygen uptake in the 30ºC crabs, so it would be interesting to see if that oxygen is used to meet internal energy demands!
- Anaerobic metabolism (lactic acid fermentation): Kinda the flipside of aerobic respiration. Are energy demands so high at 30ºC that there's a need for anaerobic respiration as well?
- Calcium channel signaling: Calcium ion channels are important for muscle contraction. In cold temperatures, Mg<sup>2+</sup> can block calcium channels, preventing movement. The 5ºC crabs showed slower righting responses, but that response got slightly faster over time, so I was interested in seeing if there were molecules involved in signaling that could help be indirectly assess channel blockage.

It's clear what molecules are involved in the first two processes, but the calcium channel process is a new one for me. So, I went to the literature. Some highlights:

- [Bootman and Bultynck (2020)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6942118/)
  - Uptake of Ca2+ by mitochondrial intimately links cellular signaling to metabolism and bioenergetics: cofactor for enzymes in the TCA cycle, promotes ATP production, metabolism can boost Ca2+ signaling as well
  - IP3 important for calcium signaling
- [Clapham (2007)](https://www.cell.com/fulltext/S0092-8674%2807%2901531-0)
  - Ca2+/calmodulin regulates cell shape through control of myosin's interaction with cytoskeletal actin, so this demonstrates how calcium is important for muscle contraction

Unfortunately none of these publications provided me any information about the metabolites I had data for. I thought, "what would Steven do?" He would turn to his bestie, ChatGPT. I gave ChatGPT my list of known metabolites and asked if any of these molecules were involved in calcium signaling, or if they were precursors or products of molecules involved in calcium signaling. I then decided to ask questions about glycolysis, the TCA cycle, and anaerobic metabolism. You can see my conversation with the AI at [this link](https://chat.openai.com/share/855424ec-921f-40b4-b372-249ec44b4527). I then spot checked a few of these suggestions to ensure that they actually made sense.

Here are my list of important metabolites for various processes based on ChatGPT responses that I then double checked:

**Glycolysis**:

- Glucose-6-phosphate: It is the first intermediate in glycolysis, formed from glucose by hexokinase. It is subsequently converted into fructose-6-phosphate.
- Glucose-1-phosphate: Glucose-1-phosphate is an intermediate in the breakdown of glycogen, which can be converted into glucose-6-phosphate before entering glycolysis.
- Glucose: Glucose is the starting substrate for glycolysis. It is phosphorylated to form glucose-6-phosphate by hexokinase or glucokinase.
- Fructose-6-phosphate: It is an intermediate in glycolysis, formed from glucose-6-phosphate by phosphoglucose isomerase. It is subsequently phosphorylated to fructose-1,6-bisphosphate.
- Fructose-1-phosphate: Fructose-1-phosphate is an intermediate in fructose metabolism. It is converted into glyceraldehyde-3-phosphate (an intermediate in glycolysis) and dihydroxyacetone phosphate by aldolase.
- Pyrophosphate (PPi): Pyrophosphate is produced in the reaction catalyzed by phosphofructokinase-1 (PFK-1) during glycolysis. It is subsequently hydrolyzed to two molecules of phosphate (Pi), driving the irreversible step of glycolysis forward.
- Phosphate (Pi): In glycolysis, phosphate ions (Pi) are involved in several steps as reactants or products, including the phosphorylation of glucose to form glucose-6-phosphate and the hydrolysis of pyrophosphate to form inorganic phosphate (Pi).
- 3-Phosphoglycerate: It is an intermediate in glycolysis, formed from 1,3-bisphosphoglycerate by phosphoglycerate kinase. It is subsequently converted into 2-phosphoglycerate.

**TCA cycle**:

- Citric acid: Also known as citrate, it is the first intermediate in the TCA cycle. It is formed from acetyl-CoA and oxaloacetate and undergoes a series of reactions to regenerate oxaloacetate, producing NADH, FADH2, and GTP in the process.
- Fumaric acid: Fumarate is an intermediate in the TCA cycle, formed by the hydration of fumarate catalyzed by fumarase. It is subsequently converted into malate.
- Alpha-ketoglutarate: Also known as 2-oxoglutarate, it is an intermediate in the TCA cycle. It is formed from isocitrate by isocitrate dehydrogenase and is subsequently converted into succinyl-CoA.
- Malic acid: Malate is an intermediate in the TCA cycle, formed from fumarate by fumarase. It is subsequently converted into oxaloacetate.

**Anaerobic respiration**:

- Glucose-6-phosphate: This molecule is an intermediate in the glycolysis pathway, which produces pyruvate, the precursor of lactic acid in anaerobic conditions.
- Glucose-1-phosphate: Similar to glucose-6-phosphate, glucose-1-phosphate is an intermediate in glycolysis and contributes to the production of pyruvate.
- Glucose: Glucose is the starting substrate for glycolysis, the metabolic pathway that generates pyruvate, the precursor of lactic acid.
- Pyruvate: Pyruvate is the key intermediate in glycolysis, and under anaerobic conditions, it is converted into lactic acid by lactate dehydrogenase.
- Lactic acid: The end product of lactic acid fermentation, formed from the reduction of pyruvate.

**Calcium signaling**:

- UDP-N-acetylglucosamine: Calcium signaling can influence the activity of enzymes involved in the synthesis of UDP-N-acetylglucosamine. Blocking calcium channels with Mg2+ could potentially reduce the production of UDP-N-acetylglucosamine, leading to decreased concentrations.
- Tyrosine: Calcium signaling pathways can regulate the activity of enzymes involved in tyrosine metabolism. Blocking calcium channels may disrupt these pathways, potentially altering the concentration of tyrosine.
- Tryptophan: Similar to tyrosine, tryptophan metabolism can be influenced by calcium signaling. Blocking calcium channels may affect tryptophan metabolism and alter its concentration.
- Threonine: Threonine is an amino acid involved in various metabolic pathways. Changes in calcium signaling can affect threonine metabolism, potentially leading to alterations in its concentration.
Serotonin: Calcium signaling pathways play a crucial role in the release and regulation of serotonin in smooth muscle cells. Blocking calcium channels with Mg2+ could lead to decreased serotonin release and lower concentrations.
- Serine: Similar to threonine, serine is an amino acid involved in various metabolic pathways. Changes in calcium signaling can affect serine metabolism, potentially leading to alterations in its concentration.
- Phosphoinositol: Phosphoinositol lipids are components of cell membranes and can serve as precursors for signaling molecules such as inositol trisphosphate (IP3), which plays a key role in intracellular calcium release from the endoplasmic reticulum.
- Citric acid: Citric acid is an intermediate in the citric acid cycle, which can be influenced by calcium signaling pathways. Blocking calcium channels may disrupt the citric acid cycle, potentially leading to alterations in citric acid concentration.
- Glutathione: Calcium signaling pathways can regulate the synthesis and metabolism of glutathione. Blocking calcium channels may disrupt glutathione metabolism, potentially altering its concentration.
- Glutamine: Glutamine is an amino acid involved in various metabolic pathways. Changes in calcium signaling can affect glutamine metabolism, potentially leading to alterations in its concentration.
- Glutamate: Glutamate is an excitatory neurotransmitter that can be influenced by calcium signaling pathways. Blocking calcium channels may disrupt glutamate release and metabolism, potentially altering its concentration.

Once I had my lists, I set about making plots in [this R Markdown plot](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/05-metabolomics-analysis.Rmd)! For each list, I subset the data then compared abundances in boxplots:

```
transMetabData %>%
  dplyr::select(., c(1:4,
                     `UDP-N-acetylglucosamine `, tyrosine, `tryptophan `, threonine, serine, phosphoinositol, `citric acid`, `glutathione `, glutamine, `glutamate `)) %>%
  pivot_longer(., cols = c(5:14), names_to = "metabolite", values_to = "value") %>%
  mutate(., treatment_day = gsub(pattern = "_3", replacement = "_03", x = treatment_day)) %>%
  mutate(., treatment_day = gsub(pattern = "5_", replacement = "05_", x = treatment_day)) %>%
  ggplot(., mapping = aes(x = treatment_day, y = value, color = treatment_day)) +
  facet_wrap(~metabolite, scales = "free_y") +
  ylab("Metabolite Abundance") +
  geom_boxplot(width = 0.5, position = position_dodge(width = 0.5), alpha = 0.7) +
  geom_point(pch = 21, position = position_dodge(width = 0.5)) +
  scale_fill_manual(name = "Temperature (ºC) x Day",
                    values = c("lightblue", "#2171B5", "grey80", "#525252", "salmon", "#CB181D"),
                    labels = c("5ºC on Day 3", "5ºC on Day 22", "13ºC on Day 3", "13ºC on Day 22", "30ºC on Day 3", "30ºC on Day 22")) +
  scale_colour_manual(name = "Temperature (ºC) x Day",
                      values = c("lightblue", "#2171B5", "grey80", "#525252", "salmon", "#CB181D"),
                      labels = c("5ºC on Day 3", "5ºC on Day 22", "13ºC on Day 3", "13ºC on Day 22", "30ºC on Day 3", "30ºC on Day 22")) +
  xlab("") + #Axis titles
  theme_classic(base_size = 15) + theme(axis.text.x = element_blank(),
                                        axis.ticks.x = element_blank(),
                                        legend.position = "bottom",
                                        legend.text = element_text(color = "black", size = 12),
                                        strip.text.x = element_text(size = 10, color = "black", face="bold"))
```

<img width="1150" alt="Screenshot 2024-04-27 at 4 33 35 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/3035c39a-8792-476c-8f5f-036c7ff07816">

<img width="1150" alt="Screenshot 2024-04-27 at 4 33 44 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/ff6a084a-4c8c-4826-b93d-aff99d31af66">

<img width="1150" alt="Screenshot 2024-04-27 at 4 33 53 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/e0b5cc20-fc94-4e64-ae73-5bd818b7ca1e">

<img width="1150" alt="Screenshot 2024-04-27 at 4 34 01 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/cc05c1ad-a369-4b2e-a5c3-514a58f72e81">

**Figures 1-4**. Metabolite abundance for glycolysis, TCA cycle, anaerobic respiration, and calcium signaling processes.

Overall, there seem to be some temporal and temperature-specific trends! For example, there's a decrease in lactic acid abundance at day 22 vs. day 3 for the 5ºC crabs. Since oxygen consumption slightly increases over time for these crabs, this could suggest that 1) they are able to compensate for energy needs with the increase in aerobic respiration and 2) since they are less mobile, perhaps their energy needs shrank, so they have a smaller energy requirement to begin with that is more easily met with aerobic respiration alone.

When I presented these plots at lab meeting, folks suggested I reorganize the plots such that the abundance for each metabolite is presented in "chronological" pathway order for each crab! This way, I can tell there are certain "breakpoints" in metabolic pathways due to temperature. For the TCA cycle specifically, I could also map that information to oxygen consumption data to see if variation in respiration ties into variation in TCA metabolite abundance! It's a super cool idea that I'm excited to try. I also got a response to my MetaboAnalyst discussion comment, so I'll need to rework the analysis from that end too. By combining programmatic + hypothesis-driven pathway analysis, I think I'm getting close to cracking the story of the metabolomics data!

### Lipidomics

Now to my problem child, the lipidomics data. I wanted to do something similar to the "a priori" pathways analysis I did for the metabolomics data. Work by Chapelle identifies phosphatidylcholines (PC) and phosphatidylethanolamines (PE) as being lipid classes sensitive to temperature. When I looked at my raw lipid data to figure out how many of these compounds existed, it was >100! So instead of boxplots, I decided to make heatmaps to compare abundance across time and temperature in [this R Markdown script](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/code/06-lipidomics-analysis.Rmd).

```
PCData <- rawLipidomicsData %>%
  slice(., 151:390) %>%
  dplyr::select(., -c(identifier, Ion.species, INCHIKEY, m.z, RT, ESI.mode)) %>%
  unite(., "lipid-uniqueID", lipid:uniqueID, sep = "-") %>%
  column_to_rownames(., var = "lipid-uniqueID") %>%
  t(.) %>% as.data.frame(.) %>%
  rownames_to_column(., var = "crab.ID") %>%
  left_join(lipidomicsMetadata, ., by = "crab.ID") #Slice PC data from the raw dataframe and remove extraneous columns. Unite lipid and uniqueID column to make unique column names. Transpose dataframe and convert crab ID to a column. Join with metadata
```

```
heatmapAnnot <- lipidomicsMetadata %>%
  mutate(., Temperature = as.character(treatment)) %>%
  mutate(., Day = as.character(day)) %>%
  dplyr::select(., Temperature, Day) #Retain treatment and day informationonly
head(heatmapAnnot) #Column annotation information
```

```
# pdf("metaboanalyst/figures/PC-heatmap.pdf", width = 11, height = 8.5)
pheatmap(t(log(PCData[, 5:244])), cluster_row = TRUE, show_rownames = FALSE, cluster_cols = TRUE, show_colnames = FALSE, annotation_col = heatmapAnnot)
# dev.off()
```

**Figures 5-6**. Heatmaps of PC and PE data.

Annoyingly, I couldn't get my color annotations to work out well, so I just ignored that for this preliminary iteration. More importantly, I don't see any temporal or temperature-specific trends in lipid abundance! When I presented this to the lab group, Rayna suggested I cluster all of the data, plot those that are significantly different across day x temperature, then determine if those are PC and PE compounds. I think that's a good next step to figure out the relationship between the different variables.

Another thing I was thinking about is lipid naming conventions. [This article](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7707175/) has EXCELLENT descriptions of lipid naming classifications that I need to parse through more. But, I don't think there's as much of a distinction between known and unknown lipids as there is for metabolites. Since we name lipids by their structure, we technically "know" all the lipids. While there are a lot of lipids in my list without a specific structure or identifier assigned, I wonder if running the analysis with all the lipids, then focusing on the lipids with known identifiers for the interpretation may be more useful. I know I want to rerun the lipid WCNA with temperature and day separately since I don't think the interaction term was significant. I should try this with and without all of the lipid data. And finally, I think it's time to pull in the big guns and ask UCD how the heck to deal with all my lipid data.

### Going forward

1. Look into MetaboAnalyst discussion comments
2. Arrange a priori identified pathway plots into "chronological order" for individual crabs
2. Repeat lipidomics WCNA with temperature and day separately
3. Repeat lipidomics WCNA with all lipid data
5. Determine how to get more common lipid names that match MetaboAnalyst formatting
7. Try RaMP enrichment instead of KEGG for lipid sets
6. Normalize for "captivity" effect
8. Determine if I can add physiology data to `mixOmics` analyses
9. Integrate metabolomics and lipidomics data

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
