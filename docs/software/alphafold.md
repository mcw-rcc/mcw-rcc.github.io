# AlphaFold

[AlphaFold](https://deepmind.google/technologies/alphafold/){:target="_blank"} is an AI system developed by Google DeepMind that predicts a protein’s 3D structure from its amino acid sequence.

## AlphaFold 2

The AlphaFold 2 workflow has two parts, MSA search which is CPU based, and inference/MD which is GPU based. To maximize resources, split your workflow into two jobs.

### Interactive Job

You should only use interactive jobs for testing/debugging.  Please submit batch jobs otherwise.

Start a CPU job for MSA:

```bash
# start interactive job
srun --job-name=alphafold_test --ntasks=1 --cpus-per-task=12 --time=8:00:00 --pty bash

# load required modules on the gpu node
module load alphafold/2.3.2
module load python/3.9.1

# start alphafold software, replace PI_NetID with your username
run_alphafold.py -d $DOWNLOAD_DIR --cpus 12 -f /scratch/g/PI_NetID/alphatest/some.fasta -t 2020-05-14 -o /scratch/g/PI_NetID/alphatest/output --run_feature=1
```

The alphafold software environment is loaded using `module load alphafold/2.3.2` command. AlphaFold 2 reference files are located at `/hpc/refdata/alphafold/2.3`. This is set automatically by the module, so do not change `-d $DOWNLOAD_DIR`. Please make sure to edit `PI_NetID`. The `--run_feature=1` flag causes AlphaFold to run the MSA search only.

Start a GPU job for model inference and MD:

```bash
# start interactive job
srun --job-name=alphafold_test --ntasks=1 --cpus-per-task=12 --time=8:00:00 --partition=gpu --gres=gpu:1 --gpu_cmode=shared --pty bash

# load required modules on the gpu node
module load alphafold/2.3.2
module load python/3.9.1

# tell alphafold about the GPU
export NVIDIA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES

# start alphafold software, replace netid with your username
run_alphafold.py -d $DOWNLOAD_DIR --cpus 12 -f /scratch/g/PI_NetID/alphatest/some.fasta -t 2020-05-14 -o /scratch/g/PI_NetID/alphatest/output --use_gpu_relax
```

This second job will use the results of first step. The `--use_gpu_relax` flag will ensure that MD uses the GPU.

!!! info "GPU Compute Mode"
    The `#SBATCH --gpu_cmode=shared` slurm header is required or your job will fail halfway through. Alphafold 2.3.2 doesn't parallelize on GPUs so you should only run on 1 GPU.

### Batch Job

Create a file `/scratch/g/PI_NetID/alphatest/alphafold_step1.slurm` for the MSA step. Replace `PI_NetID` with your PI's Username.

=== "alphafold_step1.slurm"

```txt
#!/bin/bash
#SBATCH --job-name=alphafold_test
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=12
#SBATCH --time=08:00:00
#SBATCH --output=%x-%j.out
  
module load alphafold/2.3.2
module load python/3.9.1
  
export NVIDIA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES
run_alphafold.py -d $DOWNLOAD_DIR --cpus 12 -f /scratch/g/PI_NetID/alphatest/some.fasta -t 2020-05-14 -o /scratch/g/PI_NetID/alphatest/output --run_feature=1
```

Submit the job:

```bash
cd /scratch/g/PI_NetID/alphatest && sbatch alphafold_step1.slurm
```

Create a file `/scratch/g/PI_NetID/alphatest/alphafold_step2.slurm` for the inference/MD step.

=== "alphafold_step2.slurm"

```txt
#!/bin/bash
#SBATCH --job-name=alphafold_test
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=12
#SBATCH --time=08:00:00
#SBATCH --output=%x-%j.out
#SBATCH --gres=gpu:1
#SBATCH --gpu_cmode=shared  
#SBATCH --partition=gpu
  
module load alphafold/2.3.2
module load python/3.9.1
  
export NVIDIA_VISIBLE_DEVICES=$CUDA_VISIBLE_DEVICES
run_alphafold.py -d $DOWNLOAD_DIR --cpus 12 -f /scratch/g/PI_NetID/alphatest/some.fasta -t 2020-05-14 -o /scratch/g/PI_NetID/alphatest/output --use_gpu_relax
```

Submit the job:

```bash
cd /scratch/g/PI_NetID/alphatest && sbatch alphafold_step2.slurm
```

## AlphaFold 3

AlphaFold3 is an improvement in functionality and performance. Please note the new license requirements to access required model parameters and change in input file syntax.

### Obtaining Model Parameters

AlphaFold3 is installed on the cluster but does not include the required model parameters, which are subject to [Terms of Use](https://github.com/google-deepmind/alphafold3/blob/main/WEIGHTS_TERMS_OF_USE.md){:target="_blank"}. In order to comply, each user wanting to use AlphaFold 3 must [submit a request](https://forms.gle/svvpY4u2jsHEwWYS6) to DeepMind to obtain their own personal copy of the model parameters. Research Computing cannot obtain the model parameters for you. Please ensure that you read and fully understand the Terms of Use agreement prior to request and download.

If approved, you will receive a download link to a ~1 GB file. Place that in your home directory, e.g. `$HOME/af3`. Per the Terms of Use, you may not share these files with anyone else, including other members of your lab.

### Input JSON

AlphaFold 3 requires a JSON format input file. Use the following example from DeepMind to test the software.

=== "fold_input.json"

```json
{
  "name": "2PV7",
  "sequences": [
    {
      "protein": {
        "id": ["A", "B"],
        "sequence": "GMRESYANENQFGFKTINSDIHKIVIVGGYGKLGGLFARYLRASGYPISILDREDWAVAESILANADVVIVSVPINLTLETIERLKPYLTENMLLADLTSVKREPLAKMLEVHTGAVLGLHPMFGADIASMAKQVVVRCDGRFPERYEWLLEQIQIWGAKIYQTNATEHDHNMTYIQALRHFSTFANGLHLSKQPINLANLLALSSPIYRLELAMIGRLFAQDAELYADIIMDKSENLAVIETLKQTYDEALTFFENNDRQGFIDAFHKVRDWFGDYSEQFLKESRQLLQQANDLKQG"
      }
    }
  ],
  "modelSeeds": [1],
  "dialect": "alphafold3",
  "version": 1
}
```

Please review the [AlphaFold 3 documentation](https://github.com/google-deepmind/alphafold3/blob/main/docs/input.md) for more info.

<!-- markdownlint-disable MD024 -->
### Interactive Job

You should only use interactive jobs for testing/debugging.  Please submit batch jobs otherwise.

The syntax and input format has changed in AlphaFold 3. You are required to provide your own model parameters (`--model_dir`) as previously discussed. The `--db_dir` is provided with the install and should not be edited. The `--json_path` will be your JSON format input file as previously discussed. The `--output_dir` can be set to anything, with the default being the current working directory.

Start a CPU job for MSA:

```bash
# start interactive job
srun --job-name=alphafold_test --ntasks=1 --cpus-per-task=12 --time=8:00:00 --pty bash

# load required modules
module purge
module load alphafold/3.0.1

# start alphafold software, replace PI_NetID with your username
apptainer exec /hpc/containers/alphafold_3.0.1.sif python /app/alphafold/run_alphafold.py --model_dir=$HOME/af3 --db_dir=$DATABASES_DIR --json_path=fold_input.json --output_dir=$PWD --norun_inference
```

The `--norun_inference` flag causes AlphaFold to run the MSA search only.

Start a GPU job for model inference and MD:

```bash
# start interactive job
srun --job-name=alphafold_test --ntasks=1 --cpus-per-task=12 --time=8:00:00 --partition=gpu --gres=gpu:1 --gpu_cmode=shared --pty bash

# load required modules
module purge
module load alphafold/3.0.1

# for V100 compatibility
export APPTAINERENV_XLA_FLAGS="--xla_disable_hlo_passes=custom-kernel-fusion-rewriter"

# start alphafold software

apptainer exec --nv /hpc/containers/alphafold_3.0.1.sif python /app/alphafold/run_alphafold.py --model_dir=$HOME/af3 --db_dir=$DATABASES_DIR --json_path=fold_data.json --output_dir=$PWD --flash_attention_implementation=xla --norun_data_pipeline
```

This second job will use the results data json output from the first step.  The --flash_attention_implementation=xla flag is for V100 GPU compatibility.

### Batch Job

Start a CPU job for MSA:

=== "alphafold_step1.slurm"

```txt
#!/bin/bash
#SBATCH --job-name=alphafold3_test
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=12
#SBATCH --time=08:00:00
#SBATCH --output=%x-%j.out

# load required modules
module purge
module load alphafold/3.0.1

apptainer exec /hpc/containers/alphafold_3.0.1.sif python /app/alphafold/run_alphafold.py \
    --model_dir=$HOME/af3 \
    --db_dir=$DATABASES_DIR \
    --json_path=fold_data.json \
    --output_dir=$PWD \
    --norun_inference
```

Start a GPU job for model inference and MD:

=== "alphafold_step2.slurm"

```txt
#!/bin/bash
#SBATCH --job-name=alphafold3_test
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=12
#SBATCH --time=08:00:00
#SBATCH --output=%x-%j.out
#SBATCH --gres=gpu:1
#SBATCH --gpu_cmode=shared  
#SBATCH --partition=gpu

# load required modules
module purge
module load alphafold/3.0.1

# for V100 compatibility
export APPTAINERENV_XLA_FLAGS="--xla_disable_hlo_passes=custom-kernel-fusion-rewriter"

apptainer exec --nv /hpc/containers/alphafold_3.0.1.sif python /app/alphafold/run_alphafold.py \
    --model_dir=$HOME/af3 \
    --db_dir=$DATABASES_DIR \
    --json_path=fold_data.json \
    --output_dir=$PWD \
    --flash_attention_implementation=xla \
    --norun_data_pipeline
```
