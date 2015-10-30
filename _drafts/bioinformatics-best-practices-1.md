---
layout:     post
title:      "Bioinformatics Best Practices (Part 1)"
subtitle:   "How to structure your working directory"
date:       2015-10-02 12:00:00
header-img: "img/bg.jpg"
---

Lets say we just started working on a brand new exciting project. Now the
question is: how do we organize the work so that our structure model is
efficient and extendable?

The elements that we usually have to deal with are:

 * data
 * scripts
 * results

To make it more clear let's work on an example. Suppose we work on an RNA-Seq
analysis and we are given a single file with two multiplexed samples (one
control and one condition) and we are asked to compare the two samples with
each other.

## Pre-Processing

The definition of what constitutes data is not very clear during an analysis.
For example in some cases the results of one analysis can serve as data for
another analysis. So we must be very precise of what consitues data in order
to place them in the appropriate directory tree level. At the first level,
data are these that are coming from the environment and that we have no
influence on. In some cases we can have data on the first level that are
minimally processed.

Having said that let's create the first level in our directory tree and let's
arrange the file (`reads.fastq`) that was given to us.

``` bash
├── data
│   └── raw
│       └── reads.fastq
```

Now we want to build a tool that demultiplexes the data into two distinct
files. The rules of the demultiplex algorithm are irrelevant at this stage but
we have to keep in mind whether we are building a generic tool or one for this
particular batch of data. If we are building a generic tool then its location
should be at the first level (in a `dev` directory) but if we are building a
one-off script then its location should be closer to the raw data. In this
case we assume the latter. So we create a `preprocess` directory where we
build the tool and run it. **Note:** We should also add a README file that
describes what processing steps had to be performed and how.

``` bash
├── data
│   ├── preprocess
│   │   ├── condition.fastq
│   │   ├── control.fastq
│   │   ├── preprocess.sh
│   │   └── README
│   └── raw
│       └── reads.fastq
```

## Build Samples

At this point we are done with the data and we can extend the tree structure
to group our data into samples. This structure will allow us to run further
processing steps (e.g. alignment, adaptor removal, etc) in units. **Notice**
that in the following tree structure the files in the samples are symbolic
links to the preprocessed files and that the links are not absolute but
relevant. We use relevant links so that our working folder is completely
independent from the outside tree structure. If needed we can move the whole
working tree in a new location and everything should work fine.

``` bash
├── samples
│   ├── condition
│   │   └── fastq
│   │       └── condition.fastq -> ../../../data/preprocess/condition.fastq
│   └── control
│       └── fastq
│           └── control.fastq -> ../../../data/preprocess/control.fastq
```

## Developing Project-wide Scripts

Now when we want to develop scripts that will be applied on all the samples
and of general usage we have to place them at the first level. For this we
create a directory tha we name `dev` in which we will place all development
scripts. Of course each particular script will have its own subdirectory and
will be versioned controled (more on this on an upcoming post).

```
├── dev
│   ├── aligner
│   │   └── align.sh
```

## Processing Samples

So let's do the alignment of the reads on a reference sequence. For this we
will build a `run.sh` script in the `run` directory. This script is a wrapper
script that runs all the processing steps on each unit of data (each sample)

```
├── run
│   └── run.sh
```

The run.sh script would look something like this.

``` bash
#!/bin/bash

samples=(
	"control"
	"condition"
)

echo ">> DO ALIGNMENT"
for name in "${samples[@]}"
do
	echo "working for" $name
	sdir=samples/$name/
	mkdir -p $sdir/aligned
	dev/aligner/align.sh $sdir/fastq/reads.fastq > $sdir/aligned/reads.sam
done
```

## Analysis
Having done all the processing it's time to move on with the analysis. So we
create a new directory named `analysis` and a subdirectory for each specific
analysis we want to do. Within each analysis direcotry we create the
subdirectories `dev` (contains scripts we build for this particular analysis),
`run` (contains the `run.sh` that is the wrapper that runs all the analysis
scripts) and `results` (contains all the analysis results). Importantly we
also put a `README` file that describes what we are doing in this analysis and
what we are trying to accomplish.

```
├── analysis
│   ├── count_reads
│   │   ├── dev
│   │   ├── README
│   │   ├── results
│   │   └── run
│   └── differential
│       ├── dev
│       ├── results
│       └── run
```
