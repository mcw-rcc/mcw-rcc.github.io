# About SLURM

As a cluster workload manager, SLURM has three key functions. First, it allocates exclusive and/or non-exclusive access to resources (compute nodes) to users for some duration of time so they can perform work. Second, it provides a framework for starting, executing, and monitoring work (normally a parallel job) on the set of allocated nodes. Finally, it arbitrates contention for resources by managing a queue of pending work.

## Nodes, Cores & Tasks

It is important to understand how resources are requested and allocated through SLURM on HPC. SLURM uses three `#SBATCH` directives, `--ntasks`, `--nodes`, and `--cpus-per-task`, which can be used to run a job with a specific set of resources. SLURM uses the concept of tasks, which are processes that can use one or more CPU cores to run a copy of a program. Below we'll discuss how nodes, cores, and tasks are used by single-thread, multi-thread, multi-process, and MPI applications.

### Single-thread

Many applications are only able to use one CPU core. For these single-thread programs, one task running on one core will suffice. In the section below on how to write a script, we will explain more in detail how to write the heather of your script file where you specify the resources that your job needs. But for now you should now that for these type of applications the heather should include the following lines:

```txt
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
```

Most applications do not use more than one CPU core. Single-thread jobs that request multiple cores, waste resources. If your application does not mention multi-thread, multi-core, or MPI, please do not request more than one CPU core. When in doubt, contact RCC for clarification.

### Multi-thread

A multi-thread application is able to use multiple cores on a single server by spawning multiple threads from a single process. Each thread uses one CPU core, and all share memory. For a multi-thread application, we can use a single task, and specify multiple cpus per task. For this, you would include the following lines in the heather of your script (in this example we are requesting 4 CPU cores, you would edit the value depending the number of cpus/threads that your program will run):

```txt
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
```

In this case, the job requests one task that can use 4 CPU cores. The user's application should then start one process that runs 4 threads. Since we are the job only requires one task, your job will be assigned a single node.

Since all CPU cores are attached to one process, multi-thread applications cannot use more than one server. Multi-thread jobs that request multiple nodes, waste resources. If your application does not say MPI, please do not request more than one node. If your application does not mention multi-thread, multi-core, or MPI, please do not request more than one CPU core.
When in doubt, contact RCC for clarification.

### Multi-process

A multi-process application, often referred to as "embarrassingly parallel", is able to use multiple cores by starting multiple independent processes or tasks. Each task is a separate instance of the running program and could have one or multiple threads. The threads within a process share memory, but they don't share memory with threads from a different process. Because of this and depending on the number of threads (CPUs) requested per process, each process could run on the same or a different node.

For example, if your program needs 4 independent processes, each running on a single thread, you would include the following lines in the heather of your script:

```txt
#SBATCH --ntasks=4
#SBATCH --cpus-per-task=1
```

In this case, the job requests 4 tasks, which will use 1 CPU cores. Each of these 4 tasks could run in a different node since they do not share memory but they could also run all in a same node if there are enough resources in that node.

If your program needs 4 independent processes, each running on 8 threads, you would include the following lines:

```txt
#SBATCH --ntasks=4
#SBATCH --cpus-per-task=8
```

In this case, the job requests 4 tasks, which will use 8 CPU cores each (32 CPU in total). Again, each task could run in a separate or all in one node depending on the other resources that are requested and the resources available on each node. Each set of 8 threads will share memory and will run in a single node.

If your application does not mention multi-thread, multi-core, or MPI, please do not request more than one CPU core. When in doubt, contact RCC for clarification.

### MPI

A multi-process application that is written to use Message Passing Interface (MPI), is able to use multiple cores across multiple nodes. MPI uses the SLURM task information to distribute the application processing across multiple cores and nodes. Each MPI process is able to communicate with the others using the high-performance, low-latency network. Therefore, an MPI application can scale across cores and nodes with little drop in performance. For a MPI application, we request multiple nodes and multiple tasks.

```txt
#SBATCH --nodes=4
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=3
```

In this case, the job requests 4 nodes, each running 8 MPI processes. The total number of MPI processes is equal to `nodes * tasks-per-node` (32 in this case). The number of threads that each process will run will depend on the value of --cpus-per-task. In this case, each of the 32 processes is running 3 threads (96 threads in total). SLURM will run these MPI processes with the fewest nodes possible. In our case above, we request 4 nodes and 8 tasks-per-node, or 32 total MPI processes. If we run this job on HPC, SLURM will use 1 node and 32 CPU cores, since HPC has 48 cores per node. If we had requested a higher number of tasks-per-node, SLURM would allocate multiple nodes. This is all done transparently by SLURM.

Please note that there are far fewer MPI applications than single- or multi-thread. If your application does not specifically mention MPI in its documentation, it probably does not support MPI. Again, single- or multi-thread jobs that request multiple nodes, waste resources. If your application does not say MPI, please do not request more than one node. When in doubt, contact RCC for clarification.

## Partitions

SLURM partitions (aka queues) organize nodes in groups by type, and allow for scheduling, priority, fairshare, and more. The HPC cluster uses 3 partitions, normal, bigmem, and gpu. The partition configuration is a work in progress and we adjust resource limits as needed.

!!! tip "Default Partition"
    The default **normal** partition is applied automatically and the `--partition` flag is not required for jobs.

### Normal

The **normal** partition contains the CPU-only nodes, cn01-cn60. This is the default partition, so any job that does not specify a partition will run in normal. Each node in this partition has **48 cores**, **360GB RAM**, and a **480GB SSD** for local scratch. Most jobs should use this partition, especially if you're using an MPI application. A job that does not parallelize, should use **1 core**. A job that does not use MPI, should use **48 cores or less**. Jobs using MPI can use multiple nodes.

### Bigmem

The **bigmem** partition contains the large memory nodes, hm01-hm02. Each node in this partition has **48 cores**, **1.5TB RAM**, and a **480GB SSD** for local scratch. Use the `#SBATCH --partition=bigmem` directive to have your job run on a large memory server.

### GPU

The **gpu** partition contains the gpu nodes. Nodes gn01-gn06 each have **48 cores**, **360GB RAM**, **4 V100 GPUs**, and a **480GB SSD** for local scratch. Nodes gn07-gn08 each have **48 cores**, **480GB RAM**, **4 A40 GPUs**, and a **3.84TB NVMe SSD** for local scratch. Use the `#SBATCH --partition=gpu` directive to have your job run on a gpu server.

If your job uses one or more GPUs, you will also need to add the `--gres=gpu:N` flag, where ***N*** is the number of GPUs. In a job script add:

```txt
#SBATCH --gres=gpu:1
#SBATCH --partition=gpu
```

To use a specific GPU type, use `--gres=gpu:type:1`, where ***type*** is either `v100` or `a40`. Please note that most jobs will not benefit from specifying a GPU type, and this may delay scheduling of your job.

## Array Jobs

If you need to run the same commands on different files or subjects, you might benefit from using array jobs. A job array is used to submit multiple similar jobs. Each element in the array represents an independent subjob.

Job arrays are submitted with the SLURM option `--array=x-y`. Here we specify an array with index 1-5, or 5 subjobs.

```txt
#SBATCH --array=1-5
```

If the SLURM job number is 1000, then the array subjobs will have ids `1000_1`, `1000_2`, ..., `1000_5`. Each subjob will also have an environment variable `SLURM_ARRAY_TASK_ID` that is set to its array index value.

When submitting many jobs at once, the job array can limit the number of subjobs running at a time. Here we specify an array with index 1-50, limited to 5 running at a time. Even if you don't specify how many jobs you would like to run at a time, the job scheduler will limit to the maximum amount aloud by the cluster. Is better to specify the limit yourself because otherwise you will be using your maximum amount of aloud jobs and will not be able to run any other jobs.

```txt
#SBATCH --array=1-50%5
```

A job array can be deleted with the `scancel` command.

```bash
scancel 1000
```

Alternatively, a single subjob can be deleted.

```bash
scancel 1000_3
```

Example 1: We want to run the same code on 50 different input files. The input files are conveniently named `input-1`, `input-2`, etc. The following script will process all 50 input files, 5 at a time.

=== "test-array-job.slurm"

```bash
#!/bin/bash
#SBATCH --job-name=array_example
#SBATCH --ntasks=5
#SBATCH --cpu-per-task=1
#SBATCH --mem-per-cpu=1gb
#SBATCH --time=00:10:00
#SBATCH --array=1-50%5
#SBATCH --account=account

command input-${SLURM_ARRAY_TASK_ID}
```

Example 2: We want to run the same code on 50 subjects, the subject files are stored in a subfolder inside scratch with each subject ID. Also, we have a file called list.txt with each subject ID in a separate line.

```bash
#!/bin/bash
#SBATCH --job-name=array_example
#SBATCH --ntasks=5
#SBATCH --cpu-per-task=1
#SBATCH --mem-per-cpu=1gb
#SBATCH --time=00:10:00
#SBATCH --array=1-50%5
#SBATCH --account=account

subjects=($(cat $SCRATCH/list.txt))
sbjID=${subjects[SLURM_ARRAY_TASK_ID-1]}

echo "Running job on ${sbjID}"
cd $SCRATCH/$sbjID
```

## Job Scheduling Policies

Job scheduling policies include resource limits on partitions and [QOS](https://slurm.schedmd.com/qos.html){:target="_blank"}, and a fairshare algorithm. The fairshare configuration is currently in development.

### Resource Limits

| Partition | Max Time (default) | Max Nodes/User | Max Cores/User | Max GPUs/User | Max Mem/Core (default) | Priority |
| --------- | ------------------ | -------------- | -------------- | ------------- | ---------------------- | -------- |
| normal    | 7 days (2 hrs)     | 30             | 1440           | N/A           | 7.5GB (7.5GB)          | 10       |
| bigmem    | 7 days (2 hrs)     | 1              | 48             | N/A           | 31GB (31GB)            | 10       |
| gpu       | 7 days (2 hrs)     | 4              | 192            | 16            | 10GB (7.5GB)           | 10       |

| QOS       | Max Time           | Max Nodes/User | Max Cores/User | Max GPUs/User | Max Mem/Core (default) | Priority |
| --------- | ------------------ | -------------- | -------------- | ------------- | ---------------------- | -------- |
| dev       | 8 hrs              | 1              | 8              | 1             | 7.5GB (7.5GB)          | 10000    |

### Fairshare

Fairshare is a scheduler algorithm that manages job priority, based on a comparison of all cluster utilization, to promote equal use the cluster. Fairshare tracks daily usage and uses data from the previous 7 days to adjust job priority. The effect of fairshare data is reduced each day using a decay factor. In practice, when a user increases their cluster utilization, their job priority is lowered compared to other users.

Fairshare effectively allows infrequent users a fair chance to run their jobs on the cluster among the many jobs of more frequent users.
