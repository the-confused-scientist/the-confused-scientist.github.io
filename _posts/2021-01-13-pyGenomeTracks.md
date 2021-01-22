---
layout: post
title: Journey through pyGenomeTracks
subtitle: Tutorial on plotting genomic regions with multiple tracks [unfinished]
cover-img: /assets/img/path.jpg
tags: [python, bioinformatics]
---

Dear reader,

This is an incomplete tutorial on using biopython open software called pyGenomeTracks. This aims to "map several genomic data tracks from a variety of resources onto one or a given list of genomic coordinates and generates an image per given coordinate including all of the input tracks". I will post here my attempt at using this program to provide a more clear tutorial, for the less technical of us (like myself) and to help me keep track of the steps I have taken thus far.

We (as in the imaginary reader, myself and I) still have no idea if this is a successful attempt but *we* are very keen.

Stay tuned.

Best,
The Confused Scientist

-------------------

These are slightly more detailed steps from the [github repository of pyGenomeTracks](https://github.com/deeptools/pyGenomeTracks)

_Step 0_

It is vital to understand that certain biopython packages are only friendly for Linux and Mac Operating Systems - even using a shell on windows to mimic a Linux OS was a nightmare for me (re-iterating how non-tech I am). I was lucky enough to get a Linux OS laptop loan from my internship supervisors.

_Step 0.5_

Understand how Linux works, skip if step 0 does not represent your case

_Step 1_

Download Anaconda3 and using
```
$ conda create -n pygenometracks -c bioconda -c conda-forge pygenometracks python=3.7
```
install the latest version of pygenometracks on python version 3.7

_Step 1.5_

To get a handle on how the software works, I wanted to first follow the instructions for the example data on the documentation of [pyGenomeTracks](https://github.com/deeptools/pyGenomeTracks). This is proving much more difficult than anticipated, therefore I am moving on to uploading my own bed files

_Step 2_

Making your own bed files

* I made my files from running GWAS statistics from an article through [FUMA](https://fuma.ctglab.nl/)
* This allows you to donwload different .txt files that you can convert into tracks files for pyGenomeTracks to use in the following way:
  - have all your files in the folder you are running pyGenomeTracks and anaconda from
  - you must be on the shell of your OS, and have the pyGenomeTracks set up (Step 1)
  - to make sure you are running the correct software, it's important here to run:
  ```
  $ conda activate pygenometracks
  ```
  - it should show up as *(pygenometracks)* before your computer's name in the shell - meaning you have activated the correct environment
  - following this, you have to clean and prepare your bed files

_Step 3_

**Preparing your files.**

Make sure they are _.bed_ and give them approrpiate names to keep track
    * the files themselves, should have at least _#chrom_, _chromStart_, _chromEnd_ and _name_
    * make sure all of your files are on the same genome assembly (i.e. hg38 vs hg19)
      * if these are not on the same, use [LiftOver](https://genome.ucsc.edu/cgi-bin/hgLiftOver) but keep in mind that this may affect your files so try to minimise how many times you use this
     * I suggest saving them as *name*_*assembly*.bed_ to keep good track of everything
     * It is important to have these files in the folder you have your pyGenomeTracks files and Anaconda3
    
_Step 4_

The only pre-processing step for pyGenomeTracks to run smoothly, is to make tracks files. For me, being less than tech-savvy, I had no clue what this meant so here is what I learnt:

- you use this code
```
$ make_tracks_file --trackFiles file1.bed file2.bw -o tracks.ini
```
- using this, you can add multiple files to the tracks.ini (configuration file, described below what this entails)
- every file you make, I would add and visualise before continuing to make sure what you are doing is going smoothly and to be able to customise it correctly

_Step 5_

Visualising images of the tracks - this is done by specifying the tracks and the region which should be shown in the ouput

- you use this code
```
$ pyGenomeTracks --tracks tracks.ini --region chr2:10,000,000-11,000,000 --outFileName nice_image.pdf
```

_**Configuration file**_
- a configuration file is, as described by [Lopez-Delisle et al. 2021](https://academic-oup-com.vu-nl.idm.oclc.org/bioinformatics/advance-article/doi/10.1093/bioinformatics/btaa692/5879987):

*"[A] file [that] defines best practice, but it can also be fully customized by the user. In a configuration file, each track is defined as a block of parameters starting with its name [track name] and continues with the parameters for that track such as the file location, its title, height, color etc.*

- and it looks like this:
```
[x-axis]
#optional
#fontsize = 20
# default is bottom meaning below the axis line
# where = top
[spacer]
# height of space in cm (optional)
height = 0.5
[file1]
file = file1.bed
# title of track (plotted on the right side)
title = file1
# height of track in cm (ignored if the track is overlay on top the previous track)
height = 2
# if you want to plot the track upside-down:
# orientation = inverted
# if you want to plot the track on top of the previous track. Options are 'yes' or 'share-y'.
# For the 'share-y' option the y axis values is shared between this plot and the overlay plot.
# Otherwise, each plot use its own scale
#overlay_previous = yes
# If the bed file contains the exon
# structure (bed 12) then this is plotted. Otherwise
# a region **with direction** is plotted.
# If the bed file contains a column for color (column 9), then this color can be used by
# setting:
#color = bed_rgb
# if color is a valid colormap name (like RbBlGn), then the score (column 5) is mapped
# to the colormap.
# In this case, the the min_value and max_value for the score can be provided, otherwise
# the maximum score and minimum score found are used.
#color = RdYlBu
#min_value=0
#max_value=100
# If the color is simply a color name, then this color is used and the score is not considered.
color = darkblue
# whether printing the labels
labels = false
# optional:
# by default the labels are not printed if you have more than 60 features.
# to change it, just increase the value:
#max_labels = 60
# optional: font size can be given to override the default size
fontsize = 10
# optional: line_width
#line_width = 0.5
# the display parameter defines how the bed file is plotted.
# Default is 'stacked' where regions are plotted on different lines so
# we can see all regions and all labels.
# The other options are ['collapsed', 'interleaved', 'triangles']
# These options assume that the regions do not overlap.
# `collapsed`: The bed regions are plotted one after the other in one line.
# `interleaved`: The bed regions are plotted in two lines, first up, then down, then up etc.
# optional, default is black. To remove the border, simply set 'border_color' to none
# Not used in tssarrow style
#border_color = black
# style to plot the genes when the display is not triangles
#style = UCSC
#style = flybase
#style = tssarrow
# maximum number of gene rows to be plotted. This
# field is useful to limit large number of close genes
# to be printed over many rows. When several images want
# to be combined this must be set to get equal size
# otherwise, on each image the height of each gene changes
#gene_rows = 10
# by default the ymax is the number of
# rows occupied by the genes in the region plotted. However,
# by setting this option, the global maximum is used instead.
# This is useful to combine images that are all consistent and
# have the same number of rows.
#global_max_row = true
# If you want to plot all labels inside the plotting region:
#all_labels_inside = true
# If you want to display the name of the gene which goes over the plotted
# region in the right margin put:
#labels_in_margin = true
# if you use UCSC style, you can set the relative distance between 2 arrows on introns
# default is 2
#arrow_interval = 2
# if you use tssarrow style, you can choose the length of the arrow in bp
# (default is 4% of the plotted region)
#arrow_length = 5000
# if you use flybase or tssarrow style, you can choose the color of non-coding intervals:
#color_utr = grey
# as well as the proportion between their height and the one of coding
# (by default they are the same height):
#height_utr = 1
# By default, for oriented intervals in flybase style,
# or bed files with less than 12 columns, the arrowhead is added
# outside of the interval.
# If you want that the tip of the arrow correspond to
# the extremity of the interval use:
# arrowhead_included = true
# optional. If not given is guessed from the file ending.
file_type = bed
[file2]
file = file2.bw
# title of track (plotted on the right side)
title = file2
# height of track in cm (ignored if the track is overlay on top the previous track)
height = 2
# if you want to plot the track upside-down:
# orientation = inverted
# if you want to plot the track on top of the previous track. Options are 'yes' or 'share-y'.
# For the 'share-y' option the y axis values is shared between this plot and the overlay plot.
# Otherwise, each plot use its own scale
#overlay_previous = yes
color = #666666
# To use a different color for negative values
#negative_color = red
# To use transparency, you can use alpha
# default is 1
# alpha = 0.5
# the default for min_value and max_value is 'auto' which means that the scale will go
# roughly from the minimum value found in the region plotted to the maximum value found.
min_value = 0
#max_value = auto
# The number of bins takes the region to be plotted and divides it
# into the number of bins specified
# Then, at each bin the bigwig mean value is computed and plotted.
# A lower number of bins produces a coarser tracks
number_of_bins = 700
# to convert missing data (NaNs) into zeros. Otherwise, missing data is not plotted.
nans_to_zeros = true
# The possible summary methods are given by pyBigWig:
# mean/average/stdev/dev/max/min/cov/coverage/sum
# default is mean
summary_method = mean
# for type, the options are: line, points, fill. Default is fill
# to add the preferred line width or point size use:
# type = line:lw where lw (linewidth) is float
# similarly points:ms sets the point size (markersize (ms) to the given float
# type = line:0.5
# type = points:0.5
# set show_data_range to false to hide the text on the left showing the data range
show_data_range = true
# to compute operations on the fly on the file
# or between 2 bigwig files
# operation will be evaluated, it should contains file or
# file and second_file,
# we advice to use nans_to_zeros = true to avoid unexpected nan values
#operation = 0.89 * file
#operation = - file
#operation = file - second_file
#operation = log2((1 + file) / (1 + second_file))
#operation = max(file, second_file)
#second_file = path for the second file
# To log transform your data you can also use transform and log_pseudocount:
# For the transform values:
# 'log1p': transformed_values = log(1 + initial_values)
# 'log': transformed_values = log(log_pseudocount + initial_values)
# 'log2': transformed_values = log2(log_pseudocount + initial_values)
# 'log10': transformed_values = log10(log_pseudocount + initial_values)
# '-log': transformed_values = - log(log_pseudocount + initial_values)
# For example:
#tranform = log
#log_pseudocount = 2
# When a transformation is applied, by default the y axis
# gives the transformed values, if you prefer to see
# the original values:
#y_axis_values = original
# If you want to have a grid on the y-axis
#grid = true
file_type = bigwig
```
I will slowly go through each of the customisations and I will highlight the most important and useful ones for a project such as mine
