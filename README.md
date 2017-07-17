# Genome Warehouse
*A Key Technology for "Precision Medicine"*

## Background

A concept of Genome Warehouse was (first?) discussed in 2012 [1].
While the basic requirement for Genome Warehouse hasn't changed much,
we see various technology changes that have happened since then.

There are the three biggest changes in this area:

* Several new
technologies for processing **Big Data** have emerged such as Spark,
Spanner, Impala. Several new genome analytic pipelines have been built
based on these technologies to process genome data efficiently (e.g.,
GATK 4 [2], ADAM [3], GESALL [4], Hail [5]).
* **Cloud** has evolved. Most of the above
mentioned technologies are publicly available from major Cloud
services like Amazon, Google, and Microsoft (e.g., BigQuery, Cloud
Spanner). The original report briefly discussed an option for using
AWS S3 as a storage option, but we need more detailed evaluation of
all available Cloud solutions to understand how we can satisfy the
requirement for Genome Warehouse in the cheapest way (c.f., [6]).
* **Deep Learning** has introduced a new dimension to data analysis.
Applying Deep Learning to Genomics is still an ongoing research effort
([7, 8]), but some early result like DeepVariant [9, 10] looks
promising.

Given the above changes, it’s a good time to think again how we should
build Genome Warehouse.

## High Level Requirements

At high level, Genome Warehouse requires (a) pipelines for processing
genome sequence data and (b) database for storing genome sequence data
and data produced by subsequence analysis (e.g., variant discovery).

A typical pipeline for DNA sequencing is as follows:

1. Sequencing: A sequencer (e.g., Illumina) generates raw sequence data from DNA samples (format; FASTQ [1]).
2. Alignment: An aligner (e.g., BWA-MEM [2, 3, 4]) align reads by finding positions that reads are most likely to have come from (format: SAM, BAM [5], CRAM [6, 7]).
3. Variant Discovery: A caller finds nucleotides that are different from reference genome at given positions in an individual genome or transcriptome (format: VCF [8]).

In addition to the above major steps, pre-processing and filtering steps such as duplicate elimination would be needed.

Some of the stages can take longer than 10 hours with a single thread, and they achieve higher throughput with parallelization.

The goal of Genome Warehouse to process a large number of DNA samples
(say more than one million) and store generated data in a queryable
format. For a human genome sample, a sequencer can produce one billion
short reads of 200-1000 bases each, totaling 0.5-1 TB of data. More
than one DNA sample can be collected from one person at different
timestamps.

# References

Background:

* [1] David H et al., "A Million Cancer Genome Warehouse", UCB/EECS-2012-211, 2012.
* [2] https://github.com/broadinstitute/gatk
* [3] https://github.com/bigdatagenomics/adam
* [4] http://gesall.cs.umass.edu/
* [5] http://www.hail.is/
* [6] Muir P et al., "The real cost of sequencing: scaling computation to keep pace with data generation". Genome Biology 17, 2016
* [7] T. Ching, et al., "Opportunities and obstacles for deep learning in biology and medicine". 2017
* [8] M. Leung, et al., "Machine learning in genomic medicine: a review of computational problems and data sets". Proceedings of the IEEE, 2016
* [9] R. Poplin, et al., "Creating a Universal SNP and Small Indel Variant Caller with Deep Neural Networks". 2016.
* [10] https://cloud.google.com/genomics/v1alpha2/deepvariant

High Level Requirements:

* [1] FASTQ
* [2] https://github.com/lh3/bwa
* [3] H. Li and R. Durbin, "Fast and accurate long-read alignment with Burrows–Wheeler transform". Bioinformatics, 2010
* [4] H. Licorresponding and N. Homer, "A survey of sequence alignment algorithms for next-generation sequencing", Briefings in Bioinformatics, 2010
* [5] SAM/BAM format specification
* [6] CRAM format specification version 3.0
* [7] CRAM: http://www.ebi.ac.uk/ena/software/cram-toolkit
* [8] VCF
