---
r_version: 4.4.0
r_path_version: 4.4
---
# R

[R](https://www.r-project.org/about.html){:target="_blank"} is a language and environment for statistical computing and graphics. The cluster has multiple versions installed with a variety of commonly used packages pre-installed.

## Package Installation

R uses a central package library that contains many common packages. The location of this library is `$R_HOME/library`. Users may also install their own packages locally. The default location for local package installation is `$HOME/R/x86_64-pc-linux-gnu-library/{{ r_path_version }}`.

!!! warning "Check installed package list first!"
    Your package installation command will not check the centrally installed packages. You should always check if your package is already installed before proceeding. RCC provides an up-to-date [list of installed R packages](r-pkg-list.md){:target="_blank"} for the most recent version.

### User Package Install

First load the R module:

```bash
module load R
```

Launch R:

```bash
R
```

Run the install command:

```R
> install.packages('SomePkg')
```

The first attempt will warn about writing to the central library. It will ask you to create and use a personal library. Answer **yes** to both.

```R
Installing package into ‘/hpc/apps/R/{{ r_version }}/lib64/R/library’
(as ‘lib’ is unspecified)
Warning in install.packages("ggplot2") :
  'lib = "/hpc/apps/R/{{ r_version }}/lib64/R/library"' is not writable
Would you like to use a personal library instead? (yes/No/cancel) yes
Would you like to create a personal library
‘~/R/x86_64-pc-linux-gnu-library/{{ r_path_version }}’
to install packages into? (yes/No/cancel) yes
```

Your package will be installed to your home directory. This package can be removed or updated using the standard R commands.

!!! warning "Did you check the installed package list first?"
    Your package installation command will not check the centrally installed packages. You should always check if your package is already installed before proceeding. RCC provides an up-to-date [list of installed R packages](r-pkg-list.md){:target="_blank"} for the most recent version.

### Request Package Install

Some packages require system libraries and/or advanced dependencies in order to install correctly. If you see errors when you're installing a package, send a package install request to {{ support_email }} and RCC will update the central library.

## Running R Jobs

R can be run in batch or interactive jobs. Please do not run long or resource intensive R scripts on the login node.  

### Small Interactive Jobs

Small interactive jobs include light plotting, simple analysis of small data sets, etc. These jobs never take more than one core, a few GB of memory, and never last more than a few minutes. These small, fast jobs are allowed on a  login node. However, use caution when running these jobs and double-check that they will not use larger resources. If you need to run an interactive R workflow that is more resource intensive, please use an interactive cluster job.

To get an interactive session to run R on the cluster:

```bash
srun --ntasks=1 --mem-per-cpu=4GB --time=01:00:00 --job-name=interactive --pty bash
```

### Multi-core Jobs

Multi-core jobs should be run on the cluster compute nodes using the Torque queuing system. There are several options for running these jobs in Torque, including the Rscript command and the BatchJobs library. Both methods interface R with Torque, however, their use cases are different. The Rscript command should be used when you have written an R program .r file and would like to run this script on the cluster. The BatchJobs library should be used when you would like to test individual functions in a semi-interactive way and submit this work to the cluster.

Example SLURM submission script:
=== "R-test.slurm"

```txt
#!/bin/bash
#SBATCH --job-name=R-test
#SBATCH --ntasks=1
#SBATCH --mem-per-cpu=1gb
#SBATCH --time=00:01:00
#SBATCH --output=%x-%j.out
 
module load R/{{ r_version }}
 
Rscript Rtest.r  
```

Submit the job:

```bash
sbatch myRtest.sh
```

<!--===BatchJobs===
BatchJobs is an R library that interfaces the R command-line with the cluster's Torque queuing system.

Load and start R:
 $ module load R/4.0.4       
 $ R                      

Load R BatchJobs library:
 > library(BatchJobs)       
 Loading required package: BBmisc
 Sourcing configuration file: '/hpc/apps/R/4.0.4/lib64/R/library/BatchJobs/etc/BatchJobs_global_config.R'
 BatchJobs configuration:
   cluster functions: Torque
   mail.from: 
   mail.to: 
   mail.start: none
   mail.done: none
   mail.error: none
   default.resources: nodes=1, cores=1, memory=5gb, walltime=8:00:00
   debug: FALSE 
   raise.warnings: FALSE
   staged.queries: TRUE
   max.concurrent.jobs: Inf
   fs.timeout: NA
 
Define data and function:
 > my_data <- (1:10)                       
 > my_func <- function(x) x^2 

Define an object to store jobs (creates a directory "batchtest-files"):         
 > reg <- makeRegistry(id = "batchtest")  

Map data and function to jobs in "batchtest" object:
 > jobs <- batchMap(reg, my_func, my_data)       

Submit jobs to cluster (change nodes, cores, mem, and walltime to fit needs):
 > jobsubmit <- submitJobs(reg, resources = list(nodes = 1, cores = 1, mem = 5gb, walltime = 8:00:00))

Check job results:
 > reduceResultsVector(reg, fun = function(job, res) res, progressbar = FALSE)
 Syncing registry ...
 Reducing 10 results...
   1   2   3   4   5   6   7   8   9  10 
   1   4   9  16  25  36  49  64  81 100

Results files:                                                
 batchtest-files/                                             # Results can be displayed in vector format as shown above. Users may also 
 |-- BatchJobs.db                                             # want to view results in the output files. Each job registry object that  
 |-- conf.RData                                               # is created starts a new file tree as shown here with the name            
 |-- exports                                                  # ''registryname''-files. Job results are located in numbered directories
 |-- functions                                                # within the ''jobs'' directory. Output files are named ''jobnumber''.out   
 |   `-- c0000e09480f70b05365872c2e90ce8a.RData               # and are numbered in the order of submission.
 |-- jobs                                                                                  
 |   |-- 01                                                   
 |   |   |-- 1-result.RData
 |   |   |-- 1.R
 |   |   `-- 1.out
 |   |-- 02
 |   |   |-- 2-result.RData
 |   |   |-- 2.R
 |   |   `-- 2.out
 |   |-- ...
 |
 |-- pending
 |-- registry.RData
 `-- resources
     `-- resources_1493676907.RData
-->

## Help

If you have questions about running R on the HPC cluster, please contact {{ support_email }} for assistance.

--8<-- "includes/abbreviations.md"
