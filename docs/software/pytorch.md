# PyTorch
PyTorch can be run in batch, interactive, or Jupyter Notebook. For more information, check the module help information with <code>module help pytorch</code>.

## PyTorch job
The following example will use PyTorch to train a network on the MNIST data set.

First, download the PyTorch examples:
```bash
$ git clone https://github.com/pytorch/examples.git
```

Now that you have the examples, use the following job script to train the network.

=== "pytorch-mnist.slurm"
    ```bash
    #!/bin/bash
    #SBATCH --job-name=pytorch
    #SBATCH --ntasks=1
    #SBATCH --time=01:00:00
    #SBATCH --account={PI_NetID}
    #SBATCH --partition=gpu
    #SBATCH --gres=gpu:1
    #SBATCH --output=%x-%j.out
    
    module load pytorch                   
    python examples/mnist/main.py >> output.log  
    ```

Submit the job:
```bash
$ cd /scratch/u/<NetID>/pytorch-test && sbatch pytorch.sh
```

## PyTorch Jupyter Notebook
This functionality is now provided by Open OnDemand!

See [Jupyter on Open OnDemand](../user-guide/access/ondemand.md#jupyter-notebooks) for more info.

--8<-- "includes/abbreviations.md"