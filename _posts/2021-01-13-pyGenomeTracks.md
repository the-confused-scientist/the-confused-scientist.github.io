---
layout: post
title: Journey through pyGenomeTracks
subtitle: Tutorial on plotting genomic regions with multiple tracks [unfinished]
cover-img: /assets/img/path.jpg
tags: [python, bioinformatics]
---

Dear reader,

This is an incomplete tutorial on using biopython open software called pyGenomeTracks (PGT). This aims to "map several genomic data tracks from a variety of resources onto one or a given list of genomic coordinates and generates an image per given coordinate including all of the input tracks". I will post here my attempt at using this program to provide a more clear tutorial, for the less technical of us (like myself) and to help me keep track of the steps I have taken thus far.

We (as in the imaginary reader and myself) still have no idea if this is a successful attempt but *we* are very keen.

Stay tuned.

Best,
The Confused Scientist

-------------------

These are slightly more detailed steps from the [github repository of pyGenomeTracks](https://github.com/deeptools/pyGenomeTracks)

_Step 0_

It is vital to understand that certain biopython packages are only friendly for Linux and Mac Operating Systems - even using a shell on windows to mimic a Linux OS was a nightmare for me (re-iterating how non-tech I am). I was lucky enough to get a Linux OS laptop loan from my internship supervisors.

_Step 0.5_

Understand how Linux works, skip if step 0 does not represent your case or if you already know how to use a macOS or Unix

_Step 1_

Download [Anaconda3](https://www.anaconda.com/products/individual) to get _conda_ commands and in the shell of your OS, write:
```
$ conda create -n pygenometracks -c bioconda -c conda-forge pygenometracks python=3.7
```
to install the latest version of pygenometracks on python version 3.7

_Step 1.5_

To get a handle on how the software works, explore the documentation of [pyGenomeTracks](https://github.com/deeptools/pyGenomeTracks).

_Step 2_

Where to get files from...

* I made my files by running GWAS summary statistics through [FUMA](https://fuma.ctglab.nl/)
  - FUMA is a software developed at the Vrije Universiteit Amsterdam (VU) that is [an integrative web-based platform using information from multiple biological resources to facilitate functional annotation of GWAS results, gene prioritization and interactive visualization](https://www.nature.com/articles/s41467-017-01261-5)
  - Essentially, it takes your GWAS results and maps them in a few graphs to allow you to prioritise your search. For my study it was ideal since the aim was an exploratory analysis of new GWASs for potential targets that can be then functionally analysed in mini-brains.
* This program allows you to download summary .txt files (that can be easily converted to bed files) with information such as genes near important loci, GWAS catalog data summarised to the loci of interest, eQTL data, chromatin interactions, [LD blocks](https://the-confused-scientist.github.io/2020-09-01-GWAS/).

_Step 3_

**Preparing your files.**

* Convert the files to _.bed_ or [other PGT-approved type](https://pygenometracks.readthedocs.io/en/latest/content/all_tracks.html) and give them approrpiate names to keep track, depending on what kind of information you want to convey:
  * the files themselves, should have at least _chrom_, _start_, _end_ and _name_ (bed4)
  * If you want .bed files with more info - check [this website](https://bedtools.readthedocs.io/en/latest/content/general-usage.html) with details of the kinds of information bed files require
  * Any other file types can be found on [the PGT website](https://pygenometracks.readthedocs.io/en/latest/content/all_tracks.html)
* Make sure all of your files are on the same genome assembly (i.e. hg38 vs hg19)
    * if these are not on the same, use [LiftOver](https://genome.ucsc.edu/cgi-bin/hgLiftOver) but keep in mind that this may affect your files so try to minimise how many times you use this
     * I suggest saving them as e.g. *name*_*assembly*.bed_ to keep good track of everything

_Step 4_

* All files can then be converted into tracks files for pyGenomeTracks to use:
  - have all your files (in the right format) in the folder you are running PGT and conda from
  - you must be on the **shell of your OS in the same folder**, and have pyGenomeTracks installed (see Step 1)
  - to make sure you are running the correct environment, it's important to run:
  ```
  $ conda activate pygenometracks
  ```
  - it should show up as *(pygenometracks)* before your computer's name in the shell - meaning you have activated the correct environment, like this:
  ```
  (pygenometracks) user/folder_w_conda_and_PGT>
  ```
  - And now you're ready to start.
    
_Step 5_

The only pre-processing step for pyGenomeTracks to run smoothly, is to make tracks files. For me, being less than tech-savvy, I had no clue what this meant so here is what I learnt:

- you use this code
```
$ make_tracks_file --trackFiles file1.bed file2.bw -o tracks.ini
```
- this gives you a track.ini file (configuration file, described below what this entails) which you can open as a text file where you can edit inside of, without running the previous command multiple times
[pic of this]
- using this, you can add multiple files directly to the tracks.ini file or you can make one file, then change in the text software and save with different names each time
- every file you add, I would visualise (Step 6) before continuing to make sure what you are doing is going smoothly and to be able to customise it correctly

_Step 6_

Visualising images of the tracks - this is done by specifying the tracks and the region which should be shown in the ouput at least, [there are more commands you can use though!](https://pygenometracks.readthedocs.io/en/latest/content/usage.html)

- you use this code
```
$ pyGenomeTracks --tracks tracks.ini --region chr2:10,000,000-11,000,000 --outFileName nice_image.pdf
```
- and the file _nice_image.pdf (or png or whatever you want)_ will come out in the same folder and can look something like:
[pic of PGT example]

Et voilà - you can now visualise your GWAS summary in a super customisable way, making ready to publish figures


_**Configuration file**_
- a configuration file is, as described by [Lopez-Delisle et al. 2021](https://academic-oup-com.vu-nl.idm.oclc.org/bioinformatics/advance-article/doi/10.1093/bioinformatics/btaa692/5879987):

*"[A] file [that] defines best practice, but it can also be fully customized by the user. In a configuration file, each track is defined as a block of parameters starting with its name [track name] and continues with the parameters for that track such as the file location, its title, height, color etc.*

- and it contains parameters described [here](https://pygenometracks.readthedocs.io/en/latest/content/possible-parameters.html)

I will slowly go through each of the customisations and I will highlight the most important and useful ones for a project such as mine
