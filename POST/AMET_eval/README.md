run_CMAQ_eval_AMET
========

This run script controls execution of multiple post-processing and evaluation steps including running combine, sitecmp, sitecmp_dailyo3, loading matched model/obs data (i.e. sitecmp files) into the AMET database and creating AMET "batch" evaluation plots.

## Running on atmos
The Simple Linux Utility for Resource Managment System (SLURM) header at the top of the shell script is used to control execution of the run script on the atmos cluster.
* User should change lines with *--gid* and *--output* to reflect their account on atmos.
* User should **not** change the *--partition=singlepe* if they wish to access the AMET database.  

## Setting environment variables
The setting of environment variables in the run script is divided into 7 different numbered sections.  Details on the environment variables within each section are provided below.  Below section 7 is the portion of the script that loops through the simulations days to create the various post-processing outputs.  The user will typically not need to edit this bottom section.  

### Section 1: Select which analysis steps you want to execute
All 14 environment variables in this section are T/F flags.  Flags can be set to T or F depending on what post-processing files are needed and which steps have already been completed. While the flags can be set in many different permutations, the post-processing must take place in a specific order:
1. Run the combine utility on CCTM output to create COMBINE_ACONC and COMBINE_DEP files. 
2. Create "sitex" run scripts for running the sitecmp and sitecmp_dailyo3 utilities.
3. Run the sitecmp and sitecmp_dailyo3 utilities using the sitex scripts created in step 2 to create comma separated files with matched model/obs pairs for different observation networks. 
4. [Optional] Load model/obs pairs into the AMET database.
5. Create evaulation plots based on matched model/obs data using the batch plotting code in AMET.

A user can choose to do all of the steps at once or run the script multiple times.  For example, the script can be set to only run combine by setting the first flag to T and the remaining flags to F.  The user can then rerun the script at a later time to create the sitecmp files and evaluation plots.  In this case the first flag can be set to F since the combine files already exist.

A user has the option to create an AMET project and load the model/obs data into the AMET MYSQL database. Loading the data into the databse allows users who have access to the RTP campus Intranet to access the data online through the [AMET web interface](http://newton.rtpnc.epa.gov/wyat/AMET_AMAD/querygen_aq.php).  The web interface allows for more refined control over the evaluation plots.  Loading the data into the AMET database also allows the users to evaluate the model output across all of the model/obs data in the simulation period rather than the default mode which produces monthly summaries.  This option is set in section 7. 

An AMET project does not have to be created in order to use the AMET batch plotting scripts.  If the user chooses not to load the data into the AMET database, they should set the AMET_DB flag to F.  In this case the batch plotting scripts will read the data directly from the .csv sitecmp and sitecmp_dailyo3 files. 

### Section 2: Simulation information, Input/Output directories
__Simulation Dates__

Simulation start and end dates (START_DATE_H, END_DATE_H) should be in the format "YYYY-MM-DD".  This run script is set up to organize the various post-processing steps into monthly files.  For example, if a user has an annual simulation and uses the script to go through all of the post-processing steps the end result will include:
1.  12 monthly COMBINE_ACONC and 12 montly COMBINE_DEP files with hourly model output, all written to the $POSTDIR directory. 
2.  12 .csv files with matched model/obs data for EACH network selected in Section 5.  These files will be organized into 12 directories labeled $EVALDIR/$YYYY$MM.
3.  Evalution plots for each month of model/obs pairs, organized into 12 directories labeled $PLOTDIR/$YYYY$MM.

If the user would prefer the evaluation plots be based on ALL available data from the simulation time period, rather than monthly summaries, there is an option for this in section 7.  

__Required Input files__
1. **METCRO2D** - needed for instantaneous hourly surface temperature (TEMP2), planatary boundary height (PBL), solar radiation (RGRND), 10m wind speed (WSDP10), 10m wind direction (WDIR10), precipitation (RN+RC). 
2. **METCRO3D** - needed for instantaneous hourly air density (DENS) which is used in unit conversions of gas and aerosol species
3. **CCTM_ACONC** - needed for hourly average gas and aerosol modeled species time stamped at the top of the hour
4. **CCTM_APMDIAG** - needed for hourly average relative humidity (RH) and PM2.5 modeled size distributions time stamped at the top of the hour
5. **CCTM_WETDEP1** - needed for hourly summed gas and aerosol wet deposition species time stamped at the top of the hour
6. **CCTM_DRYDEP** - needed for hourly summed gas and aerosol dry deposition species time stamped at the top of the hour

*Notes*
* PM2.5 modeled size distributions from the CCTM_APMDIAG file are used to calculate PM2.5 species with a cut-off diameter of 2.5μm or less.  These species are labeled with the "PM25\_" in concentration species definition files provided in the CMAQ code base for versions 5.2 and later.  For example, model variable ANO3IJ is I and J mode particle nitrate and PM25_NO3 is particle nitrate with diameter of 2.5μm or less.
* Prior to CMAQv5.2, aerosol modeled size distributions were contained in the AERODIAM file which contained instantaneous hourly model variables starting with hour 1.  In CMAQv5.2 the CCTM_APMDIAG output was created to produce hourly average model variables starting with hour 0 which is analogous to the structure of the CCTM_ACONC output file.  This script is structured to *only* work with the CCTM_APMDIAG file for extracting model size distributions.  If the CCTM_APMDIAG file was not produced by the model simulation (by setting CTM_APMDIAG flag to F in the run_cctm.csh run script) then this evaluation script can be modified to remove the dependency on the CCTM_APMDIAG file.  See section 4 for more details.
* Surface temperature and relative humidity are used to calculate an "FRM equivalent" PM2.5 total estimate that accounts for loss of particle nitrate, sulfate and ammonium from the FRM sampling filters. These species are labeled with "\_FRM" in the concentration species definition files provided in the CMAQ code base for versions 5.2 and later, i.e. PMIJ_FRM and PM25_FRM.


__Naming Conventions for Input/Output Files__

Consistent naming conventions are used throughout the script to facilitate looping over dates. 
+ This script assumes MET files are dated with the following naming convention: 
   *${METCRO2D_NAME}_${YY}${MM}${DD}.nc*   
   *${METCRO3D_NAME}_${YY}${MM}${DD}.nc*  
+ This script assumes daily CCTM output files are dated with the following naming convention: 
    *${CCTM_Name}_${YYYY}${MM}${DD}.nc*  
  For example: *CCTM_ACONC_v52_intel17.0_SE52BENCH_20110701.nc*    
+ This script will create monthly combine files that are dated with the following naming convention: 
  *${COMBINE_ACONC_NAME}_${YYYY}${MM}.nc*  
  *${COMBINE_DEP_NAME}_${YYYY}${MM}.nc*  

File names can be adjusted but may require changes to the script below section 7. 

Output files are organized into three directories: 
+ $POSTDIR - loction to write combine files, or the location of existing combine files
+ $EVALDIR - location to save sitecmp and sitecmp_dailyo3 .csv files for each network
+ $PLOTDIR - location to save evaluation plots 

These directories can be set to the same path.

### Section 3: System configuration, location of observations and code repositories
Prior to running this post-processing run script, the user is encouraged to build their own executables for the combine, sitecmp and sitecmp_dailyo3 executables using the following steps:
1. Clone the 5.2 branch of the USEPA CMAQ GitHub repository: 
  `gitclone -b 5.2 https://github.com/USEPA/CMAQ.git CMAQ52_repo`    
2. Edit and run bldit_project.csh to create a CMAQ “Project” space:
   `Ln 18: set CMAQ_HOME = /home/username/cmaq_project`    
   `Ln 24-40: Select which tools you need (e.g. COMBINE, SITECMP, HR2DAY)`    
   `./bldit_project.csh epa`  
3. Create executables for the Fortran utilities:
   `cd $CMAQ_HOME/POST/combine/scripts`  
   `./bldit_combine.csh [compiler] [version]`    
  Compiler options are intel, gcc, pgi  
  If you don’t choose a version number, the default for the system you’re on will be used (e.g. on atmos: intel 17.0)  

* Within section 3 of this run script the user should select the compiler and compiler version that they used to create the combine, sitecmp and sitecmp_dailyo3 executables.
* CMAQ_HOME should be set to the project directory set in the bldit_project.csh script in step 2.  If you are not using a CMAQ5.2 reposiotry you can comment out the line for CMAQ_HOME in section 3 and modify the location of the executables and the spec_def files in sections 4 and 5.
* OBS_DATA_DIR should be set to the location of the observation data from the different routine networks of interest.  These observation files need to be formatted to be compatible with the sitecmp and sitecmp_dailyo3 utilities.  The pre-formatted files are already available on atmos under the directory /work/MOD3EVAL/aq_obs/routine, but can also be downloaded from the  [CMAS Center Data Clearinghouse](https://www.cmascenter.org/download/data.cfm) under the heading "2000-2015 North American Air Quality Observation Data".
* AMETBASE should be set to the location of the AMETv1.3 code base.  These files are already available on atmos under the directory /work/MOD3EVAL/amet.  They can also be cloned directly from GitHub using the command  `gitclone -b 1.3 https://github.com/USEPA/AMET.git AMET13_repo` 

### Section 4: Combine configuration options

### Section 5: Site compare configuration options

### Section 6: AMET configuration options

### Section 7: Evaluation plotting configuration options
Loading the data into the AMET database also allows the users to evaluate the model output across all of the model/obs data in the simulation period.  This option is set in section 7.  The default setting is for evaluation plots to organized into monthly summaries. For example for a 2 week simulation that spans two months, e.g. 6/15/2011 - 7/15/2011, the default will produce evalation plots for all of the data in June and separate plots for the July data.  
