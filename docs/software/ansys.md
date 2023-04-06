# ANSYS

ANSYS is a simulation software with a variety of functions including finite-element analysis and computational fluid dynamics.

!!! info "Licensed Software"
    ANSYS is resticted to licensed users. Please contact {{ support_email }} with questions.

## Multi-node Fluent job

Fluent should be run from the ANSYS Workbench if possible, as this is the easiest method. However, if you need more resources than 1 node with 48 cores, you can run Fluent in a batch job on the cluster.

To get started, setup some file structure. Everytime we run a simulation, we should start with a new parent folder, which is `example-job` in this case. Make sure this is meaningful, since it will help you identify your simulations later.

```bash
cd /scratch/g/PI_NetID
mkdir -p example-job/{cas-files,output}
```

We created a new folder for our first simulation `example-job` in our group scratch directory. Within that folder, we also created a place to hold our input and output files. Upload the input cas files to the `cas-files` directory.

We need a journal file to tell Fluent how to proceed. Create a file `/scratch/g/PI_NetID/example-job/fluent-test.jou`.

=== "fluent-test.jou"

```txt
/file/read-case-data "/scratch/g/PI_NetID/example-job/cas-files/test_fluent_case"
/solve/iterate 1000
/file/write-case ""/scratch/g/PI_NetID/example-job/output/test_fluent_case.cas"
/file/write-data "/scratch/g/PI_NetID/example-job/output/test_fluent_case.data"
/parallel timer usage
/exit
yes
```

We also need a job script. Create a file `/scratch/g/PI_NetID/example-job/job-script.slurm`. Replace PI_NetID with your PI's username.

=== "job-script.slurm"

```txt
#!/bin/bash
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=16
#SBATCH --job-name=test-fluent-case
#SBATCH --output=%x-%j.out
#SBATCH --time=168:00:00

# this is where we load software
module load impi
module load ansys/2022R1

export FLUENT_AFFINITY=0                             
export SLURM_ENABLED=1                               
export SCHEDULER_TIGHT_COUPLING=1                    
FL_SCHEDULER_HOST_FILE=slurm.${SLURM_JOB_ID}.hosts  
/bin/rm -rf ${FL_SCHEDULER_HOST_FILE}               
scontrol show hostnames "$SLURM_JOB_NODELIST" >> $FL_SCHEDULER_HOST_FILE 
time fluent -g 3ddp -t${SLURM_NTASKS} -i fluent-mpi.jou -cnf=${FL_SCHEDULER_HOST_FILE}
```

Submit the job:

```bash
cd /scratch/g/PI_NetID/example-job
sbatch job-script.slurm
```

The example above is a job request for 2 nodes with 16 cores per node. Please make sure to insert your PI's username for `PI_NetID`.
