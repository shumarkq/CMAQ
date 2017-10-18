run_CMAQ_eval_AMET
========

This shell script controls execution of multiple post-processing and evaluation steps including running combine, sitecmp, sitecmp_dailyo3, loading matched model/obs data (i.e. sitecmp files) into the AMET database and creating AMET "batch" evaluation plots.

## Running on atmos
SLURM header at the top of the shell script is used to control execution of the runscript on the atmos cluster.
* User should change lines with *--gid* and *--output* to reflect their account on atmos.
* User should **not** change the *--partion=singlepe* if they wish to access the AMET database.  T

## Setting environment variables
The runscript is divided into different numbered sections.  
```
