# TensorFlow

TensorFlow on HPC can be run in batch, interactive, or Jupyter Notebook.

## TensorFlow interactive job

Start the job.

```bash
srun --job-name=tensorflow --ntasks=1 --time=1:00:00 --gres=gpu:1 --pty bash
```

Load the TensorFlow module.

```bash
module load tensorflow
```

Start your training or other commands.

```bash
python train.py options input output
```

When your commands end, always remember to end the interactive job.

```bash
exit
```

## TensorFlow batch job

This is an example of running a Python script in a SLURM batch job.

<!-- markdownlint-disable MD046 -->
=== "learning-ml.py"

    ```python
    import tensorflow as tf
    hello = tf.constant('Hello, TensorFlow!')
    session = tf.Session()
    tf.print(hello)
    ```

=== "learning-ml.slurm"

    ```txt
    #!/bin/bash
    #SBATCH --job-name=learning
    #SBATCH --ntasks=1
    #SBATCH --time=00:01:00
    #SBATCH --account={PI_NetID}
    #SBATCH --partition=gpu
    #SBATCH --gres=gpu:1
    #SBATCH --output=%x-%j.out

    module load tensorflow
    python /scratch/g/pi_netid/learning-ml.py >> output.log  
    ```
<!-- markdownlint-enable MD046 -->

Save these scripts to your scratch directory, and submit the job:

```bash
sbatch learning-ml.slurm
```

This simple job should print *Hello, TensorFlow!* to your output file.

## TensorFlow Jupyter Notebook job

This functionality is provided by Open OnDemand!

See [Jupyter on Open OnDemand](../cluster/access/ondemand.md#jupyter-notebook-example) for details.

## TensorBoard Job

This functionality is provided by Open OnDemand!

--8<-- "includes/abbreviations.md"
