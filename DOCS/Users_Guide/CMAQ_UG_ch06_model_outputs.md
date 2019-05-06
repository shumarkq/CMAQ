
<!-- BEGIN COMMENT -->

[<< Previous Chapter](CMAQ_UG_ch05_running_CMAQ.md) - [Home](README.md) - [Next Chapter >>](CMAQ_UG_ch07_analysis_tools.md)

<!-- END COMMENT -->

# Model Output Files

## CCTM Output Files

** >> Comment <<** P153 (for example):  Should remove M3 I/O API "file type" from these tables.  Use more general descriptions.

In this section, details on the CCTM output files are provided. All CMAQ programs produce output files that adhere to the netCDF format.  In addition to model data output, CMAQ can optionally produce log files that contain the standard output from the various CMAQ processors. If the log file option is not selected by the user, CMAQ will write all of the log information to the screen along with the standard error, which can be captured to a text file using basic UNIX syntax.

<a id=Output_Table></a>

** >> Comment <<** In Table 8.1, those Hourly denotation, we should put an asterisk to indicate that it can be user defined, e.g. 30 minutes rather 1 hour and in the same table and table 8-13, there are places with +1 and I think we can make it more generic as +NTHIK (your call).


**Table 3-6. CMAQ Output files**

|**File Name**|**File Type**|**Time-Dependence**|**Spatial Dimensions**|
|----------------------------|------|----|-----------------------------------|
|**Standard**| | | |
|[Output Log](#cmaq_output_log) <a id=cmaq_output_log_t></a>|ASCII|n/a|n/a
|[CTM_CONC_1](#conc)<a id=conc_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[CTM_CGRID_1](#cgrid) <a id=cgrid_t></a>|GRDDED3|1-hour|[2(X+1)+2(Y+1)]*Z
|[CTM_ACONC_1](#aconc) <a id=aconc_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[CTM_DRY_DEP_1](#drydep) <a id=drydep_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[CTM_WETDEP_1](#wetdep) <a id=wetdep_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[CTM_VIS_1](#vis) <a id=vis_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[CTM_AVIS_1](#avis) <a id=avis_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|**Diagnostic and Advanced**| | | |
|[CTM_PMDIAG_1](#pmdiag) <a id=pmdiag_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[CTM_APMDIAG_1](#apmdiag) <a id=apmdiag_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[B3GTS_S](#b3gts) <a id=b3gts_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_DEPV_DIAG](#depv) <a id=depv_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_PT3D_DIAG](#pt3d) <a id=pt3d_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[CTM_DUST_EMIS_1](#dust) <a id=dust_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_AOD_1](#aod) <a id=aod_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[CTM_IPR_1-3](#ipr) <a id=ipr_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[CTM_IRR_1-3](#irr) <a id=irr_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[FLOOR](#floor) <a id=floor_t></a>|ASCII|Hourly|n/a
|[MEDIA_CONC](#media)<a id=media_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_DEPV_MOS](#depv_mos) <a id=depv_mos_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_DRY_DEPV_MOS](#dry_depv_mos) <a id=dry_depv_mos_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_DEPV_FST](#depv_fst) <a id=depv_fst_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_DRY_DEPV_FST](#dry_depv_fst) <a id=dry_depv_fst_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_VDIFF_DIAG](#vdiff_diag) <a id=vdiff_diag_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_VSED_DIAG](#vsed_diag)<a id=vsed_diag_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[LTNG_DIAG1](#ltngdiag1) <a id=ltngdiag1_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]*Z
|[LTNG_DIAG2](#ltngdiag2) <a id=ltngdiag2_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[PLAY_SRCID](#play_srcid) <a id=play_srcid_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_RJ_1-2](#ctm_rj) <a id=ctm_rj_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[SOILOUT](#soilout) <a id=soilout_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_SSEMIS_1](#ssemis) <a id=ssemis_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]
|[CTM_WETDEP_2](#wetdep2) <a id=wetdep2_t></a>|GRDDED3|Hourly|[2(X+1)+2(Y+1)]


<a id=cmaq_output_log></a>
### CMAQ output log
[Return to Table 3-6](#cmaq_output_log_t)

All of the CMAQ processors generate standard output and standard error during execution. For all of the processors other than CCTM, this diagnostic output information can be captured to a log file at execution using a UNIX redirect command. For example, to capture the standard output and error of a CCTM simulation, use the following command:

```
run.cctm >& tee cctm.log
```

For the CCTM, the LOGFILE environment variable allows users to specify the name of a log file for capturing the standard output from the program. If this variable is not set, the standard output is written to the terminal and can be captured using the UNIX redirect command (“>”), as shown in the example above.

<a id=conc></a>
### CTM_CONC_1: CCTM hourly instantaneous concentration file
[Return to Table 3-6](#conc_t)

The 3-D CCTM hourly concentration file (CONC) contains gas-phase species mixing ratios (ppmV) and aerosol species concentrations (µg m<sup>-3</sup>). CONC files include instantaneous model species concentrations at the end of each model hour. The number and types of species contained in the CONC files depend on the chemical mechanism and aerosol model configurations that are selected when the CCTM is compiled. The [FORTRAN NameLists](#matrix_nml) within the mechanism directories list the modeled species, and contain a column that specifies which species are written to the CONC files. The GC_*mechname*.nml file lists the gas-phase species, the AE_*mechname*.nml file lists the aerosol species, and the NR_*mechname*.nml lists the nonreactive (inert) species. Species can be removed from the CONC file by editing the CONC column in the NameList file(s) to reduce the number of species that are written to, and thus the size of the CONC file.

<a id=cgrid></a>
### CTM_CGRID_1: CCTM restart file
[Return to Table 3-13](#cgrid_t)

The 3-D CCTM ending concentration file (CGRID) is the CCTM restart file. Containing gas-phase species mixing ratios (ppmV) and aerosol species concentrations (µg m<sup>-3</sup>), the CGRID file includes model species concentrations at the end of each simulation period. The number and types of species contained in the output CGRID files depend on the chemical mechanism and aerosol model configurations that are selected when CCTM is compiled. This file can be used to initialize CCTM from the simulation period that the model completed. For example, if the CCTM is configure to produce daily output files, a CGRID file will be written out at the end of each simulation day.

<a id=aconc></a>
### CTM_ACONC_1: CCTM hourly average concentration file
[Return to Table 3-13](#aconc_t)

The 3-D CCTM integral average concentration file (ACONC) contains average model species concentrations for each model hour, as opposed to instantaneous concentrations at the end of each output time step. The species written to the ACONC file are set by the user in the CCTM run script using the variable AVG_CONC_SPCS. The model layers that are used to calculate the integral average concentration are also set in the CCTM run script using the variable ACONC_BLEV_ELEV, where BLEV corresponds to the bottom layer number and ELEV corresponds to the top layer number. An example setting for the ACONC_BLEV_ELEV variable is “1 6”, which defines layers 1 through 6 as the vertical extent over which to calculate hourly average concentrations.

<a id=drydep></a>
### CTM_DRY_DEP_1: CCTM hourly cumulative dry deposition file
[Return to Table 3-13](#drydep_t)

The 2-D CCTM dry deposition file (DRYDEP) includes cumulative hourly dry deposition fluxes (kg hectare<sup>-1</sup>) for selected model species. CCTM calculates dry deposition for all of the species listed in the dry deposition column of the [FORTRAN NameLists](#matrix_nml) within the mechanism directories. The GC_*mechname*.nml file lists the gas-phase species, the AE_*mechname*.nml file lists the aerosol species, and the NR_*mechname*.nml lists the nonreactive (inert) species. Species can be removed from the dry deposition file by editing the DDEP column in the NameList file(s).

<a id=wetdep></a>
### CTM_WETDEP_1: CCTM hourly cumulative wet deposition file
[Return to Table 3-13](#wetdep_t)

The 2-D CCTM wet deposition file (WETDEP) includes cumulative hourly wet deposition fluxes (kg hectare<sup>-1</sup>) for selected model species. CCTM calculates wet deposition for all of the species listed in the wet deposition column of the [FORTRAN NameLists](#matrix_nml) within the mechanism directories. The GC_*mechname*.nml file lists the gas-phase species, the AE_*mechname*.nml file lists the aerosol species, and the NR_*mechname*.nml lists the nonreactive (inert) species. Species can be removed from the wet deposition file by editing the WDEP column in the NameList file(s).

<a id=vis></a>
### CTM_VIS_1
[Return to Table 3-13](#vis_t)

<a id=avis></a>
### CTM_AVIS_1
[Return to Table 3-13](#avis_t)

## Diagnostic and Advanced CMAQ Output Files

Along with the basic outputs detailed in the previous section, CMAQ can be configured to output several auxiliary files for diagnosing model performance.

<a id=pmdiag></a>
### CTM_PMDIAG_1: Instantaneous hourly aerosol diagnostics file
[Return to Table 3-13](#pmdiag_t)

This diagnostic file contains information on the geometric mean diameters and geometric standard deviations for the lognormal modes.

<a id=apmdiag></a>
### CTM_APMDIAG_1: Average hourly aerosol diagnostics file
[Return to Table 3-13](#apmdiag_t)


This diagnostic file contains information on the geometric mean diameters and geometric standard deviations for the lognormal modes.

<a id=b3gts></a>
### B3GTS_S: Biogenic emissions diagnostic file
[Return to Table 3-13](#b3gts_t)

This optional 2-D CCTM hourly output file contains calculated biogenic emissions in mass units. The B3GTS_S file will be produced only if in-line biogenic emissions are being calculated by CCTM and if the B3GTS_DIAG variable is turned on.

<a id=depv></a>
### CTM_DEPV_DIAG: CCTM inline deposition diagnostics file
[Return to Table 3-13](#depv_t)

This 2-D CCTM file contains the deposition velocity (m/s) for each chemical species calculated for the final time step for the hour.

<a id=pt3d></a>
### CTM_PT3D_DIAG: CCTM PT3D diagnostics file
[Return to Table 3-13](#pt3d_t)

The PT3D diagnostics file records the 3-D point source emissions as a linear average over the output timestep.

<a id=dust></a>
### CTM_DUST_EMIS_1
[Return to Table 3-13](#dust_t)

This optional 2-D CCTM hourly output file contains calculated dust emissions in mass units. The DUST_EMIS_1 file will be produced only if in-line windblown dust emissions are being calculated by CCTM and if the CTM_DUSTEM_DIAG variable is turned on.

<a id=aod></a>
### CTM_AOD_1
[Return to Table 3-13](#aod_t)

<a id=ipr></a>
### CTM_IPR_[1-3]
[Return to Table 3-13](#ipr_t)

The 3-D CCTM integrated process rate files (IPR) contains hourly concentrations of selected model output species in terms of the model process that contributed to the predicted concentration at each hour. For each grid cell in the process analysis domain (which is most likely a subset of the full modeling domain), the IPR file shows the hourly change in species concentration that is due to particular source/sink processes in the model. The input file procan.inp is used to set the model species for which to capture process analysis information, and the processes to track during the process analysis.

<a id=irr></a>
### CTM\_IRR_[1-3] Process analysis output – integrated reaction rates
[Return to Table 3-13](#irr_t)

The 3-D CCTM integrated reaction rate file (IRR) contains hourly concentrations of selected model output species in terms of the gas-phase chemistry pathways that contributed to the predicted concentration at each hour. For each grid cell in the process analysis domain (which is most likely a subset of the full modeling domain), the IRR file shows the hourly change in species concentration that is due to particular gas-phase chemistry reactions or reaction groups. The input file procan.inp is used to select the process analysis domain, the model species for which to capture process analysis information, and the chemistry reactions or groups of reactions to track during the process analysis.

<a id=floor></a>
### FLOOR: concentration-reset diagnostics file
[Return to Table 3-13](#floor_t)

FLOOR files are optional output diagnostic files which list specific gridboxes/timesteps in which species with negative concentrations are reset to zero.

<a id=media></a>
### MEDIA_CONC: Bidirectional soil NH4+ restart file
[Return to Table 3-13](#media_t)

This 2-D CCTM file contains the soil NH<sub>4</sub> and pH concentrations if using the bidirectional NH<sub>3</sub> option and/or the soil, vegetation and water Hg concentrations. This file is used to initialize the next day of the model simulation.

<a id=depv_mos></a>
### CTM_DEPV_MOS
[Return to Table 3-13](#depv_mos_t)

This 3-D CCTM file contains the deposition velocity (m s<sup>-1</sup>) for the final time step of the hour for each land use type within a grid cell. This output file is structured with the land use category being the 3rd dimension (i.e. equivalent to the layers in a concentration file). So, for model runs using the NLCD land use category system, the files will have 40 "layers".  

<a id=ctm_dry_depv_mos></a>
### CTM_DRY_DEPV_MOS
[Return to Table 3-13](#ctm_dry_depv_mos_t)

This 3-D CCTM file contains the total deposition (kg hectare<sup>-1</sup>) for the hour for each land use type within each grid cell. This output file is structured with the land use category being the 3rd dimension (i.e. equivalent to the layers in a concentration file). So, for model runs using the NLCD land use category system, the files will have 40 "layers".  

<a id=depv_fst></a>
### CTM_DEPV_FST
[Return to Table 3-13](#depv_fst_t)

This 3-D CCTM file contains the deposition velocity (m s<sup>-1</sup>) through the stomatal pathway for the final time step of the hour for each land use type within a grid cell. This output file is structured with the land use category being the 3rd dimension (i.e. equivalent to the layers in a concentration file). So, for model runs using the NLCD land use category system, the files will have 40 "layers".  


<a id=ctm_dry_depv_fst></a>
### CTM_DRY_DEPV_FST
[Return to Table 3-13](#ctm_dry_depv_fst_t)

This 3-D CCTM file contains the total deposition (kg hectare<sup>-1</sup>) through the stomatal pathway for the hour for each land use type within each grid cell. This output file is structured with the land use category being the 3rd dimension (i.e. equivalent to the layers in a concentration file). So, for model runs using the NLCD land use category system, the files will have 40 "layers".  

<a id=ctm_vdiff_diag></a>
### CTM_VDIFF_DIAG
[Return to Table 3-13](#ctm_vdiff_diag_t)

The VDIFF_DIAG file provides diagnostic output of vertical dispersion parameters.  It is controlled by the VDIFF_DIAG_FILE environment variable.


<a id=ctm_vsed_diag></a>
### CTM_VSED_DIAG
[Return to Table 3-13](#ctm_vsed_diag_t)

The VSED_DIAG file provides diagnostic output of particle gravitational settling velocities. It is controlled by the VDIFF_DIAG_FILE environment variable and activated when gravitational sedimentation is turned on.

<a id=ltnghourly></a>
### LTNG_DIAG1
[Return to Table 3-13](#ltnghourly_t)

Hourly 3-D lightning NO emissions calculated in-line by the CCTM.

<a id=ltngcol></a>
### LTNG_DIAG2
[Return to Table 3-13](#ltngcol_t)

Hourly column-total lightning NO emissions calculated in-line by the CCTM.

<a id=play_srcid></a>
### PLAY_SRCID
[Return to Table 3-13](#play_srcid_t)

<a id=ctm_rj></a>
### CTM\_RJ_[1-2]: In-line photolysis output – gridded photolysis rates
[Return to Table 3-13](#ctm_rj_t)

The photolysis diagnostic output files (RJ) contain the photolysis rates calculated by CCTM when the in-line photolysis option is used.

<a id=soilout></a>
### SOILOUT
[Return to Table 3-13](#soilout_t)

Name and location of hourly soil NO emissions file; output when in-line biogenic emissions processing is activated by setting CTM_BIOGEMIS to “T” or “Y”.

<a id=ssemis></a>
### CTM_SSEMIS_1: Sea salt emissions diagnostic file
[Return to Table 3-13](#ssemis_t)

This optional 2-D CCTM hourly output file contains calculated sea salt emissions. The SSEMIS file will be produced by CCTM only if the CTM_SSEMDIAG variables are turned on.

<a id=wetdep2></a>
### CTM_WETDEP_2: CCTM cloud diagnostics file
[Return to Table 3-13](#wetdep2_t)

In CMAQ, wet deposition is calculated separately for resolved (grid-scale) clouds and for convective (subgrid) clouds. The WETDEP1 file contains the total wet deposition, i.e., the sum of both resolved-scale and subgrid-scale deposition. The WETDEP2 file contains only subgrid-scale deposition, plus some cloud diagnostic variables. The 2-D CCTM wet deposition file (WETDEP2) includes cumulative hourly wet deposition fluxes (kg hectare<sup>-1</sup>) for selected model species. CCTM calculates wet deposition for all of the species listed in the wet deposition column of the FORTRAN Namelist files within the mechanism directories. The GC_*mechname*.nml file lists the gas-phase species, the AE_*mechname*.nml file lists the aerosol species, and the NR_*mechname*.nml lists the nonreactive (inert) species. Species can be removed from the wet deposition file by editing the WDEP column in the NameList file(s).


[<< Previous Chapter](CMAQ_UG_ch05_running_CMAQ.md) - [Home](README.md) - [Next Chapter >>](CMAQ_UG_ch07_analysis_tools.md)<br>
CMAQ User's Guide (c) 2019<br>

<!-- END COMMENT -->
