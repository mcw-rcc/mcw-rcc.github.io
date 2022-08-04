# TensorFlow
TensorFlow on HPC can be run in batch, interactive, or Jupyter Notebook.

## TensorFlow interactive job
```bash
$ srun --job-name=tensorflow --ntasks=1 --time=1:00:00 --gres=gpu:1 --pty bash
$ module load tensorflow
(tensorflow-2.4.1) $ python
Python 3.8.5 (default, Sep  4 2020, 07:30:14)
[GCC 7.3.0] :: Anaconda, Inc. on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
2021-03-04 10:33:09.763458: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudart.so.11.0
>>> hello = tf.constant('Hello, TensorFlow!')
>>> session = tf.Session()
>>> tf.print(hello)
Hello, TensorFlow!
>>> exit()
(tensorflow-2.4.1) $ exit
```

The example above is run as a normal interactive SLURM job. SLURM will assign a node for interactive use, in this case `gn03`. The TensorFlow software environment is loaded using `module load tensorflow` command. A Python instance is used to run a simple *Hello, TensorFlow!* example. Lastly, exit the python interpreter and close the interactive job. When the interactive job ends, SLURM exits the shell back to the original login shell.

## TensorFlow batch job
This is an example of running a Python script in a SLURM batch job.
=== "learning-ml.py"
    ```python
    import tensorflow as tf
    hello = tf.constant('Hello, TensorFlow!')
    session = tf.Session()
    tf.print(hello)
    ```

=== "learning-ml.slurm"
    ```bash
    #!/bin/bash
    #SBATCH --job-name=learning
    #SBATCH --ntasks=1
    #SBATCH --time=00:01:00
    #SBATCH --account={PI_NetID}
    #SBATCH --partition=gpu
    #SBATCH --gres=gpu:1
    #SBATCH --output=%x-%j.out
    
    module load tensorflow                   
    python /scratch/u/user/learning-ml.py >> output.log  
    ```


Save these scripts to your scratch directory, and submit the job:
```
$ sbatch learning-ml.slurm
```

This simple job should print *Hello, TensorFlow!* to your output file.

## TensorFlow Jupyter Notebook job
This functionality is provided by Open OnDemand!

See [Jupyter on Open OnDemand](../user-guide/access/ondemand.md#jupyter-notebooks) for details.

## TensorBoard Job
This functionality is provided by Open OnDemand!

--8<-- "includes/abbreviations.md"