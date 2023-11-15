## Tutorial 2

**Objective:** To perform reference-based assemblies of West Nile Virus genome libraries to determine infer geographic origin.

You will map reads to a reference assembly, generate consensus genomes, identify single nucleotide variants (SNVs),
and phylogenetically place genomes to determine whether there is a geographical and/or temporal signal to the data.
Additionally, we will employ and compare the results of different methods to generate our consensus genomes.
This includes using software that I have pre-installed on the VM as well as containerized versions of the same
software pulled from Docker. Finally, you will learn about GitHub and Docker for obtaining and installing bioinforamtics software.
<br>

## Some general information

Our read data are in fastq format. Fastq files contain the quality score for each nucleotide in the read and each read has four lines associated with it:

	@SRR10971381.1 1 length=151
	AGTCGATCAGCTGAGTACTAGTAGCATG
	+
	BBBCCCKKIBCCKKBIJDFFHHHIIJKK

> The read name, naming conventions may vary but names are always prefaced by '@'<br>
> This is followed by the sequence line <br>
> `+` a Line break <br>
> Followed by quality scores for each nucleotide in ASCII format

<br>

Each base has an associated quality score, which indicates the reliability of that call.
Phred scores are equal to -10 log<sub>10</sub>P, where P is the error probability for the base in question.
Thus, a phred score of 10 is equal to an error probability of 0.01 (P=10<sup>–Q/10</sup>) or a 1 in 10 chance that the base call is incorrect.

Most NGS output is in fastq format.  We are working with short, paired-end reads (2x250 bp or 2x150 bp)
generated by Illumina sequencers (such as MiSeq, NextSeq, or HiSeq).
While Illumina reads are shorter than those from traditional Sanger sequencing, Illumina sequencing can produce millions to hundreds of millions of reads per sample.
<br>


## General workflow for producing a reference-based assembly

Reference-based assemblies rely on a reference genome on which to map the reads of a genomic library.
Reference assemblies allow the direct comparison of a genome of interest to an already-assembled genome for SNP and/or Indel detection.
Not all differences detected will be legitimate variation.  Miss-assembly (usually due to repetitive elements),
poor quality reads, low coverage, and contamination can all contribute to erroneous base calls.
Implementing additional filtering criteria to detect true variation is necessary,
such as only considering SNPs from an area of high coverage with a certain number of high-quality reads.
A general workflow for generating a reference-based assembly is given below:

<br>

**Quality control**

  - Generate some summary statistics with FastQC

**Adaptor removal (and primer removal) and quality trimming**

 - cutadapt
 - Trimmomatic
 - BBduk from BBtools
 - FastX-toolkit
 - TrimGalore
 - Seqyclean
 - fastp
 - FaQCs
 - And many more

**Read alignment or mapping to reference genome**

 - BWA
 - Bowtie
 - BBmap
 - STAR (for RNA-Seq)

**General statistics pertaining to reference assembly**

 - Samtools
 - Picard
 - BBtools
 - Quast
 - Qualimap

**SNP and INDEL calling**

 - Samtools/bcftools
 - GATK
 - iVAR (for viruses)
 - octopus (for polyploid genomes)

**Additional SNV filtering and annotation**

- bcftools
- GATK

<br>


## Sample assignment and metadata

Each student will map the paired-end (PE) fastqs from one sample to the reference and generate a consensus genome for downstream analyses.

| Student | Sample name |
| ------- | ------------|
| Student1 | wnvA |
| Student2 | wnvB |
| Student3 | wnvC |
| Student4 | wnvD |
| Student5 | wnvE |
| Student6 | wnvF |
| Student7 | wnvG |

Copy the fastq files for your sample from the `BMS500C-2023` directory to your `fastq` directory. Assuming you're in your home directory an example command would look like this:

	cp ../BMS500C-2023/fastq/wnv[A-G]*fastq fastq

> Replace `[A-G]` with the letter of your sample.
> Notice that fastq files have either an _1 or _2 in their names, representing the forward and reverse direction of the sequencing.

<br>


## Adaptor removal and quality trimming

Remove adapters from your reads and keep only those reads passing quality metrics.

	trim_galore -q 30 --length 100 --paired --basename wnv[A-G] wnv[A-G]_1.fastq wnv[A-G]_2.fastq

> Adapters are short oligonucleotides ligated to a library for sequencing on Illumina machines. The reads in the _1 files are generated by sequencing that extends from the read 1 adapter to the read 2 adapter in the 5'-3' direction while the reads in the _2 file are those generated by extending from the read 2 adapter to the read 1 adapter in the 5'-3' direction.
> Adapters are typically removed by the sequencing core but some may still remain. We might also want to remove reads that fall below a certain average quality or length.
> Before running trim_galore, explore the options available to you with the help command. Most default setting are fine.
> Here we are using the default quality threshold of 30 (if we used the default of 20 we wouldn't have to specify it) and a minimum sequence length of 100. 
> **`Paired`** keeps 1 and 2 reads in order.  In other words, if one of the pairs fails quality control, both are removed.  
> The nice thing about TrimGalore is it pretty much does everything for you automatically, including detecting the type of adapter present 
> It also prints some summary statistics to STDOUT.
> Trimmed reads are now in fastq files with the specified basename: `wnv[A-G]_val_1.fastq` and `wnv[A-G]_val_2.fastq

* Judging from the TrimGalore output, does your sequencing run look good?
* What is the percentage of reads that passed trimming?
* You can also look at the quality with [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)


<br>


## Map trimmed reads to the reference genome

We will map our reads to the HQ596519 reference genome with [BWA](http://bio-bwa.sourceforge.net">http://bio-bwa.sourceforge.net) (Burrows-Wheeler Aligner), a short-read aligner tool. Most short-read aligners rely on breaking reads into K-mers (shorter sequences of a specified length), aligning these to the reference genome, and extending these seeds with some amount of base misincorporation. This is actually faster than aligning the entire read. BWA outputs the alignment information in a [SAM](https://samtools.github.io/hts-specs/SAMv1.pdf) format. The output from BWA will need to be saved in a file, converted to a binary format, and sorted by read position in the genome (or coordinate) for additional analyses, such as variant calling.  While we could do each step individually, we can also pipe these commands together for faster processing and to avoid generating intermediate files that we would later delete.

Create an index of your reference genome.

	bwa index HQ596519.fasta

 > Notice the additional files created with this command. Indexing is employed to improve the speed of the mapping algorithm. You can think of it as a look-up table that allows you to more efficiently identify the location of shorter sequences (your kmers) in a longer sequence.

Align your reads to the reference genome with BWA, pipe the output to samtools to sort the reads by their coordinates and to convert to a binary format. Note that we could also use compressed fastq files.

	bwa HQ596519.fasta wnv[A-G]_val_1.fastq wnv[A-G]_val_2.fastq | samtools sort -o wnv[A-G].sorted.bam