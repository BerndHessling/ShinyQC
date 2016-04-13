# ShinyQC
Automated Proteomics quality control package

# Table of Contents
1. [Package description](https://github.com/BerndHessling/ShinyQC/blob/readme_edits/README.md#package-description)
2. [Prerequisites](# Prerequisites)
3. [General setup AutoQCSoftware](# General setup AutoQCSoftware)
4. [SampleType setup](# SampleType setup)

# Package description


This package allows the automatic analysis and visualization of proteomics standard samples. ID rates as well as certain parameter can be monitored over time for multiple LC-MS platforms in an interactive web application. The software supports any kind of peptide standard and is highly flexible.

It consists of three R-scripts: 

1.  QC-setup.R: Setup scripts that creates the folders and files for the different workflows

2.	AutoQC.R: Automatic MaxQuant analysis of MS raw files

3.	runAutoQCShiny.R: Interactive web application (Shiny app), to visualize data


# Prerequisites


*  The hosting computer needs to have a running version of [MaxQuant](http://www.coxdocs.org/doku.php?id=maxquant:common:download_and_installation) (version number should not matter, but needs to remain unchanged during over different raw files of same sample type).

*	[R](https://cran.r-project.org/bin/windows/base/) needs to be installed (package was developed for version 3.2.4).

*	[R-Studio](https://www.rstudio.com/products/rstudio/download/) needs to be installed.

*	For full functionality certain naming convention should be followed:

    +	MS raw files should start with a unique identifier for the instrument, e.g. if a lab has multiple LC-MS platforms they can be named:

        ```
        Orbi1_*.raw
        Orbi2_*.raw
        etc
        ```

    + MS raw files need to have an unique identifier for the sample type, e.g.:

        ```
        Orbi1_*_BSA_*.raw.
        Orbi1_*_HeLaSTD_*.raw
        etc
        ```
    +	Spaces and unconventional special character must be avoided in file names and folders used by the software.

*	Client computer, from which data can be interactivally und simultanly inspected net to be connected in the same local area network and access the data via browser.


# General setup AutoQCSoftware

1.  Create an empty folder that will store all data created by the software on a local hard-drive. This folder will henceforth be referred to as "DataFolder"
```
D:/QC-software/
```
  
2.	Create subfolder for every sample type that should be analyzed by the software. The folder name is the identifier for sorting raw files in the correct subfoder and must match exactly to a unique identifier Tag in the .raw file name (see Prerequisites: raw file naming convention). These subfolders will henceforth be referred to as “SampleType folders”.
      
    ```
    Orbi_*_BSA_*.raw
    ```
    ***needs a sample type folder named:***
    ```
    D:/QC-software/BSA
    ```


3.	Unpack the zip folder in the DataFolder

4.	Run the “QC-setup.R” script, which copies all needed files in all created subfolders.

5.	Configure the instrument.txt file in the DataFolder according to your instruments. The Abbreviation must match exactly to a unique identifier Tag in the .raw file name, which must be located at the beginning of the raw file. The full instrument name will be the one shown in plots.

      ***Make sure to keep format as .txt and don’t change the file name.***

6.	Configure the logBookChoices.txt file in the DataFolder according to your lab setup. Entry types and user names configured here can be selected for logbook entries in the software.

      ***Make sure to keep format as .txt and don’t change the file name.***


# SampleType setup

## Setting up MaxQuant for each individual SampleType

1.  Go to the raw file type-specific sub-folder (e.g.“D:/QC-software/BSA”). It contains a dummy.raw file, an PeptidesOfInterest.txt file and an “borders.txt” file which need to be configured.

2.	Create an MaxQuant parameter file by starting MaxQuant and follow the upcoming steps:

    +	Load the “Dummy.raw” file from the workflow type folder.
    
    +	Configure the “Group-specific parameters” and “Global parameters” according to your Raw file type.
    
    +	Save the changed parameter file by the “Save parameters” button using default location and name.
    
    +	Close MaxQuant.
    
## Configuring the PeptidesOfInterest.txt and borders.txt

**These two files should be configured after the first analysis of a SampleType specific raw file:**


1.  PeptidesOfInterest.txt:

    +	The PeptidesOfInterest.txt file contains peptides that can be manually controlled over all analyzed runs to check e.g. retention time, score or intensity of these peptides.
    +	To specify these peptides copy the Modified sequence and Charge from the evidence.txt file in that file.
    +	Make sure to choose peptides that are well suited for your quality control (e.g. avoid peptides bearing Methionine which can be oxidized and therefore vary in intensity, choose peptides occurring in only one charge state…).
    +	Make sure to keep format as .txt and don’t change the file name.

2.	borders.txt

    +	The borders.txt file shows all column names of the MaxQuant evidence.txt file as rows.
    +	If the “Include” column shows “TRUE” this data type can be plotted in the software.
    +	The three value columns (“High”, “Low” and “Optimal”) are optional values. If specified these values will be shown in the plot as vertical line in red (“Low” and “High”) or green (“optimal line”).
    +	As these values can differ between different instrument types the according instrument needs to be specified in the “Instrument” column.
    +	Make sure to keep format as .txt and don’t change the file name.
