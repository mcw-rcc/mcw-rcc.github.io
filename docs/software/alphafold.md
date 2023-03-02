# AlphaFold

!!! info "Updates"
    03/11/2022: Version 2.1.2 including multimer support.

AlphaFold can accurately predict 3D models of protein structures and is accelerating research in nearly every field of biology.

***- [Deepmind](https://www.deepmind.com/research/highlighted-research/alphafold)***

AlphaFold is installed on the HPC Cluster using conda virtual environments. The [code version](https://github.com/amorehead/alphafold_non_docker) we use is an alternative to the native docker container they provide.

## Reference Files

AlphaFold downloaded reference files are located at `/hpc/refdata/alphafold`. This path is set for you in the alphafold software module by default when you use the `-d $DOWNLOAD_DIR` flag.

## Example AlphaFold interactive job

```bash
# start interactive job
srun --job-name=alphafold_test --ntasks=8 --mem=30gb --time=8:00:00 --gres=gpu:1 --pty bash

# load softare on compuate node
module load alphafold/2.1.2

# tell alphafold about the GPU
export NVIDIA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES

# start alphafold software, replace netid with your username
run_alphafold.sh -d $DOWNLOAD_DIR -f /scratch/u/NetID/alphatest/some.fasta -t 2020-05-14 -o /scratch/u/NetID/alphatest/output
```

The example above is run as a normal interactive SLURM job. SLURM will assign a GPU node for interactive use. The alphafold software environment is loaded using `module load alphafold/2.1.2` command. Please make sure to insert your username for
`NetID`.

## Example AlphaFold batch job

Create a file `/scratch/u/NetID/alphafold.slurm` containing the following. Replace NetID with your username.

=== "alphafold.slurm"

```txt
#!/bin/bash
#SBATCH --job-name=alphafold_test
#SBATCH --ntasks=8
#SBATCH --mem=30gb
#SBATCH --time=08:00:00
#SBATCH --output=%x-%j.out
#SBATCH --gres=gpu:1`  
#SBATCH --partition=gpu
  
module load alphafold/2.1.2
  
export NVIDIA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES
run_alphafold.sh -d $DOWNLOAD_DIR -f /scratch/u/{NetID}/alphatest/some.fasta -t 2020-05-14 -o /scratch/u/{NetID}/alphatest/output
```

Submit the job:

```bash
cd /scratch/u/NetID && sbatch alphafold.slurm
```

The example above is a job request for 1 node, 8 processors, and 1 gpu. Notice that the job is run within the user's scratch directory. Please make sure to insert your username for `NetID`. **Alphafold 2.1.2 doesn't parallelize on GPUs so you should only run on 1 GPU**

## Help

Specific help information is available on [GitHub](https://github.com/amorehead/alphafold_non_docker). Additional command-line help can be printed with `module help alphafold/2.1.2`.
