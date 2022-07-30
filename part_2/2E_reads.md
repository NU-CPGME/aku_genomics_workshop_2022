# Read Quality Control and Trimming

### August 2022

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)* 

----

## Section 1 - Read quality control with [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)

<img src="../images/fastqc_example.png" width=600/>



## Section 2 - Read trimming

Before we assemble, we'll do some very light read trimming. This step removes any Illumina adapter sequences that may have made it into the reads due to read-through of short library fragments. These adapter sequences can sometimes get incorporated into your assembly and cause misassemblies. Better to take the time to remove them prior to assembly. 

We are going to use [trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic) to perform the trimming step. Trimmomatic can perform a number of functions including adapter removal as well remove low-quality sequences from the reads. For more information about trimmomatic, you can visit its [website](http://www.usadellab.org/cms/?page=trimmomatic). 

Trimmomatic comes with adapter sequence files corresponding to most Illumina library prep methods. You just have to make sure to use the one that matches the library prep kit used to generate your sequencing libraries.

_Commands_ 

```
conda activate cpgme_workshop
```

```
trimmomatic \
	PE \
	reads/GAS_1.fastq.gz reads/GAS_2.fastq.gz \
	GAS_trimmed_paired_1.fastq.gz GAS_trimmed_unpaired_1.fastq.gz \
	GAS_trimmed_paired_2.fastq.gz GAS_trimmed_unpaired_2.fastq.gz \
	ILLUMINACLIP:$CONDA_PREFIX/share/trimmomatic/adapters/NexteraPE-PE.fa:2:30:10 \
	LEADING:3 \
	TRAILING:3 \
	SLIDINGWINDOW:4:15 \
	MINLEN:36
```    
_Settings Used_

Setting | Descripton
--- | ---
`PE` | Paired-end mode
`ILLUMINACLIP` |  Trim adapter sequences. With associated settings.
`LEADING` | Remove low quality bases (< 3) from the beginning of the read  
`TRAILING` | Remove low quality bases (< 3) from the end of the read
`SLIDINGWINDOW` | Cut within sliding window (4 bp) when average quality falls below threshold (15).
`MINLEN` | Minimum length of the read to keep after trimming (36 bp)
   

See [trimmomatic manual](http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/TrimmomaticManual_V0.32.pdf) for more detail on settings and other options.

_Outputs_

Files | Description
--- | ---
`GAS_trimmed_paired_1.fastq.gz` & `_2.fastq.gz` | Paired reads remaining after trimming
`GAS_trimmed_unpaired_1.fastq.gz` & `_2.fastq.gz` | Unpaired reads remaining after trimming 

---

## 4.2 Assembly

Now we'll generate a _de novo_ whole genome assembly from our trimmed reads. For this we'll use the assembler [SPAdes](https://cab.spbu.ru/software/spades/) ([Github site](https://github.com/ablab/spades)).  

**Commands**

```
spades.py \
    -o GAS_assembly \
    -1 GAS_trimmed_paired_1.fastq.gz \
    -2 GAS_trimmed_paired_2.fastq.gz \
    --cov-cutoff auto 
```

**Settings**

Setting | Descripton
--- | ---
`-o` | Name of the directory where the results will be output
`-1` & `-2` | Forward and reverse read paired-end files, respectively
`--cov-cutoff auto` | Remove contigs below a minimum amount of read coverage

See [SPAdes manual](https://cab.spbu.ru/files/release3.15.2/manual.html) for version 3.15.2 for more detail on available options.

> <img src="../images/warn.png" width="20" /> **_Note:_** Usually when you are using SPAdes to perform an assembly you'll also want to use the `--careful` setting. This setting reduces mismatches and short indels by aligning reads back to the contigs to check for errors. We are only skipping it here to save processing time in the workshop.

**Outputs**

All of the output files can be found in the `GAS_assembly` folder. There are a lot of files in there, so we're just going to pick a few of the most relevant to describe.

Files | Description
--- | ---
`contigs.fasta` | Assembly contig sequences.
`scaffolds.fasta` | Scaffold sequnences. Scaffolds consist of contigs joined by N's at sites with paired reads support.
`spades.log` | Log file. Useful for troubleshooting poor or failed assemblies.
`assembly_graph.fasta` | Graph of assembly showing contig connections. May be useful for assembly quality assessment. Can be visualized with a graph viewer like [Bandage](https://rrwick.github.io/Bandage/)  

---

## 4.3 Assembly statistics and quality assesment

After assembly you'll want some sense of the quality of the assembly. This usually includes, but is not limited to, the total number of contigs or scaffolds, the total length of the sequence, and the GC content. 

* Low numbers of contigs usually indicates a pretty good assembly where many unambiguous connections could be made. High numbers of contigs may indicate low read coverage or possible genome or read contamination from another source (though not always)
* Total contig length (sum of the lengths of all contigs) should be compared to the expected genome size of the species or organism. [NCBI Genome](https://www.ncbi.nlm.nih.gov/genome/) is a good source of expected genome sizes. 
* Percent GC content (i.e. the percentage of the sequence that is either cytosine or guanine bases) should also be compared to the expected genome GC content. 
* N50 is length of the contig such that contigs that length or longer account for >= 50% of the total assembly size. A small N50 value may indicate a more fragmented assembly.

A tool that can be used for calculating these values is [Quast](http://quast.sourceforge.net/quast.html). This software is available as either a command-line program or through an interactive website: <http://cab.cc.spbu.ru/quast/>

> <img src="../images/warn.png" width="20" /> **_NOTE:_** I was planning to use the web version of Quast in this workshop, but lately the website has not been working very well, possibly because it is hosted on a server in Russia. To install the command-line version using Conda, enter the following command within your active "cpgme_workshop" environment: `conda install -c bioconda quast`

**Commands**

```
quast GAS_assembly/contigs.fasta
```

**Settings**

Clearly there are not very many settings for the basic quast functionality, just the path to the contigs file.  

You can also use quast to compare to a closely related genome sequence using the `-r` setting to determine possible misassemblies. Just be warned that if your reference genome sequence is not very closely related, you may end up with a number of false-positive missassembly warnings. _Caveat emptor_. 

**Outputs**

By default, outputs will be put into a directory called "quast_results". Here are a few of the output files worth noting:

Files | Description
--- | ---
`report.html` | A report file that can be opened in a web browser like Chrome or Safari.
`report.pdf` | A pdf version of the assembly report
`report.tsv` | A tab-separated version of the report table that can be viewed in a spreadsheet program like Excel or in a text editor

Below is some example output from our assembly viewed in a web browser. 

<img src="../images/quast_example.png" />

---

# [Back to table of contents](../README.md)


---
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.