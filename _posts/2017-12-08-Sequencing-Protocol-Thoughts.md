---
layout: post
title: Sequencing Protocol Thoughts
---

## a.k.a. How to take a break from proteomics

Instead of poring over environmental data, I worked on an MBD-Sequencing protocol for the *Crassostrea virginica* gonad samples this week. Turns out I know absolutely nothing about different sequencing protocols, so I had to do a bunch of reading about bisulfite sequencing.

Here's what I learned from a general literature review:

- Most papers are focused on either plant, mice or human epigenomes. Those are not invertebrates.
- According to Olova et al. 2017, "amplification-free and post-bisulfite procedures should become the gold standard for [Whole Genome Bisulfite Sequencing (WGBS)] library preparation"
- MBD-Seq is not the same as WGBS! It's called lots of things, like MethylCap-Seq. According to this [Illumina post](https://www.illumina.com/science/sequencing-method-explorer/kits-and-arrays/mbdcap-seq-methylcap-seq-mbd-seq-mbdcap-migs.html), proteins bind to methylated cytosines, then precipitated out using beads.
- MeDip uses antibodies. Riviere et al. 2017 used this method to profile Pacific oyster methylomes at different methylation states, so it would probably work with *C. virginica*. However, bisulfite sequencing methods are found to be more accurate. We're going to ignore this method.

After learning about the different sequencing methods, I reviewd the methods section from Gavery and Roberts 2013.

- Sheared DNA was incubated with MBD-Biotin Protein and dynabeads following MethylMinder instructions
- Enriched DNA was eluted from the beads and purified
- Illumina Tru-Seq adapters were used to prepare the DNA libraries
- Qiagen's EpiTect Bisulfite Kit was used for bisulfite treatment
- Library preparation and sequencing were done on the Illumina HiSeq 2000

This protocol matches with Olova et al. 2017's suggestion of post-bisulfite treatment and no PCR amplification. However, I don't know much about the kits she used, and whether or not there are better alternatives. Kurdyukov and Bullock 2016 lay out information for MethylCap (Diagenode) and MethylMiner, but don't really compare the efficacy of the two. I'm going to ping Sam and Mac to see if they have any opinions on the kits.

My overall takeway is that Mac's protocol should be sufficient for our work!