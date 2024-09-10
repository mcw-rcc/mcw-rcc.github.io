# Nextflow

Nextflow is a workflow software used to manage data and compute-intensive pipelines. It is commonly used in genomic workflows but is generally applicable. See [nextflow](https://www.nextflow.io/){:target="_blank"} for more information and [tutorials](https://www.nextflow.io/blog/2023/learn-nextflow-in-2023.html){:target="_blank"}.

## Finding pipelines

Many ready-to-use pipelines are available from [nf-core](https://nf-co.re/pipelines){:target="_blank"}, which is a collection of community-built pipelines using Nextflow.

## Anatomy of a pipeline

A Nextflow pipeline consists of an ordered list of tasks with resource requirements and software dependencies. Each task or step is run in a specific order to produce an end result, and a master process controls when and where these tasks are executed. When a Nextflow pipeline begins, the master process will download software dependencies using a software management system such as Docker, Singularity, Conda, etc. This provides a portable and reproducible software environment for each task.

Task execution can be delegated to a workload manager like SLURM, which is used on the HPC cluster. With the SLURM executor, the master process has a dedicated SLURM job, and each task is run as a separate SLURM job. Tasks can also be run with a local executor, i.e. no outside workload manager, where pipeline tasks run in the same SLURM job as the master process. The choice of executor determines what resources are available to the master process and pipeline tasks. The local executor is fine for simple pipelines and testing. For more advanced pipelines including NGS workflows, the SLURM executor will generally be more efficient.

## Running a pipeline

We can use an interactive or batch job to start the pipeline master process. This guarantees that the master process has dedicated resources. Do not start a nextflow pipeline on a login node, even if you're using the SLURM executor. Admins will delete your pipeline process if it is running on a login node.

!!! warning "Understand your pipeline!"
    Take time to read the Nextflow and pipeline-specific documentation. A misconfigured pipeline could adversely affect the cluster scheduler and yield poor performance results for you. If you have any questions, please contact {{ support_email }}

### Interactive job

We'll start with a simple `Hello world!` pipeline. To get started, launch an interactive job.

```txt
srun --job-name=nextflow --time=1:00:00 --ntasks=1 --pty bash
```

Next we load the nextflow module and start the pipeline with the default local executor.

```txt
module load nextflow
nextflow run hello
```

The output below shows `Hello world!` printed in four languages. Notice that the executor was set to local by default, which launches the pipeline tasks in the same job as the master process. The local executor works for many workflows, but may not be appropriate in all cases. For resource-intensive workflows, such as NGS pipelines, use the SLURM executor.

```txt
N E X T F L O W  ~  version 23.04.1
NOTE: Your local project version looks outdated - a different revision is available in the remote repository [1d71f857bb]
Launching `https://github.com/nextflow-io/hello` [condescending_descartes] DSL2 - revision: 4eab81bd42 [master]
executor >  local (4)
[d6/af64fc] process > sayHello (4) [100%] 4 of 4 ✔
Bonjour world!

Ciao world!

Hello world!

Hola world!
```

To run the same `Hello world!` workflow using the SLURM executor, add a `nextflow.config` file for the pipeline in your working directory.

=== "nextflow.config"

```txt
process {
    executor = 'slurm'
    // time limit needed for this example
    // most pipelines from nf-core have this built-in
    time = '1:00:00'
}
```

To use your new config file, add the `-c` flag.

```txt
nextflow run -c nextflow.config hello
```

Output will show the executor is SLURM. If you watch the SLURM queue with command `squeue`, you will see 4 additional jobs launch.

```txt
N E X T F L O W  ~  version 23.04.1
NOTE: Your local project version looks outdated - a different revision is available in the remote repository [1d71f857bb]
Launching `https://github.com/nextflow-io/hello` [nasty_lagrange] DSL2 - revision: 4eab81bd42 [master]
executor >  slurm (4)
[92/338040] process > sayHello (4) [100%] 4 of 4 ✔
Ciao world!

Bonjour world!

Hello world!

Hola world!
```

### Batch job

To run the same workflow with local executor in a batch job, use the `nf-hello-local-exec.slurm` job script.

=== "nf-local-exec.slurm"

```txt
#!/bin/bash
#SBATCH --job-name=nextflow
#SBATCH --ntasks=1
#SBATCH --time=1:00:00
#SBATCH --output=%x-%j.out

module load nextflow
nextflow run hello
```

Submit the job:

```txt
sbatch nf-local-exec.slurm
```

To use the SLURM executor with a batch job, use the `nf-slurm-exec.slurm` job script.

=== "nf-slurm-exec.slurm"

```txt
#!/bin/bash
#SBATCH --job-name=nextflow
#SBATCH --ntasks=1
#SBATCH --time=1:00:00
#SBATCH --output=%x-%j.out

module load nextflow
nextflow run -c nextflow.config hello
```

Submit the job:

```txt
sbatch nf-slurm-exec.slurm
```

## Examples

Here we provide suggested configurations for various pipelines.

!!! warning "Configuration issue"
    The nf-core pipelines have a known bug with configuration files. If you attempt to combine your params and nextflow.config files, the pipeline will fail. To avoid this, put your params in `nf-params.json`, and general nextflow configuration in a `nextflow.config` file. You will pass the params file to nextflow on the command-line. The `nextflow.config` file is ready automatically when found in your working directory.

### nf-core/rnaseq

These configurations are the minimum required. You will need to customize and add additional parameters to the `nf-params.json` file as needed for your specific analysis. See <https://nf-co.re/rnaseq/3.12.0/parameters>{:target="_blank"} for details.

<!-- markdownlint-disable MD046 -->
=== "nf-params.json"

    ```txt
    {
        "input": "samplesheet.csv",
        "outdir": ".\/exampleOut"
    }
    ```

=== "nextflow.config"

    ```txt
    process {
        executor = 'slurm'
    }
    ```

=== "nf-rnaseq.slurm"

    ```txt
    #!/bin/bash
    #SBATCH --job-name=nf-rnaseq
    #SBATCH --nodes=1
    #SBATCH --ntasks=2
    #SBATCH --time=12:00:00
    #SBATCH --output=%x-%j.out
    
    module load nextflow
    module load singularity

    nextflow run nf-core/rnaseq -name batch1 -profile singularity -params-file nf-params.json
    ```
<!-- markdownlint-enable MD046 -->
