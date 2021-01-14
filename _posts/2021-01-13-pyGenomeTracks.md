---
layout: post
title: Journey through pyGenomeTracks
subtitle: Tutorial on plotting genomic regions with multiple tracks [unfinished]
cover-img: /assets/img/path.jpg
tags: [python]
---

Dear reader,

This is an incomplete tutorial on using biopython open software called pyGenomeTracks. This aims to "map several genomic data tracks from a variety of resources onto one or a given list of genomic coordinates and generates an image per given coordinate including all of the input tracks". I will post here my attempt at using this program to provide a more clear tutorial, for the less technical of us (like myself) and to help me keep track of the steps I have taken thus far.

We (as in the imaginary reader, myself and I) still have no idea if this is a successful attempt but *we* are very keen.

Stay tuned.
Best,
The Confused Scientist

-------------------

####_Step 0_
It is vital to understand that certain biopython packages are only friendly for Linux and Mac Operating Systems - even using a shell on windows to mimic a Linux OS was a nightmare for me (re-iterating how non-tech I am). I was lucky enough to get a Linux OS laptop loan from my internship supervisors.

####_Step 0.5_
Understand how Linux works, skip if step 0 does not represent your case

####_Step 1_
Download Anaconda3 and using
```
$ conda create -n pygenometracks -c bioconda -c conda-forge pygenometracks python=3.7
```
install the latest version of pygenometracks on python version 3.7

####_Step 1.5_
To get a handle on how the software works, I wanted to first follow the instructions for the example data on the documentation of [pyGenomeTracks](https://github.com/deeptools/pyGenomeTracks). This is proving much more difficult than anticipated, therefore I am moving on to uploading my own bed files

####_Step 2_
#####Making your own bed files
* I made my files from running GWAS statistics from an article through [FUMA](https://fuma.ctglab.nl/)
* This allows you to donwload different .txt files that you can convert into tracks files for pyGenomeTracks to use in the following way:
` - [] have all your files in the folder you are running pyGenomeTracks and anaconda from
  - [] you must be on the shell of your OS, and have the pyGenomeTracks set up (Step 1)
  - [] important here to run:
  ```
  $ conda activate pygenometracks
  ```
  - this will make sure you are running the correct software
  - [] it should show up as *(pygenometracks)* before your computer's name in the shell - meaning you have activated the correct environment
  - [] following this, you have to clean and prepare your bed files
    * make sure they are _.bed_ and give them approrpiate names to keep track
    * the files themselves, 
    
    
    as described by [Lopez-Delisle et al. 2021](https://academic-oup-com.vu-nl.idm.oclc.org/bioinformatics/advance-article/doi/10.1093/bioinformatics/btaa692/5879987)
    
    "This configuration file defines best practice, but it can also be fully customized by the user. In a configuration file, each track is defined as a block of parameters starting with its name [track name] and continues with the parameters for that track such as the file location, its title, height, color etc. as has been shown in the Supplementary


