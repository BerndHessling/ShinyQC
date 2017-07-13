# ShinyQC
Automated Proteomics quality control package

## Table of Contents
[Package description](#head1)

[Prerequisites](#head2)

[Initialize folder structure and parameter files](#head3)

[Setting up MaxQuant for each SampleType](#head4)

[Analyze first raw file for each SampleType](#head5)

[Starting the Shiny app](#head6)

[Switching to new versions of the software](#head7)

[Tips & Tricks](#head8)


## <a name="head1"></a>Package description


This package allows the automatic analysis and visualization of proteomics standard samples. ID rates as well as certain parameter can be monitored over time for multiple LC-MS platforms in an interactive web application. In addition, a logbook function is implemented, that allows to track hardware changes on LC-MS systems like column exchanges or calibrations, which can also be visualized in plots. The software supports any kind of peptide standard and is highly flexible.

It consists of six R-scripts: 

1.  QC-setup.R: Setup scripts that creates the folders and files for the different workflows

2.	autoQC.R: Automatic MaxQuant analysis of MS raw files

3.  reanalyze.R: Script to create new global data files when e.g. "peptides of interest have been changed"

4.	runAutoQCShiny.R: Interactive web application (Shiny app), to visualize data

5.  server.R: Server script for the Shiny app (does not need to be executed)

6.  ui.R: User interface script for the shiny app (does not need to be executed)


## <a name="head2"></a>Prerequisites


*  The hosting computer needs to have a running version of [MaxQuant](http://www.coxdocs.org/doku.php?id=maxquant:common:download_and_installation) (package is confirmed to be working with version 1.5.2.8 to version 1.5.3.30).

*	[R](https://cran.r-project.org/bin/windows/base/) needs to be installed (package was developed for version 3.2.4 but should be running with every later version as well).

*	[R-Studio](https://www.rstudio.com/products/rstudio/download/) needs to be installed.

*	Following naming conventions for your raw files must be matched:

    +	MS raw files must **start with a unique identifier for the instrument**. Identifiers for different instruments must be of **same length**. If a lab has multiple LC-MS platforms, identifiers could be named like this:

        ```
        Orbi1*.raw
        Orbi2*.raw
        etc
        ```

    + MS raw files must have an **unique identifier for the sample type**. This can be placed anywhere in the raw file name. e.g.:

        ```
        Orbi1*BSA*.raw.
        Orbi1*HeLaSTD*.raw
        etc
        ```
    + **Spaces and unconventional special characters must be avoided in file names and folders used by the software (Including MaxQuant path).**



## <a name="head3"></a>Initialize folder structure and parameter files

1.  Unpack the QCSoftware.zip folder where the software should be stored. The created QCSoftware folder does contain the different R scripts and TemplateFiles folder.
  
2.	Create subfolder for every sample type that should be analyzed by the software in the QCSoftware folder. The folder name is the identifier for sorting raw files in the correct subfoder and must match exactly to a unique identifier Tag in the .raw file name (see Prerequisites: raw file naming convention). These subfolders will henceforth be referred to as “SampleType folders”.
      
    ```
    Orbi_*_BSA_*.raw
    ```
    needs a sample type folder named:
    ```
    D:/QCSoftware/BSA/
    ```


3.	Unpack the zip folder in the QCSoftware folder

4.	Open the QC-setup.R script in RStudio and run the code by hitting the "Run App" button, which copies all needed files in all created subfolders.

5.	Configure the instrument.txt file in the DataFolder according to your instruments. The Abbreviation must match exactly to a unique identifier Tag in the .raw file name, which must be located at the beginning of the raw file. The full instrument name will be the one shown in plots.

      *Make sure to keep format as tab seperated txt and don’t change the file name.*

6.	Configure the logBookChoices.txt file in the DataFolder according to your lab setup. Entry types and user names configured here can be selected for logbook entries in the software.

      *Make sure to keep format as tab seperated txt and don’t change the file name.*


## <a name="head4"></a> Setting up each SampleType

The following steps need to be performed once for every SampleType individually:

1. As a first step run a regular MaxQuant search of your standard file with appropriate settings and uncheck the "Include contaminats" options in MaxQuant (located in "Global parameters/Sequences").

2. Check if an appropriate number of peptides and proteins has been identified. If everything looks fine, load the created mqpar.xml file from the raw file containing folder of that data analysis, remove the raw file and replace it by the "dummy.raw" file located in your SampleType folder.

3. Save the changed parameter file by the “Save parameters” button using default location and name. MaxQuant software can now be closed.

4. Select peptides from this initial MaxQuant analysis in the PeptidesOfInterest.txt file to check e.g. retention time, score or intensity of these peptides:

    +   open evidence.txt output file in the txt folder from the initial search in a spreadsheet program like excel and select 3 to 6 peptides well suited for your quality control (e.g. choose peptides with decent intensity that should be consistently identified in upcoming analysis, choose peptides occurring in only one charge state, choose peptides covering a large area of your gradient, avoid peptides bearing Methionine which can be oxidized and therefore vary in intensity, …).
    
    +	To specify these peptides copy the Modified sequence and Charge from the evidence.txt file in PeptidesOfInterest.txt file.
    
    *Make sure to keep format as tab seperated txt and don’t change the file name.*

## <a name="head5"></a> Analyze first raw file for each SampleType

1. Drag and drop a one raw file for each SampleType in the DataFolder.

2. Open the AutoQC.R script in RStudio and run the code by hitting the "Run App" button.

3. Specify the DataFolder in the pop-up window.

4. Track the raw file processing in the R-Studio console and wait until it's finished. The console should print the following statements:

    ```
    Found file Orbi1_160130_BSA_01.raw
    Start maxquant analysis of file Orbi1_160130_BSA_01
    Finished MQ search file Orbi1_160130_BSA_01
    Finished writing QC-pdf for Orbi1_160130_BSA_01
    No more raw files to process; check again in 2 min
    ```
    
    ***If any problem occurs during MaxQuant search, try a regular MaxQuant search without the script but same settings. If that worked out fine, change back to the dummy.raw file in the mqpar.xml file and save.***


## <a name="head6"></a>Starting the Shiny app

1. Open another instance of R-Studio and open the runAutoQCShiny.R.

2. Run the app and specify the the SampleType folder, which should be inspected, in the first pop-up window.

    ```
    D:/QC-software/BSA
    ```
    
3. Specify the MaxQuant/bin folder in the second pop-up window.

    ```
    D:/MaxQuant_1.5.3.8/MaxQuant/bin
    ```
    
4. Interactively inspect your data and create logbook entries for the instruments.

5. Connect from other computers by calling the IP adress of the hosting pc and the port 3168 in browser.

    ```
    //192.168.0.1:3168
    ```
    
6. Update the data with newly analyzed raw files by hitting the "update app" from host or client computers.

7. If you have more than one sample type, repeat steps 1-6 and change the port number in the last line of the script to a unique number. E.g. port = 3167:

    ```
   runApp(parentDir, host="0.0.0.0",port=3167)
    ```

## <a name="head7"></a>Switching to new versions of the software

If a new version of ShinyQC is released this version can be applied by downloading the latest zip folder and replacing all R-script files used before with these new ones. To account for changed data formats the MaxQuant output files need to be reanalyzed with the reanalyze.r script for each data analysis type.

## <a name="head8"></a>Tips & Tricks

### Minimize data volume

All raw files are deleted ones analyzed and only the MaxQuant txt output folder is stored for each analysis and renamed as the raw-file name. To minimize the used data volume one can specify in the MaxQuant parameter to spare some of the large output files in the txt folder. This can be done by unchecking all boxes in the "Tables" option under "Global parameters" of the MaxQuant GUI when setting up the MaxQuant parameter file for each workflow (specifically important for high complex standards).
