run_CMAQ_eval_AMET
========

This run script controls execution of multiple post-processing and evaluation steps including running combine, sitecmp, sitecmp_dailyo3, loading matched model/obs data (i.e. sitecmp files) into the AMET database and creating AMET "batch" evaluation plots.

## Running on atmos
SLURM header at the top of the shell script is used to control execution of the runscript on the atmos cluster.
* User should change lines with *--gid* and *--output* to reflect their account on atmos.
* User should **not** change the *--partition=singlepe* if they wish to access the AMET database.  

## Setting environment variables
The runscript is divided into 7 different numbered sections.  Details on the environment variables within each section are provided below.

### Section 1: Select which analysis steps you want to execute
All 14 environment variables in this section are T/F flags.  Flags can be set to T or F depending on what post-processing files are needed and which steps have already been completed. While the flags can be set in many different permutations, the post-processing must take place in a specific order:
1. Run the combine utility on CCTM output to create COMBINE_ACONC and COMBINE_DEP files. 
2. Create "sitex" run scripts for running the sitecmp and sitecmp_dailyo3 utilities.
3. Run the sitecmp and sitecmp_dailyo3 utilities using the sitex scripts created in step 2 to create comma separated files with matched model/obs pairs for different observation networks. 
4. [Optional] Load model/obs pairs into the AMET database.
5. Create evaulation plots based on matched model/obs data using the batch plotting code in AMET.

A user can choose to do all of the steps at once or run the script multiple times.  For example, the script can be set to only run combine by setting the first flag to T and the remaining flags to F.  The user can then rerun the script at a later time to create the sitecmp files and evaluation plots.  In this case the first flag can be set to F since the combine files already exist.

A user has the option to create an AMET project and load the model/obs data into the AMET MYSQL database. Loading the data into the databse allows users who have access to the RTP campus Intranet to access the data online through the [AMET web interface](http://newton.rtpnc.epa.gov/wyat/AMET_AMAD/querygen_aq.php).  The web interface allows for more refined control over the evaluation plots. 

An AMET project does not have to be created in order to use the AMET batch plotting scripts.  If the user chooses not to load the data into the AMET database, they should set the AMET_DB flag to F.  In this case the batch plotting scripts will read the data directly from the .csv sitecmp and sitecmp_dailyo3 files. 

### Section 2: Simulation information, Input/Output directories
Simulation start and end dates (START_DATE_H, END_DATE_H) should be in the format "YYYY-MM-DD".  This run script is set up to organize the various post-processing steps into monthly files.  For example, if a user has an annual simulation and uses the script to go through all of the post-processing steps the end result will include:
1.  12 monthly COMBINE_ACONC and 12 montly COMBINE_DEP files with hourly model output, all written to the $POSTDIR directory. 
2.  12 sitecmp .csv files for EACH network selected in Section 5.  These files will be organized into 12 directories labeled $EVALDIR/$YYYY$MM.
3.  Evalution plots will be created for each month of model/obs pairs and organized into 12 directories labeled $PLOTDIR/$YYYY$MM.

If the user would prefer the evaluation plots be based on ALL available data from the simulation time period, rather than monthly summaries, there is an option for this in section 7.  

### Section 3: System configuration, location of observations and code repositories

### Section 4: Combine configuration options

### Section 5: Site compare configuration options

### Section 6: AMET configuration options

### Section 7: Evaluation plotting configuration options
