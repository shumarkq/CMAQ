
<!-- BEGIN COMMENT -->

[<< Previous Chapter](CMAQ_UG_ch04_model_inputs.md) - [Home](README.md) - [Next Chapter >>](CMAQ_UG_ch06_model_outputs.md)

<!-- END COMMENT -->

# 5. Compile and Run

## 5.1 Introduction

During this chapter the user will learn about how to obtain the CMAQ source codes and how to set-up their CMAQ environment to complete a CMAQ simulation. It should be noted that before you can configure your CMAQ Environment, you must have the required hardware, software and data files required described in the subsequent chapters.

## 5.2 Getting the CMAQ Source Code

CMAQ source code can be installed either using git or from tarballs downloaded from the git repository hosted by GitHub. Both options are described here.

### 5.2.1 Git Installation

In the directory where you would like to install CMAQ, issue the following command to clone the official EPA GitHub repository for CMAQv5.3:

`git clone -b 5.3 https://github.com/USEPA/CMAQ CMAQ_REPO`

Using the git clone option, CMAQ will install into the following directories:

```
CMAQ_REPO/CCTM
CMAQ_REPO/PREP
CMAQ_REPO/POST
CMAQ_REPO/UTIL
CMAQ_REPO/DOCS
```

### 5.2.2 Zip file Installation

Zip files of the CMAQ source code are available from the public GitHub repository. Click the button "Clone or download" from https://github.com/USEPA/CMAQ/tree/5.3 and select "Download ZIP" to download a Zip file of the CMAQv5.3 repository. Alternatively, you may download the Zip file from the [EPA CMAQ website](https://www.epa.gov/cmaq/access-cmaq-source-code).

Reference input/output data for testing the installation of the software are available from the CMAS Center; *data are not available through GitHub*. You must register/login to access the source codes and data from the CMAS Center.

In the directory where you would like to install CMAQ, unzip the model distribution file:

`unzip CMAQ-5.3.zip`

The following directories will be created:

```
CMAQ-5.3/CCTM
CMAQ-5.3/PREP
CMAQ-5.3/POST
CMAQ-5.3/UTIL
CMAQ-5.3/DOCS
```

The Git and Zip file installation options will produce slightly different subdirectories on your Linux system. The base installation directory using the git clone command will be `CMAQ_REPO`; the directory from the Zip file will be `CMAQ-5.3`. The subsequent instructions in this guide will be based on the git clone installation. For Zip file installations, replace CMAQ_REPO with CMAQ-v5.3 in the instructions that follow. The differences in the directory names highlights the difference in functionality between the two options. Cloning the repository gives the user access to the full repository and its history, while downloading the Zip file will only give access to version 5.3.

## 5.3 The CMAQ Repository Structure

After downloading the source codes the user is encouraged to look through the repository to familiarize themselves with the structure. A summarized image of the repository is shown below:

![Figure 5-1](images/Figure5-1.png)

In this image it can be seen that there four main sub folders within the CMAQ repository. The first folder being the CCTM folder which houses all the source codes (i.e. fortran/C programs) and scripts that drive the CMAQ Chemistry Transport Model (CCTM). There is also a README file within this folder to provide a description of the contents of the folders and any pertaint information that the owner of these files wishes to convey.  

The second folder seen is the DOCS folder which contains all relevant documentation pertaining to the CMAQ program suite. It should be noted that this folder contains many subfolders, indicated by the dot dot symbol, and each folder contains documentation pertaining to the file name. For example, the User Manual contains all documentation, including this one, that describes the technical information required for a user to use the CMAQ program suite.

The third folder in the repository is the POST folder which contains several very useful tools for post processing of the input/output data files. Each tool within the folder comes wth the source code, scripts and a README used to run the tool. A technical description of the tools within this folder can be found in [Chapter 7](CMAQ_UG_ch07_analysis_tools.md).

The fourth folder in the repository is the PREP folder which contains all the preparatory subprograms to be run before the CCTM folder is run. Again as before, each subprogram folder contains source codes, scripts and a README used to run the subprogram. The technical information on how to run these programs will be discussed in section 5.4 of this chapter.

The last folder within the repository is the UTIL folder which contains useful utilities relating to the CMAQ program suite. An example is the bldmake utility which is used to compile the source code into executables when you use any of the build scripts in the CMAQ repository. Also included in this repository are two additional C-Shell scripts along with a README to describe the contents of the repository. `Bldit_project.csh` allows the user to extract the build and run scripts and compile the model outside of the repository, while `config_cmaq.csh` helps enforce consistent environment setting for the CMAQ project. Both these scripts will be discussed in the following sections.

## 5.4 Building CMAQ outside of the Repository in a user-specified directory

When cloning the repository or unpacking the tar file of the CMAQ distribution, the top-level directory is recognized by the default build and run scripts as `CMAQ_HOME` (formerly M3HOME prior to CMAQv5.2). This directory is an arbitrary base location of the CMAQ installation on your Linux system for a specific application. If the user will build and run CMAQ within the repository folder structure, then `CMAQ_HOME` does not need to be set explicitly in the `bldit_project.csh` script. If, on the other hand, the user wishes to extract the build and run scripts and compile the model outside of the repository, then `CMAQ_HOME` will need to be specified in `bldit_project.csh`. Executing `bldit_project.csh` will automatically perform this extraction and create a CMAQ folder structure under the location now specified by `CMAQ_HOME`. To perform this operation modify the variable `CMAQ_HOME` in the `bldit_project.csh ` script to identify the folder that you would like to install the CMAQ package under. For example:

```
set CMAQ_HOME = /home/username/CMAQ_v5.3
```

Now execute the script:

```
./bldit_project.csh
```

It should be noted that from now on, the other CMAQ directories are referenced relative to CMAQ_HOME and it is where your CMAQ project will be run from. While this directory structure is convenient for the benchmark case and most CMAQ applications, other configurations are possible.


## 5.5 Compiling and Running CMAQ Preprocessors

### 5.5.1 Initialization of CMAQ Environment

Consistency of configuration variables is critical for building CMAQ itself, not just its libraries. Accordingly CMAQ includes the configuration script `config_cmaq.csh` to help enforce consistent environment settings for CMAQ and its associated libraries [Appendix A](CMAQ_UG_appendixA_model_options.md) lists the `config_cmaq.csh` variables defined for the build process and suggests values to which to set those variables.

Note that for multiprocessor applications it is recommended that the Fortran MPI wrapper script mpif90 be specified for the Fortran compiler (myFC). Using this script, instead of a direct call to the Fortran compiler, will ensure that the full suite of MPI components (libraries and include files) for the compiler are included in the parallel build.

Use the following steps to initialize your CMAQ environment:

```
source config_cmaq.csh [compiler]
```

After running the above command it should be noticed now under CMAQ_HOME, the data file directory has been created and serves as a container for the input and output data for the model, and the lib directory contains links to the compiled binary library files required to build the CMAQ executables. The CMAQ scripts use the following environment variables to alias the locations of these directories:


`CMAQ_LIB   = $CMAQ_HOME/lib` (M3LIB before CMAQv5.2)<br>
`CMAQ_DATA  = $CMAQ_HOME/data` (M3DATA before CMAQv5.2)



If you encounter errors about libraries not being found, check the settings of the `config_cmaq.csh` script variables IOAPI, NETCDF, or MPI to ensure that they are correctly point to the locations of these libraries on your Linux system.

Sourcing the `config_cmaq.csh` script only needs to be invoked during a new installation of CMAQ to make sure the links to these libraries are working correctly. For every successive session, links to these libraries will automatically be created when you run any of the build or run scripts.

### 5.5.2 Meteorology-Chemistry Interface Processor (MCIP)

**>>COMMENT<<** Does MCIP still estimate dry deposition velocities?

**>>COMMENT<<** MCIP is technically in M3 I/O API, not netCDF.

**>>COMMENT<<** Need to define "window" for new users.

**>>COMMENT<<** Should SMOKE have its own section?

As discussed previously in [Chapter 4](CMAQ_UG_ch04_model_inputs.md), MCIP processes meteorological model output from the WRF‑ARW model into I/O API-formatted files that are compatible with CMAQ and SMOKE (the emissions processor that computes emissions inputs to CMAQ). MCIP automatically determines whether an input file is generated by WRF‑ARW by trying to open the file as a netCDF file. If the file can be read as netCDF, MCIP assumes the input is a WRF‑ARW dataset. MCIP can be used to uniformly trim cells off the lateral boundary of the domain defined by the meteorological model, or to window in on a subset of that domain. Configuration options for MCIP include the time periods over which to extract data from the meteorological model output files, horizontal and vertical grid definitions, and selections for integrating satellite cloud observations into MCIP output.

Many of the fields that are simulated by the meteorological model are not modified by MCIP for the emissions model and CMAQ, and they are written to I/O API files. Fields that are required for the transformation to CMAQ’s generalized coordinate system are calculated within MCIP. The dry deposition velocities are no longer calculated by the current version of MCIP. CMAQv5 can now calculate all deposition velocities, MCIPv3.4 will be the last version of MCIP to calculate those velocities internally.

MCIP can extract both temporal and spatial subsets of the input meteorology files. The run script allows the user to specify the beginning and end dates/times of the MCIP simulation; these dates/times can fall anywhere within the range of the input meteorological time period, but must be consistent with the time granularity of the meteorological files. MCIP cannot perform temporal interpolations to artificially increase the temporal resolution of the meteorology fields. Two types of horizontal domain windowing are allowed with MCIP. The boundary trim option (“BTRIM”) uniformly trims grid cells off each of the four lateral boundaries of the input meteorology grid. The nonuniform trim option specifies an offset from the lower left corner of the input meteorology domain and the number of cells in the X and Y directions from the revised origin to extract from the input domain. More information about how to invoke these options is provided in the next section: [MCIP](#Execution Configuration Variables). MCIP also provides the capability to reconfigure the vertical layer structure in the input meteorology through interpolation from the input structure to an output structure defined through sigma coordinates in the run script. Commonly referred to as “layer collapsing,” this option should be exercised with caution as it can significantly impact the conservation of energy assumption inherent in the meteorology through its effects on the predicted wind fields.

The output files generated by MCIP are part of the input files for ICON and BCON and various other programs in CMAQ, requiring it to be the first program that needs to be compiled after installing the CMAQ source codes and initializing CMAQ environment variables on your system. Unlike many of the programs in the CMAQ program suite MCIP is compiled with a Makefile and then run with a run script. The following instructions outline the steps to compile and run MCIP.

In the root of your project directory:

```
source config_cmaq.csh [compiler]
cd $CMAQ_HOME/PREP/mcip/
cp -r $CMAQ_REPO/PREP/mcip/src  ./
cd src
```

Open the Makefile in a text editor to see if path variables for libraries and compilers are correct before executing the Makefile. Refer to the MCIP README file for further information and options.

```
make  | & tee make.mcip.log
```

If no errors arise proceed to running MCIP with the following commands.

```
cd $CMAQ_HOME/PREP/mcip/scripts
```

Open ``run_mcip.csh`` in a text editor and change any path variables for libraries and compilers required as well as run time options desired to run mcip before executing ``run_mcip.csh``. Information on environment variables, input and output files, compiling and running MCIP are provided in the [README.md](../../PREP/mcip/README.md) file in the PREP/mcip folder.

```
source run_mcip.csh |& tee run.mcip.log
```

After completing this step examine the log file to see if script was executed without any errors and terminated normally. If the script was terminated normally the user is ready to compile and run ICON and BCON.

### 5.2.3 Initial Conditions Processor (ICON)

**>>COMMENT<<** ICON/BCON summaries need to be updated to reflect v5.3 updates

ICON generates a gridded binary netCDF file of the chemical conditions in the modeling domain for the initial time of a simulation. It can generate these initial conditions from either an ASCII file of vertically resolved concentration profiles (distributed with CMAQ) or from an existing CCTM output file. If the profiles in an ASCII file do not have the same vertical structure as the CCTM configuration to be used, ICON will interpolate the data to a vertical structure consistent with CCTMs. Using an existing CCTM output file to generate initial conditions is applicable when extrapolating initial conditions from a coarse to a fine grid simulation, as may occur when setting up nested simulations (simulations with finer-resolution grids that cover part of coarser-resolution grids). The configuration options for ICON include selecting the chemical mechanism to model, defining the horizontal and vertical grids, and choosing whether the initial conditions are generated from an ASCII profile or from an existing CCTM output file. To compile and run ICON the following commands should be executed:

```
cd $CMAQ_HOME/PREP/icon/scripts
bldit_icon.csh [compiler] [version] |& tee build_icon.log
```

After successfully compiling ICON, use `run_icon.csh` to run the ICON executable.

```
cd $CMAQ_HOME/PREP/icon/scripts
./run_icon.csh |& tee run_icon.log
```

It should be noted that ICON can be configured for different chemical mechanisms and for different kinds of input data. Information on environment variables, input and output files, compiling and running ICON are provided in the [README.md](../../PREP/icon/README.md) file in the PREP/icon folder.

### 5.2.4 Boundary Conditions Processor (BCON)

**>>COMMENT<<** ICON/BCON summaries need to be updated to reflect v5.3 updates

**>>COMMENT<<** Does BCON really generate the GEOS-Chem-based dynamic BCs? If not, I think this is misleading. (Second paragraph clarifies this, but the first paragraph is misleading with regard to dynamic BCs.)


BCON generates a gridded binary netCDF file of the chemical conditions along the lateral boundaries of the modeling domain. These boundary conditions can be either static or time-varying, and (as with ICON) can be generated from either an ASCII file of vertically resolved concentration profiles or from an existing CCTM output file. Also as with ICON, BCON will interpolate the data in ASCII profiles to a vertical resolution that is consistent with the CCTM configuration. BCON differs from ICON, however, in that it can generate time-varying (i.e., dynamic) boundary conditions. Dynamic boundary conditions are typically extracted either from CCTM outputs from a coarse-grid simulation for nested simulations or from a CCTM simulation using a global-scale model. The file structure of the ASCII input profiles can also support the creation of dynamic boundary conditions, but generally these files are used only for creating static data. The configuration options for BCON include selecting the chemical mechanism to model, defining the horizontal and vertical grids, and choosing whether the boundary conditions are generated from an ASCII profile or from an existing CCTM output file.

BCON is only used to create boundary conditions inputs for the CCTM from an ASCII profile file or from an existing CCTM output file. Users are responsible for preparing CCTM input boundary conditions from other sources, such as global chemistry transport models. To compile and run BCON the following commands should be executed:

```
cd $CMAQ_HOME/PREP/bcon/scripts
bldit_bcon.csh [compiler] [version] |& tee build_bcon.log
```

After successfully compiling BCON, use `run_bcon.csh` to run the BCON executable.

```
cd $CMAQ_HOME/PREP/bcon/scripts
./run_bcon.csh |& tee run_bcon.log
```

It should be noted that BCON can be configured for different chemical mechanisms and for different kinds of input data. Information on environment variables, input and output files, compiling and running BCON are provided in the [README.md](../../PREP/bcon/README.md) file in the PREP/bcon folder.

### 5.2.5 Optional Pre-Processor

Insert brief information on JPROC, CALMAP (agdust), and Windblown dust (wbdust).

**>>COMMENT<<** Has anyone run/tested the CALMAP processor recently? Deposition changes in v5.3 have a large impact on dust estimates. Need to think carefully about what we want to say about windblown dust in the updated guide.

**>>COMMENT<<** Are we recommending dust here? How would a user know whether to use dust or not?

**>>COMMENT<<** Can I run dust with the two-way model? If so, where would I get GRIDCRO2D? Is this done before I run CCTM, or can it be done on the fly?

**>>COMMENT<<** Figure 2-6: Where do I get these inputs? Need to specify what comes with CCTM and what a user has to do separately. Where does BELD01 come from?

JPROC

Windblown Dust

**CALMAP:** CMAQ has the capability to estimate windblown dust emissions in-line in the CCTM. The CMAQ dust emissions module uses land cover/land use data to identify dust source regions. The dust module includes a feature to estimate dust blown off by the wind (as opposed to anthropogenic dust emissions) from agricultural areas and the impacts of planting and harvesting cycles on available erodible lands that affect dust emissions. Calmap is a preprocessor to the CCTM that uses crop calendar information to produce gridded crop planting and harvesting dates for input to the CMAQ dust module. CALMAP reads grid information from the GRIDCRO2D meteorology file (MCIP output), land cover/land use data from BELD3, and crop calendar data to produce files of planting start dates, planting end dates, and harvesting end dates for different crop types interpolated to the modeling grid. These files are input to the CCTM when it is configured to estimate windblown dust and simulate the impacts of agricultural activity on the windblown dust emissions.

### 5.2.6 Utilities for Developers

insert breif description of chemmech, create_ebi, inline_phot_prepro, nml

**Chemical Mechanism Compiler (CHEMMECH):**

**>>COMMENT<<** How do "users choose which mechanism to use"?

**>>COMMENT<<** The last sentence of this section is confusing.

The release version of CMAQ includes all necessary chemical mechanism information for the preconfigured atmospheric chemistry reactions sets or mechanisms in a released version of CMAQ. Users choose which mechanism to use for compiling and running the CCTM executable. CCTM implements a chemical mechanism by using its namelist and FORTRAN modules. The files are in ASCII format and include the mechanism parameters required such as the species, reaction stoichiometry, and kinetics information. The module files are used to compile the CCTM executable while the namelists are read by CCTM at run time.

Advanced users who wish to generate a new chemical mechanism have to use the CHEMMECH utility to convert the mechanism into the files needed by the CCTM program. CHEMMECH uses a mechanism definition file, often named “mech.def”, and optionally the mechanism namelist files to generate FORTRAN modules. The “mech.def” is an ASCII file that uses a rigid syntax to define reactions and their rate constants.

This approach defining the CMAQ chemical mechanisms allows the chemical reactions and their species to be a fixed part of the executable code. Modifications to the namelists can change predictions saved to the output files, deposition processes of species, emissions inputs and other options for species without recompiling the executable. The namelists defining a chemical mechanism are used by CCTM as well as the ICON and BCON pre-processors. The FORTRAN modules are required to run utility programs such as create_ebi and inline_phot_preproc and JPROC.

create_ebi

inline_phot_preproc

[**NML:**](../../UTIL/nml/README.md) In most cases, users will already have a pre-generated species namelist file to be input into ICON, BCON and CCTM. However advanced users who wish to edit these files or want to generate a namelist from scratch can use the NML utility. This utility contains two scripts, CSV2NML and NML2CSV that are used to convert the species definition files from CSV format to NAMELIST files and from a NAMELIST format to a CSV file, respectively. The NAMELIST files are used as inputs to the CMAQ programs ICON, BCON, or CCTM to define the processes that will impact each model species. Four NAMELIST files define the processes for gas-phase species (GC.nml), aerosol species (AE.nml), nonreactive species (NR.nml) and tracer speces (TR.nml).

## 5.6 Compiling CMAQ Chemistry-Transport Model (CCTM)

After all required CMAQ inputs are generated using the preprocessors mentioned above the user is now ready to compile CCTM. CMAQ’s current coding structure is based on a modularity level that distinguishes from each other CCTM’s main driver, science modules, data estimation modules, and control/utility subroutines. Also distinguished from each other are the science models (including submodels for meteorology, emissions, chemistry-transport modeling) and the analysis and visualization subsystems.

In CCTM, the process modules that affect the pollutant concentration fields are classified as listed below. Each bullet contains a description of the process followed by module name in parentheses. These modules, with the exception of gencoor, are discussed further later in [Chapter 8](CMAQ_UG_ch08_model_formulation.md).

Science Modules:

-  Horizontal advection (hadv)
-  Vertical advection (vadv)
-  Mass conservation adjustments for advection processes (adjc)
-  Horizontal diffusion (hdiff)
-  Vertical diffusion (vdiff)
-  Gas-phase chemical reaction solver (gas)
-  Aqueous-phase reactions and cloud mixing (cloud)
-  Aerosol dynamics and size distributions (aero)
-  Potential vorticity scaling for stratosphere/troposphere exchange (pv_o3)
-  Meteorology-chemistry coupling (twoway)

Control/Utility Modules:

-  Model data flow and synchronizing of fractional time steps (driver)
-  Model horizontal grid system (grid)
-  Unit conversion (couple)
-  Initialization (init)
-  MPI/parallelization (par)
-  CGRID configuration (cgrds)
-  Process analysis (procan)
-  Species namelist utilities (spcs)
-  Miscellaneous functions (util)

Data Estimation Modules:

-  Deposition velocity estimation (depv)
-  Photolytic rate computation (phot)

line Emissions Modules:

-  Calculate emissions (biogenics, dust, lightning, sea salt, plume rise) in-line (emis)
-  In-line BEIS3 biogenic emissions (biog)
-  In-line plume rise (plrise)

The CMAQ modularity makes it easy to modify or introduce a specific scientific process in CCTM. For example, the gas module contains several options for different gas-phase chemistry solvers that can be used to optimize model performance. Without the modular structure, changes to just one scientific process could entail having to modify source code throughout CCTM, thereby greatly increasing the risk of human error.

In addition to modifying individual scientific processes, the user has the option to specify which scientific processes that the user wants to include when building the model. These scientific options are split into build time options and run time options and can be configured in a multitude of ways. To modify any science options during build time, edit the `bldit_cctm.csh` script. The `bldit_cctm.csh` script also contains other information, such as the option to run in single or multiprocessor mode as well as debug mode. To see a complete list of optional settings reference [Appendix A](CMAQ_UG_appendix_A.md).

Once the `bldit_cctm.csh` script is configured to the user's preference, the user is ready to run the script to build the CCTM executable. To do this run the following commands:

```
cd $CMAQ_HOME/CCTM/scripts
source bldit_cctm.csh [compiler] [version] |& tee build_cctm.log
```

**>>COMMENT: Not sure if this section adds any value for the user <<**

Following normal termination of the script with the default configuration, the user will notice a BLD directory created where their CCTM scripts are located. This is the location of the CCTM executable along with the relevant source codes needed to build the model. At this location a few useful commands can be used to update the executable if any changes are made to the fortran source codes via the MakeFile. For example, if the user wants to recompile the source codes in debug mode instead of re-running the `bldit_cctm.csh` script the user can use the following commands:

```
cd BLD_CCTM_v53_[compiler][version]
make clean
make DEBUG=TRUE
```

In another example, if the user has made any changes to the source codes in the BLD directory and wanted to update the CCTM executable to reflect these changes the uesr can use the following commands:

```
cd BLD_CCTM_v53_[compiler][version]
make clean
make
```

**>>COMMENT: END SECTION <<**


## 5.7 Running CCTM or CMAQ

After setting up the CCTM executable the model is ready to be run. Much like the `bldit_cctm.csh` script, to modify any run time options edit the `run_cctm.csh` script referencing [Appendix A](CMAQ_UG_appendix_A.md) for a complete list of optional settings. After these settings have been configured use the following commands to run the script:


```
cd $CMAQ_HOME/CCTM/scripts
source run_cctm.csh |& tee run_cctm.log
```

To confirm that the model ran to completion view the run.[data].log file. For MPI runs, check each of the CTM_LOG_[ProcessorID]*.log files. A successful run will contain the following line at the bottom of the log(s):

```
>>----> Program completed successfully <----<<
```

Note: If you are running on multiple processors the log file for each processor is also moved from the $CMAQ_HOME/CCTM/scripts directory to the data output directory:

```
$CMAQ_DATA/output_CCTM_v53_[compiler]/[data_name]
```

and these log files have the name convention:

```
CTM_LOG_[ProcessorID].v53_[compiler]_[data_name]/_[RUNDATE].log
```

The output results will have been placed in the directory:

```
$CMAQ_DATA/output_CCTM_v53_[compiler]_[data_name]
```

and can include upto 23 netCDF-type files: ACONC, AOD_DIAG, APMDIAG, APMVIS, B3GTS_S, CGRID, CONC, DEPV, DRYDEP, DUSTEMIS, LTNGCOL, LTNGHRLY, MEDIA_CONC, PHOTDIAG1, PHOTDIAG2, PMDIAG, PMVIS, SOILOUT, SSEMIS, VDIFF, VSED, WETDEP1, and WETDEP2. The in-depth description about each of these files is described in [Chapter 6](CMAQ_UG_ch06_model_outputs.md).

Common errors in a CCTM simulation include the following:

-  Incorrect paths to input files. Look in the CCTM screen output (capture in your log file) for an Error message about an input file   not being found.
-  Incorrect MPI implementation. A series of MPI errors at the end of the log file often indicate that the MPI job was not submitted correctly.

Check the last few lines of the CCTM output log for messages to help diagnose why the simulation did not complete.


<!-- BEGIN COMMENT -->

[<< Previous Chapter](CMAQ_UG_ch04_model_inputs.md) - [Home](README.md) - [Next Chapter >>](CMAQ_UG_ch06_model_outputs.md)<br>
CMAQ User's Guide (c) 2019<br>

<!-- END COMMENT -->
