# Contig reordering

### August 2022

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)* 

----

## Section 1 - Using Mauve to order contigs

### Step 1.1: Download Mauve

The download links are found [here](https://darlinglab.org/mauve/download.html)
* MacOS: [Download](https://darlinglab.org/mauve/snapshots/2015/2015-02-25/MacOS/Mauve-snapshot_2015-02-25.dmg)
* Windows: [Download](https://darlinglab.org/mauve/snapshots/2015/2015-02-26/windows/mauve_installer_20150226.exe)

### Step 1.2: Perform alignment

1. In the Mauve menu bar, select the following option: _File_ --> _Align with progressiveMauve_
2. Click _Add Sequence..._
3. Select sequence files in this order:  
**1: reference\GAS_NGAS638.gbk**  
**2: GAS_assembly\contigs.fasta**
4. Set output file: GAS_mauve.xmfa
5. Click _Align..._

### Step 1.3: Rearrange contigs relative to a reference sequence

1. In the Mauve menu bar, select the options _Tools_ --> _Move Contigs_
2. Create new folder named GAS_contig_mover and select this folder
3. Select sequence files in this order:  
**1: reference\GAS_NGAS638.gbk**  
**2: GAS_assembly\contigs.fasta**  
4. Click _Start_


## Section 2 - Ordering contigs with nucmer

### Step 2.1: Set up new conda environment

```Shell
conda create -y -n mummer -c bioconda -c conda-forge mummer gnuplot
conda activate mummer
```

### Step 2.2: Create a new folder for the output and run nucmer to generate an alignment. 

```Shell
mkdir demo_data/GAS_mummer
cd demo_data/GAS_mummer
nucmer ../reference/GAS_NGAS638.fasta ../GAS_assembly/contigs.fasta
```

### Step 2.3: Generate a coverage plot of the contigs vs the reference.

For more information about the mummerplot command, see [here](http://mummer.sourceforge.net/manual/#mummerplot).

```Shell
mummerplot -f -l --png out.delta
```

### Step 2.4: Order contigs relative to the reference and generate pseudomolecule

For more information about the show-tiling command, see [here](http://mummer.sourceforge.net/manual/#tiling).

```Shell
show-tiling -c -p consensus.fasta -u unusable.txt -R -V 0 -v 0 out.delta
```

**Output column definitions:**

Column | Definition
--- | ---
1 | start in the reference 
2 | end in the reference 
3 | gap between this contig and the next 
4 | length of this contig 
5 | alignment coverage of this contig 
6 | average percent identity of this contig 
7 | contig orientation 
8 | contig ID


---

# [Back to table of contents](../README.md)


---
<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is 





