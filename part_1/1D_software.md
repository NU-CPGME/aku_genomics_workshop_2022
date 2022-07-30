# Installing and managing software: Conda, Github, and Docker

### August 2022

*Egon A. Ozer, MD PhD (<e-ozer@northwestern.edu>)*  
*Ramon Lorenzo Redondo, PhD (<ramon.lorenzo@northwestern.edu>)* 


# Step 1 - Install Conda <img src="../images/conda.png" width="50"/> 

Conda is a software package manager that allows you to easily install much of the software required for this workshop on your computer. Software is installed into "environments" that can be activated and deactivated from the command line. By setting up conda environments you can have multiple versions of the same software on one computer and avoid conflicts between different version of software packages or incompatible software. This will also allow you to easily install software and and any other programs that are needed to run that software in one step. For a nice introduction to conda, see [this tutorial](https://towardsdatascience.com/a-guide-to-conda-environments-bc6180fc533) or [here](https://docs.conda.io/projects/conda/en/latest/index.html) for more detail. 

### Step 1.A - Download Miniconda installer:

System requirements and installer downloads for Mac, PC, and Linux can be found at [this site](https://docs.conda.io/en/latest/miniconda.html). The links below are for the latest Python 3.9 versions of the packages for each system:  

* Linux (including Windows WSL): [Miniconda3 Linux 64-bit](https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh)  
* Mac: [Miniconda3 macOS 64-bit pkg](https://repo.anaconda.com/miniconda/Miniconda3-py39_4.11.0-MacOSX-x86_64.pkg) (visual installer)


### Step 1.B - Install Miniconda:  

* Mac: Double-click the downloaded .pkg file. Accept all the default settings.
* Linux: In the terminal window, run

```
bash Miniconda3-latest-Linux-x86_64.sh
```

### Step 1.C - Verify installation:

```
conda list
```

If you see a list of installed packages in your Terminal window, you're good!  

### Step 1.D - Install packages in environments:

Conda environments can be set up by 1) manually creating a new environment and then adding software packages to the environment one at a time, 2) listing the software you want added to the environment when you create it, or 3) by using a specially formatted `environment.yml` file to create the environnment and install all the correct packages in one step. If you want more detail about setting up Conda environments, take a look [here](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).



### Step 3.A - Set up the `cpgme_workshop` environment: 

1. First, open your **Terminal** application.  
2. Second, copy or type the following command in the Terimnal: 

    ```
    conda create \
    -y \
    -c bioconda \
    -n cpgme_workshop \
    prokka \
    snippy \
    ivar \
    iqtree \
    mafft \
    treetime \
    spades \
    blast \
    trimmomatic
    ```

3.  Hit enter to run the command. This should take several minutes to complete. 
4.  When the installation has finished and the command prompt reappears, type the following command to activate your environment:

    ```
    conda activate cpgme_workshop
    ```

5.  You will know the environment is active if your commmand prompt now starts with `(cpgme_workshop)`.
6.  Next, in your active conda environment, install the updated software packages.  

    ```
    conda install -y -c conda-forge -c bioconda prokka=1.14.6 snippy=4.6.0 snpEff=4 biopython=1.76 quast
    ```

7. To exit the conda environment, either close your Terminal or type the following command:

    ```
    conda deactivate
    ```

---

# Next Steps and Troubleshooting

You should now be ready to complete the workshop exercies.  
In the next section, we'll download the example data: [Download example files](1_data_download.md)

---

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.