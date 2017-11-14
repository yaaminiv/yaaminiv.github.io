---
layout: post
title: Skyline Test 2
---

## Skyline works with our demultiplexed files!

Emma sent over a [test .blib](http://owl.fish.washington.edu/spartina/DNR_Skyline_20170427/gigas-4-27-oyster1-test) made from my demultiplexed and converted [oyster 1 mzML file](http://owl.fish.washington.edu/spartina/DNR_MSConvert_20170417/2017_January_23_envtstress_oyster1.mzML). Using the new Windows machine, I ran this file through Skyline Daily. I followed [settings Emma suggested](https://www.evernote.com/l/APItbEh8fZtCPaegmmxkGZMCgriBIWTNRh8) as well as the protocol from my [previous Skyline test](https://yaaminiv.github.io/Skyline-Test-Run/).

### Peptide digestion settings

#### Adding my background proteome.
My background proteome FASTA file can be found [here](https://github.com/RobertsLab/project-oyster-oa/tree/master/analyses/DNR_PECAN_20170228/PECAN-inputs/Combined-gigas-QC.fasta).

![unnamed-1](https://cloud.githubusercontent.com/assets/22335838/25507772/f1c855ec-2b62-11e7-92f1-b9c993ea39fc.png)

![unnamed-2](https://cloud.githubusercontent.com/assets/22335838/25507774/f1cb8d48-2b62-11e7-8e2c-ec6378fe532e.png)

![unnamed-3](https://cloud.githubusercontent.com/assets/22335838/25507771/f1c4c062-2b62-11e7-993d-79ff1819fd34.png)

#### Modifying my peptide digestion settings.
For digestion enzyme, I used Trypsin.

![unnamed-2a](https://cloud.githubusercontent.com/assets/22335838/25507770/f1c462e8-2b62-11e7-9815-c72d52531c32.png)

![unnamed-2b](https://cloud.githubusercontent.com/assets/22335838/25507769/f1c3c630-2b62-11e7-96e4-ce18ee98ed7e.png)

![unnamed-4](https://cloud.githubusercontent.com/assets/22335838/25507773/f1cabbc0-2b62-11e7-8a25-c09fc2256946.png)

![unnamed-5](https://cloud.githubusercontent.com/assets/22335838/25507775/f1e67c0c-2b62-11e7-84fd-3d208de9e9ff.png)

#### Modification and Quantification.
Last time I used Skyline, I did not touch these sections.

![image](https://cloud.githubusercontent.com/assets/22335838/25507927/b691429e-2b63-11e7-820b-ecdec11eec95.png)

![image-2](https://cloud.githubusercontent.com/assets/22335838/25507926/b690f988-2b63-11e7-8325-26c17d633d0d.png)

![unnamed-8](https://cloud.githubusercontent.com/assets/22335838/25507778/f1ee330c-2b62-11e7-9f3b-cba6d688aaf7.png)

![unnamed-9](https://cloud.githubusercontent.com/assets/22335838/25507779/f1f076da-2b62-11e7-9f70-a1db8b4cd2d8.png)

### Adding FASTA file
I imported the FASTA file like I did previously, but it did not work.

![unnamed-10](https://cloud.githubusercontent.com/assets/22335838/25507781/f1f997c4-2b62-11e7-8e20-9735517e9177.png)

![unnamed-11](https://cloud.githubusercontent.com/assets/22335838/25507780/f1f1017c-2b62-11e7-9f92-befc4d56bd12.png)

![unnamed-12](https://cloud.githubusercontent.com/assets/22335838/25507787/f20b2c82-2b62-11e7-8708-04b66ab204e5.png)

Error message from my attempt to import my FASTA file:
![unnamed-13](https://cloud.githubusercontent.com/assets/22335838/25507782/f1fd2736-2b62-11e7-9983-b88c6d421348.png)

### Transition settings
I decided to move on and change my transition settings before I figured out how to add my protein sequences.
![unnamed-14](https://cloud.githubusercontent.com/assets/22335838/25507784/f204633e-2b62-11e7-8e5b-6dfb2b9cd512.png)

![unnamed-15](https://cloud.githubusercontent.com/assets/22335838/25507783/f2034a1c-2b62-11e7-9880-109540a397f4.png)

![unnamed-16](https://cloud.githubusercontent.com/assets/22335838/25507785/f2053a3e-2b62-11e7-821b-207877dcc777.png)

![unnamed-17](https://cloud.githubusercontent.com/assets/22335838/25507786/f20a2558-2b62-11e7-9ff8-f789659d2bfe.png)

![unnamed-18](https://cloud.githubusercontent.com/assets/22335838/25507788/f20e5d08-2b62-11e7-9cb5-0834118ba72c.png)

### Adding protein sequences
I opened up my FASTA file in Notepad and literally copied and pasted my sequences into the lefthand bar. It worked!

![unnamed-21](https://cloud.githubusercontent.com/assets/22335838/25507791/f215f298-2b62-11e7-9abb-78a84676e5ea.png)

### Importing my raw files as "Results"

![unnamed-22](https://cloud.githubusercontent.com/assets/22335838/25507792/f21b62fa-2b62-11e7-8d1e-866bec018563.png)

![unnamed-23](https://cloud.githubusercontent.com/assets/22335838/25507793/f21cfeb2-2b62-11e7-9806-08f6300959ce.png)

Added [oyster 1](2017_January_23_envt..>) to ensure it worked. It did!
![unnamed-24](https://cloud.githubusercontent.com/assets/22335838/25507794/f21e7968-2b62-11e7-9c1d-25a05c9a20d9.png)

I then imported the rest of my files.
![unnamed-25](https://cloud.githubusercontent.com/assets/22335838/25507795/f220fb0c-2b62-11e7-801d-b26a04484015.png)

Now that my Skyline file is set up, all I need to do is replace the .blib with one from all of my mzML files and start picking targets for SRM.
