## Website

<https://github.com/deepmind/alphafold> Fork we are using since we don't
support Docker: <https://github.com/amorehead/alphafold_non_docker>

# HPC Cluster Updates

03/11/2022: Version 2.1.2 including multimer support. AlphaFold is
installed on the HPC Cluster using Miniconda3 virtual environments. This
is a working alternative to the native docker container they provide,
since we are unable to run docker on the RCC.

For information on running alphafold (similar to these wiki
instructions), check their

<html>

<a href="https://github.com/amorehead/alphafold_non_docker">website</a>

</html>

, and you can also get module help information with
`module help alphafold`.

# Reference Files

AlphaFold downloaded reference files are located at:
`/hpc/refdata/alphafold` This path is set for you in the alphafold
software/module as the default when you use the -d $DOWNLOAD_DIR

# Example AlphaFold Interactive job

``` bash
[bgizelar@ln01 ~]$ srun --job-name=alphafold_test --ntasks=8 --mem=30gb --time=8:00:00 --gres=gpu:1 --pty bash
[bgizelar@gn03 ~]$ module load alphafold
[bgizelar@gn03 ~]$ export NVIDIA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES
[bgizelar@gn03 ~]$ run_alphafold.sh -d $DOWNLOAD_DIR -f /scratch/u/{NetID}/alphatest/some.fasta -t 2020-05-14 -o /scratch/u/{NetID}/alphatest/output
[bgizelar@gn03 ~]$ exit
[bgizelar@ln02 ~]$
```

The example above is run as a normal interactive SLURM job. SLURM will
assign a GPU node for interactive use, in this case `gn03`. The
alphafold software environment is loaded using `module load alphafold`
command. When the interactive job ends, SLURM exits the shell back to
the original login shell. Please make sure to insert your username for
`{NetID}`.

# Example AlphaFold batch job

First create a file `/scratch/u/{NetID}/alphatest/my_af_script.sh`
containing the following. Replace {NetID} with your userid.

`#!/bin/bash`  
`#SBATCH --job-name=alphafold_test`  
`#SBATCH --ntasks=8`  
`#SBATCH --mem=30gb`  
`#SBATCH --time=08:00:00`  
`#SBATCH --output=%x-%j.out`  
`#SBATCH --gres=gpu:1`  
`#SBATCH --partition=gpu`  
  
`module load alphafold/2.1.2`  
  
`export NVIDIA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES`  
`run_alphafold.sh -d $DOWNLOAD_DIR -f /scratch/u/{NetID}/alphatest/some.fasta -t 2020-05-14 -o /scratch/u/{NetID}/alphatest/output`

Submit the job:

`[bgizelar@ln01 ~]$ cd /scratch/u/{NetID}/alphatest && sbatch my_af_script.sh`

The example above is a job request for 1 node, 8 processors, and 1 gpu.
Notice that the job is run within the user's scratch directory. Please
make sure to insert your username for `{NetID}.`. Alphafold doesn't
parallelize on GPUs so you should only run on 1 GPU.
