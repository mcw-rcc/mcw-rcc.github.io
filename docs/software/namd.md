# NAMD

NAMD is a parallel molecular dynamics code designed for simulation of large biomolecular systems.

## NAMD2 or NAMD3

NAMD3 was recently released and is installed on the cluster. The command options and syntax may be different from NAMD2 and it is up to the user to decide the appropriate version.

To list available NAMD versions:

=== "NAMD2"
    ```txt
    module avail namd2

    ------------------------------------------------------------ /hpc/modulefiles -------------------------------------------------------------
    namd2/2.14-cuda-mpi    namd2/2.14-mpi    namd2/2.14 (D)
    ```

=== "NAMD3"
    ```txt
    module avail namd3

    ------------------------------------------------------------ /hpc/modulefiles -------------------------------------------------------------
    namd3/3.0.1-cuda    namd3/3.0.1 (D)
    ```

Several versions are installed including parallel (mpi) and GPU (cuda) enabled options. The default module marked with `(D)` should be used in most cases for single-node simulations up to 48 cores.

To load NAMD in a job script:

=== "NAMD2"
    ```txt
    module load namd2/2.14
    ```

=== "NAMD3"
    ```txt
    module load namd3/3.0.1
    ```

Remember to adjust the version of NAMD required for your type of simulation.

## Batch job

To run NAMD in a batch job, first create a job script.

=== "namd2-job.slurm"
    ```txt
    #!/bin/bash
    #SBATCH --job-name=single-node
    #SBATCH --ntasks=1
    #SBATCH --cpus-per-task=8
    #SBATCH --time=72:00:00

    module load namd2/2.14

    ## Single node.
    ## This will start a simulation on 8 CPU cores with 72 hour time 
    ## limit. If you want to increase the number of CPU cores, adjust 
    ## the cpus-per-task parameter above. You need to update the namd 
    ## config and log output file names to suit your simulation.

    namd2 +p$SLURM_CPUS_PER_TASK +idlepoll namd_config_file.conf > namd_run_1.log
    ```

=== "namd3-job.slurm"
    ```txt
    #!/bin/bash
    #SBATCH --job-name=single-node
    #SBATCH --ntasks=1
    #SBATCH --cpus-per-task=8
    #SBATCH --time=72:00:00

    module load namd3/3.0.1

    ## Single node.
    ## This will start a simulation on 8 CPU cores with 72 hour time 
    ## limit. If you want to increase the number of CPU cores, adjust 
    ## the cpus-per-task parameter above. You need to update the namd 
    ## config and log output file names to suit your simulation.

    namd3 +p$SLURM_CPUS_PER_TASK +idlepoll namd_config_file.conf > namd_run_1.log
    ```
