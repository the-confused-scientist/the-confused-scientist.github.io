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
    * if these are not on the same, use [LiftOver](https://genome.ucsc.edu/cgi-bin/hgLiftOver) but keep in mind that this may affect your files so try to minimise how many times you use this, also if you have large files it is best to use the command line command of liftOver (only for Unix OS though) which I explain further in my post on LocusZoom plots.
     * I suggest saving them as e.g. *name*_*assembly*.file-type_ to keep good track of everything

_Step 4_

* All files can then be converted into tracks files for pyGenomeTracks to use:
  - have all your files (in the right format) in the folder you are running PGT and conda from
  - you must be on the **shell/command prompt of your OS in the same folder**, and have pyGenomeTracks installed (see Step 1)
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
[config file example](https://github.com/the-confused-scientist/the-confused-scientist.github.io/blob/master/_posts/config_1.jpg)
- using this, you can add multiple files directly to the tracks.ini file or you can make one file, then change in the text software and save with different names each time
- every file you add, I would visualise (Step 6) before continuing to make sure what you are doing is going smoothly and to be able to customise it correctly

_Step 6_

Visualising images of the tracks - this is done by specifying the tracks and the region which should be shown in the ouput at least, [there are more commands you can use though!](https://pygenometracks.readthedocs.io/en/latest/content/usage.html)

- you use this code
```
$ pyGenomeTracks --tracks tracks.ini --region chr2:10,000,000-11,000,000 --outFileName nice_image.pdf
```
- and the file _nice_image.pdf (or png or whatever you want)_ will come out in the same folder and can look something like:
[pic of PGT example](https://github.com/the-confused-scientist/the-confused-scientist.github.io/blob/master/_posts/grl1.png)

Et voilà - you can now visualise your GWAS summary in a super customisable way, making ready to publish figures


_**Configuration file**_
- a configuration file is, as described by [Lopez-Delisle et al. 2021](https://academic-oup-com.vu-nl.idm.oclc.org/bioinformatics/advance-article/doi/10.1093/bioinformatics/btaa692/5879987):

*"[A] file [that] defines best practice, but it can also be fully customized by the user. In a configuration file, each track is defined as a block of parameters starting with its name [track name] and continues with the parameters for that track such as the file location, its title, height, color etc.*

- and it contains parameters described [here](https://pygenometracks.readthedocs.io/en/latest/content/possible-parameters.html)

I will slowly go through each of the customisations and I will highlight the most important and useful ones for a project such as mine


---------------------------

_[bed]_
- This type of file is ideal for all kinds of bed files (bed4+): [chr, start, end, name(, etc.)].  Ideally, used for genes, SNPs, variants, basically any region blocks. *.bed* files also can be used for different types of files, explained below.
	- height = 3-5 (for genes), 1-3 (for SNPs); depends on your publishing format
	- style (for genes only) = UCSC (blocks with arrowhead), tssarrow (rectangular blocks with arrows at the top) 
	- color = any [HEX code](https://www.color-hex.com/) or [matplotlib](https://matplotlib.org/3.1.0/gallery/color/named_colors.html)
	- fontsize = 10-15 (crucial for genes labels! also depends on publishing format)
	- labels = true (genes) or false (if you want the plot to be minimal)
	- display = stacked (best for genes, plotted on different levels), collapsed (best for plotting SNPs on one line, without labels), triangles (plots triangles based on length of the coordinates), or interleaved (plots on fewer levels)
	- **file_type** = [bed](https://pygenometracks.readthedocs.io/en/latest/content/tracks/bed.html)
	- file = your_file.bed
	- title = your_title (if you want this plotted on the right hand side as a label then add)

[genes](https://github.com/the-confused-scientist/the-confused-scientist.github.io/blob/master/_posts/genes.jpg)

_[bedgraph]_
- UCSC defines bedgraph, but you can also use a simplified version, as a .bed file with: [chr, start, end, value] to plot a peak graph for frequencies for example.
	- **use_middle** = true (this part is crucial if you use the simplified bed file so that the value specified is thought of as the peak point. This is not useful if you use the conventional [bedgraph file](http://genome.ucsc.edu/goldenPath/help/bedgraph.html)
	- height = 3
	- color = same as before, use lighter colors especially if you want to overlay a bedfile to match the coordinates of your plot, such as follows:
	- (overlay_previous = share-y --> this would be a parameter for the bed file type you want to overlay, i.e. plot over each other; share-y means that the y axis will be shared and therefore they will be proportional to each other)
	- type = fill (default, makes an area type graph), line (lineplot), points (scatterplot, but if you have only basic, this is not a good visualisation)
	- nans_to_zeros = true (recommended in case you have NaNs)
	- number_of_bins = play with this to see this best visualisation for your plot
	- **file_type** = [bedgraph](https://pygenometracks.readthedocs.io/en/latest/content/tracks/bedgraph.html)
	- file = your_file.bed
	- title = your_title (if you want this plotted on the right hand side as a label then add)
	
[frequency](https://github.com/the-confused-scientist/the-confused-scientist.github.io/blob/master/_posts/bedgraph.jpg)

_[links]_
- This type of file can be useful for visualising links/intervals between regions (such as _basic_ chromatin interactions). A .bed file is also useful here: [chr1, start1, end1,chr2, start2, end2]
	- color = (as before) I liked to use darker colors here
	- links_type = arcs (good for general links) , triangles (more compact than arcs), loops (pretty weird, wouldn't recommend)
	- line_width = 2 (the more interactions you have, the less this nr should be)
	- line_style = solid, dashed, dotted or dashdot (all nice, depends on you)
	- orientation = inverted (this is good if you want to plot interactions at the bottom of your graph)
	- alpha = 1 (or lower for more transparent lines)
	- height = 15 (if you have a lot of links, this is good to play around with)
	- compact_arcs_level = 1 (this means your arcs will not be super compact, choose between 0,1 and 2)
	- **file_type** = [links](https://pygenometracks.readthedocs.io/en/latest/content/tracks/links.html)
	- file = your_file.bed
	- title = your_title (if you want this plotted on the right hand side as a label then add)
	
[HiC loops](https://github.com/the-confused-scientist/the-confused-scientist.github.io/blob/master/_posts/links.png)

_[vlines]_
- This type is nice to delimit areas on your plot that you can highlight later. Use a .bed file: [chr, start, start+1]
	- **file_type** = vlines (no extra resource for this)
	- file = your_file.bed
	- line_width = 2
	
[vlines](https://github.com/the-confused-scientist/the-confused-scientist.github.io/blob/master/_posts/vlines.jpg)
             
_[x-axis]_	    
- Plots the genomic region and chr#. I like to put this twice, at the top and at the bottom
	- fontsize = 15
	
_[spacer]_
- This adds a space between your plots	    
	- height = 0.5 (or higher)

[x and spacer](https://github.com/the-confused-scientist/the-confused-scientist.github.io/blob/master/_posts/x_axis%20and%20spacer.jpg)

--------------------------------------------------------------------------------------------------------------------------------------------------
