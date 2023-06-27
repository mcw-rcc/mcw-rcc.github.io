# Nextflow

Nextflow is a workflow software used to manage data and compute-intensive pipelines. It is commonly used in genomic workflows but is generally applicable. See <https://www.nextflow.io/> for more information and [tutorials](https://www.nextflow.io/blog/2023/learn-nextflow-in-2023.html).

## Pipelines

Many ready-to-use pipelines are available from [nf-core](https://nf-co.re/pipelines), which is a collection of community-built pipelines using Nextflow.

## Running a pipeline

Nextflow pipelines require a master process that controls each part. You can configure your pipeline so that the master process launches these sub-jobs locally (same server), or using a workload manager like SLURM. Either method will work on the HPC cluster.

!!! warning "Nextflow master process"
    Do not start a nextflow pipeline on a login node. This is true even if you're using the SLURM executor. Instead, start your pipeline in an interactive or batch job so that the master process has dedicated resources. Admins will delete your pipeline process if it is running on a login node.

### Interactive job

We'll start with a simple `"Hello world!"` pipeline. To get started, launch an interactive job.

```txt
srun --job-name=nextflow --time=1:00:00 --ntasks=1 --pty bash
```

Next we load the nextflow module and run the pipeline.

```txt
module load nextflow
nextflow run hello
```

The output will show the nextflow version, pipeline source, and results.

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

That simple workflow printed `"Hello world!` in four languages. Notice that the executor was set to local, which uses the resources of the interactive job on the same server. The local executor works for many workflows but may not be appropriate in all cases. For resource intensive workflows, such as genomics pipelines, use the SLURM executor to run each part its own new and separate SLURM job.

!!! warning "Understand your pipeline!"
    Take time to read the Nextflow documentation before you submit a pipeline using the SLURM executor. A misconfigured pipeline could submit many thousands of unwanted jobs and adversely affect the cluster scheduler.

To run the same `Hello world!` workflow using the SLURM executor, add a `nextflow.config` file for the pipeline.

=== "nextflow.config"

```txt
profiles {
    local {
        process.executor = 'local'
    }
    cluster {
        process.executor = 'slurm'
        // our cluster requires time limit for every job
        // you may need to modify for your needs
        process.executor = '1:00:00'
    }
}
```

To launch the pipeline, select your profile enabling the SLURM executor.

```txt
nextflow run -profile cluster hello
```

Output will show the executor is SLURM.

```txt
N E X T F L O W  ~  version 23.04.1
NOTE: Your local project version looks outdated - a different revision is available in the remote repository [1d71f857bb]
Launching `https://github.com/nextflow-io/hello` [stupefied_lattes] DSL2 - revision: 4eab81bd42 [master]
executor >  slurm (4)
[60/75c541] process > sayHello (4) [100%] 4 of 4 ✔
Bonjour world!

Ciao world!

Hello world!

Hola world!

Completed at: 26-Jun-2023 19:46:14
Duration    : 2m 2s
CPU hours   : (a few seconds)
Succeeded   : 4
```

### Batch job

To run the same workflow in a batch job, create a job script.

=== "nf-hello.slurm"

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
sbatch nf-hello.slurm
```
