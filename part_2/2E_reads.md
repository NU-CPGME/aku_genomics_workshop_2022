# Read Quality Control and Trimming

### August 2022

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)* 

----

**Before we start:**

```Shell
conda activate asssembly
```

Using `cd`, navigate to the "demo_data" folder you downloaded today. 

You should see this as the output of your `ls` command:

```
Alignments    MLTrees       Phylodynamics reads         reference
```


## Section 1 - Read quality control with [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)

<img src="../images/fastqc_example.png" width=600/>

FastQC provides quality metrics for read files and shows the output in graphical and text formats. 

_Commands_

```Shell
fastqc reads/GAS_1.fastq.gz reads/GAS_2.fastq.gz
```

_Outputs_

Files | Description
--- | ---
GAS_1_fastqc.html | Read characteristics in graphical format. Can be opened with a web browser like Chrome
GAS_1_fastqc.zip | Zip file containing results in text versions


More detailed information about how to interpret the results can be found in the manual: [https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/)

## Section 2 - Read trimming

Before we assemble, we'll do some very light read trimming. This step removes any Illumina adapter sequences that may have made it into the reads due to read-through of short library fragments. These adapter sequences can sometimes get incorporated into your assembly and cause misassemblies. Better to take the time to remove them prior to assembly. 

We are going to use [trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic) to perform the trimming step. Trimmomatic can perform a number of functions including adapter removal as well remove low-quality sequences from the reads. For more information about trimmomatic, you can visit its [website](http://www.usadellab.org/cms/?page=trimmomatic). 

Trimmomatic comes with adapter sequence files corresponding to most Illumina library prep methods. You just have to make sure to use the one that matches the library prep kit used to generate your sequencing libraries.

_Commands_ 

```Shell
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

# [Back to table of contents](../README.md)


---
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.