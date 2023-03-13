# AlphaFold

!!! info "Updates"
    03/06/2023: Version 2.3.1 Adds new --cpus option to speed up tasks.  https://github.com/deepmind/alphafold/releases/tag/v2.3.0
    RCC is using this folk of alphafold to add singularity support: https://github.com/dialvarezs/alphafold

AlphaFold can accurately predict 3D models of protein structures and is accelerating research in nearly every field of biology.

***- [Deepmind](https://www.deepmind.com/research/highlighted-research/alphafold)***

AlphaFold is installed on the HPC Cluster using Singularity images. The [code version](https://github.com/dialvarezs/alphafold) we use is an alternative to the native docker container they provide.

## Reference Files

AlphaFold downloaded reference files are located at `/hpc/refdata/alphafold/2.3`. This path is set for you in the alphafold software module by default when you use the `-d $DOWNLOAD_DIR` flag.

## Example AlphaFold interactive job

```bash
# start interactive job
srun --job-name=alphafold_test --ntasks=1 --cpus-per-task=12 --mem=30gb --time=8:00:00 --gres=gpu:1 --gpu_cmode=shared --pty bash

# load required modules on the gpu node
module load alphafold/2.3.1
module load python/3.9.1

# tell alphafold about the GPU
export NVIDIA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES

# start alphafold software, replace netid with your username
run_alphafold.py -d $DOWNLOAD_DIR --cpus 12 -f /scratch/g/PI_NetID/alphatest/some.fasta -t 2020-05-14 -o /scratch/g/PI_NetID/alphatest/output
```

The example above is run as a normal interactive SLURM job. SLURM will assign a GPU node for interactive use. The alphafold software environment is loaded using `module load alphafold/2.3.1` command. Please make sure to insert your username for
`PI_NetID`.

## Example AlphaFold batch job

Create a file `/scratch/g/PI_NetID/alphatest/alphafold.slurm` containing the following. Replace PI_NetID with your PI's Username.

=== "alphafold.slurm"

```txt
#!/bin/bash
#SBATCH --job-name=alphafold_test
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=12
#SBATCH --mem=30gb
#SBATCH --time=08:00:00
#SBATCH --output=%x-%j.out
#SBATCH --gres=gpu:1
#SBATCH --gpu_cmode=shared  
#SBATCH --partition=gpu
  
module load alphafold/2.3.1
module load python/3.9.1
  
export NVIDIA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES
run_alphafold.py -d $DOWNLOAD_DIR --cpus 12 -f /scratch/g/PI_NetID/alphatest/some.fasta -t 2020-05-14 -o /scratch/u/PI_NetID/alphatest/output
```

Submit the job:

```bash
cd /scratch/g/PI_NetID/alphatest && sbatch alphafold.slurm
```

The example above is a job request for 1 node, 12 processors, and 1 gpu. The #SBATCH --gpu_cmode=shared slurm header is required or your job will fail halfway through. **Alphafold 2.3.1 doesn't parallelize on GPUs so you should only run on 1 GPU**

## Help

Specific help information is available on [GitHub](https://github.com/dialvarezs/alphafold). Additional command-line help can be printed with `module help alphafold/2.3.1`.
