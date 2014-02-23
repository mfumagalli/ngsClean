ngsClean
========

DO NOT USE, UNDER DEVELOPMENT

Scripts for initial data filtering.
These have been shamelessly copied from https://github.com/fgvieira/ngsClean and slightly modified.

These scripts are a simple way to go from raw FASTQ reads to BAM files, by filtering reads, sites, and samples.

Briefly, we can wrap up this process in 4 main steps:

1) Reads QC.

     ./filter_reads.sh file_1.fastq file_2.fastq     
     
If file_2.fastq ommited assumes single end reads.
This script reads an input file called filter_reads.paths.txt with paths to software, and filter_reads.options.txt with parameters to be used for filtering.
Examples of these files are given.


2) Read mapping. Not included here! Use your favorite read mapping program (eg. 
BWA, Bowtie2, SNAP, ...)


3) Sites and samples QC.

     ./filter_SNP.sh run_ID $N_IND $CHR bam_list.txt $REF_SEQ $ANC_SEQ     

Again, this script reads an input file called filter_SNP.paths.txt with paths to software, and filter_SNP.options.txt with parameters to be used for filtering.

The output from this step is a BED file with the positions that passed the quality filters. This file can be used on all downstream analyses.


4) Check VCF and SFS to assess overall quality of data.
% Rscript analyze_vcf.R myvariants.vcf output_file




Script Description
==================

filter_reads.sh      Script to perform raw reads QC. Namely, read filtering, 
		     quality trimming, adaptor trimming, PE read merging,...

filter_SNP.sh        SNP QC based on min/max depth, HWE, quality bias, strand 
		     bias, ... It calls the scripts below.

get_mut_bias.pl	     Script to plot the mutation frequency bias along the read. 
		     It was developed to deal with ancient DNA.

get_depth_thresh.R   R script to automatically define the upper and lower depth
		     coverage limits on SNPcleaner. It fits a mixture of Gamma 
		     and Neg-Binomial distribution to the empirical read coverage 
		     distribution.

SNPcleaner.pl        Filters sites from VCF files following a set of rules.

analyze_vcf.R	     Script to visualize aspects of data in VCF files.
