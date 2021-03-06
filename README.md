# Dependencies
* Mac OS X, or UNIX operating system (Microsoft Windows is not supported)
* Java SE 6 (or higher)
  * [Java.com](https://www.java.com/en/download/)
* Python version 2.7 (or higher)
  * [Python.org](https://www.python.org/downloads/)
* PIP (used for install Python libraries)
  * `sudo easy_install pip` 
* Python-bx (on Mac OS X you may be prompted to install XCode)
  * `sudo pip install bx-python`   
* Pandas (version 0.15.2 or higher)
  * `sudo pip install pandas`
* PLINK version 1.9 or higher (only needed if you want to construct loci yourself instead of using the precomputed onces for this example)
  * [PLINK version 1.9](https://www.cog-genomics.org/plink2/) 


# Run DEPICT
The following description explains how to download DEPICT, test run it on example files and how to run it on your GWAS summary statistics.


## Download DEPICT
Download the compressed [DEPICT version 1 rel128](http://www.broadinstitute.org/mpg/depict/depict_download/bundles/DEPICT_rel128.tar.gz) files and unzip the archive to where you would like the DEPICT tool to live on your system. Note that you when using DEPICT can write your analysis files to a different folder.  Be sure to that you meet all the dependencies described above.


## Test run DEPICT on LDL cholesterol GWAS
The following steps outline how to test run DEPICT on LDL cholesterol GWAS summary statistics from [Teslovich Nature 2010](http://www.nature.com/nature/journal/v466/n7307/full/nature09270.html). 

1. Edit `DEPICT/example/ldl_teslovich_nature2010.cfg`
  * Point `analysis_path` to the where `DEPICT/example` lives on your system (e.g. `/home/projects/depict/git/DEPICT/example/`).  All DEPICT output files will be written to this directory
  * Point `gwas_summary_statistics_file` to the where `DEPICT/example/ldl_teslovich_nature2010.txt.gz` lives on your system.  This is the LDL GWAS summary statistics file used as example input to DEPICT
  * Point `plink_executable` to where PLINK 1.9 executable is on our system (e.g. `/usr/bin/plink`)
  * Point `genotype_data_plink_prefix` to where PLINK binary format 1000 Genomes Project genotype files are on our system. They are part of the downloaded DEPICT files.  You simply need to change to path to where the `DEPICT` folder lives on your system.  Specify the entire path of the filenames except the extension (e.g. `/home/projects/depict/git/DEPICT/data/genotype_data_plink/ALL.chr_merged.phase3_shapeit2_mvncall_integrated_v5.20130502.genotypes`)
2. Run DEPICT on the LDL summary statistics
  * E.g. `./src/depict.py example/ldl_teslovich_nature2010.cfg`
3. Investigate the results (see the [Wiki](https://github.com/DEPICTdevelopers/DEPICT/wiki) for a description of the output format).
  * DEPICT loci `ldl_teslovich_nature2010_loci.txt`
  * Gene prioritization results `ldl_teslovich_nature2010_geneprioritization.txt`
  * Gene set enrichment results `ldl_teslovich_nature2010_genesetenrichment.txt`
  * Tissue enrichment results `ldl_teslovich_nature2010_tissueenrichment.txt`


## Run DEPICT based your GWAS
The following steps allow you to run DEPICT on your GWAS summary statistics. We advice you to run the above LDL cholesterol example before this point to make sure that you meet all the necessary dependencies to run DEPICT.

1. Make an 'analysis folder' in which your trait-specific DEPICT analysis will be stored
2. Copy the template config file from `src/template.cfg` to your analysis folder and give the config file a more meaningful name
3. Edit your config file
  * Point `analysis_path` to your analysis folder.  This is the directory to which output files will be written
  * Point `gwas_summary_statistics_file` to your GWAS summary statistics file.  This file can be either in plain text or gzip format (i.e. having the .gz extension)
  * Specify the GWAS association p value cutoff (`association_pvalue_cutoff`). We recommend using `5e-8` or `1e-5`
  * Specify the label, which DEPICT uses to name all output files (`label_for_output_files`)
  * Specify the name of the association p value column in your GWAS summary statistics file (`pvalue_col_name`)
  * Specify the name of the marker column (`marker_col_name`). Format: <chr:pos>, ie. '6:2321'.  If this column does not exist chr_col and pos_col will be used, then leave if empty
  * Specify the name of the chromosome column (`chr_col_name`).  Leave empty if the above `marker_col_name` is set
  * Specify the name of the position column (`pos_col_name`).  Leave empty if the above `marker_col_name` is set
  * Specify the separator used in the GWAS summary statistics file (`separator`). Options are
    * `tab`
    * `comma`
    * `semicolon`
    * `space`
  * Point `plink_executable` to where PLINK 1.9 executable is on our system (e.g. `/usr/bin/plink`)
  * Point `genotype_data_plink_prefix` to where PLINK binary format 1000 Genomes Project genotype files are on our system. They are part of the DEPICT download. You simply need to change to path to where the DEPICT folder lives on your system.  Specify the entire path of the filenames except the extension (e.g. `/home/projects/depict/git/DEPICT/data/genotype_data_plink/ALL.chr_merged.phase3_shapeit2_mvncall_integrated_v5.20130502.genotypes`)
4. Run DEPICT
  * `<path to DEPICT>/src/depict.py <path to your config file>`
5. Investigate the results which have been written to your analysis folder (see the [Wiki](https://github.com/DEPICTdevelopers/DEPICT/wiki)
  * Associated loci in file ending with `_loci.txt`
  * Gene prioritization results  in file ending with `_geneprioritization.txt`
  * Gene set enrichment results  in file ending with `_genesetenrichment.txt`
  * Tissue enrichment results in file ending with `_genesetenrichment.txt`


# Run a testing version of DEPICT
The following steps outline how to run a lightweight, reduced version of DEPICT based on a precomputed LDL cholesterol DEPICT loci file.  For a particular phenotype, the DEPICT loci file specifices which genes map to given set of associated GWAS SNPs.  Note that this example does not support construction of loci or tissue enrichment analysis.  It is meant to give computational biologists lightweight framework to play around with.

1. Clone DEPICT from Github (use this option if you want to use play around with a reduced, lightweight version of DEPICT)
  * `git clone git@github.com:DEPICTdevelopers/DEPICT.git`
2. Edit `example/ldl_teslovich_nature2010_reduced_example.cfg` 
  * Point `analysis_path` to the where DEPICT/example lives on your system
3. Run DEPICT 
  * `./src/depict.py example/ldl_teslovich_nature2010_reduced_example.cfg`
4. Investigate the results which have been written to the following files (see the [Wiki](https://github.com/DEPICTdevelopers/DEPICT/wiki)
  * Gene prioritization results `ldl_teslovich_nature2010_reduced_example_geneprioritization.txt`
  * Gene set enrichemtn results `ldl_teslovich_nature2010_reduced_example_genesetenrichment.txt`


# Troubleshooting
Please send the log file (ending with `_log.txt`) with a brief description of the problem to Tune H Pers (tunepers@broadinstitute.org).


# Data used in these examples

LDL GWAS [summary statistics](http://csg.sph.umich.edu/abecasis/public/lipids2010/) from [Teslovich Nature 2010](http://www.nature.com/nature/journal/v466/n7307/full/nature09270.html) are used as input in this example. We included all SNPs with P < 5e-8 and manually added chromosome and position columns (hg19/GRCh37).


