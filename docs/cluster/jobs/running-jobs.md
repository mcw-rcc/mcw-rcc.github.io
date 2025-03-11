# Running Jobs

We use **SLURM** (Simple Linux Utility for Resource Management) for submitting, scheduling, and monitoring workloads, which we call jobs. Each job consists of resource requests and a set of commands to run. SLURM helps to schedule these jobs to run on the cluster using efficient methods to maximize throughput and minimize waiting. From the moment you submit a job, SLURM will make many decisions about priority of your workload, fair utilization of the cluster, and which resource is best to complete your job.

!!! tip "Clusters are shared resources."
    Please be respectful of all other users. Do not start resource-intensive scripts on a login node. Do not request more resources than your job can use. For more info see [User Etiquette](../etiquette.md).

Need help scheduling your job? Send us an email at {{ support_email }}.

## About SLURM

As a cluster workload manager, SLURM has three key functions. First, it allocates exclusive and/or non-exclusive access to resources (compute nodes) to users for some duration of time so they can perform work. Second, it provides a framework for starting, executing, and monitoring work (normally a parallel job) on the set of allocated nodes. Finally, it arbitrates contention for resources by managing a queue of pending work.

## Common SLURM Commands

These are the most common SLURM commands.

Submit a batch job script:

```bash
sbatch test-job.slurm
```

List queued and running jobs:

```bash
squeue
```

Show information about a running job:

```bash
squeue -j jobId # jobId is the job number
```

Cancel a queued job or stop a running job:

```bash
scancel jobId # jobId is the job number
```

Show history of job:

```bash
sacct -j jobId # jobId is the job number
```

!!! tip "RCC provides an additional command `slurminfo`."
    Use this command to see cluster info and a summary of SLURM stats.

    <!-- markdownlint-disable MD046 -->
    ```bash
    $ slurminfo
           QUEUE   FREE  TOTAL   FREE  TOTAL   RESORC    OTHER  MAXJOBTIME    CORES    NODE  GPU      
       PARTITION  CORES  CORES  NODES  NODES  PENDING  PENDING   DAY-HR:MN    /NODE  MEM-GB (COUNT)
          normal   2066   2880     41     60        0      144     7-00:00       48     360 -         
          bigmem     48     96      0      2        0        0     7-00:00       48    1500 -         
             gpu    223    384      1      8        0       12     7-00:00       48 360-480 gpu:v100:4(S:0-1)(6),gpu:a40:4(S:0-3)(2)
    ```
    <!-- markdownlint-enable MD046 -->

## Managing Job Input/Output Files

The HPC cluster requires a specific job workflow:

1. Copy job input/supporting files from your RGS directory within `/group/PI_NetID/...` to your scratch directory `/scratch/g/PI_NetID`
2. Submit a job from your `/scratch/...` directory that utilizes the staged job input/supporting files. **You must make sure the job I/O runs in your scratch directory.** The easiest way is to run the sbatch command in your `/scratch/...` directory.
3. When job finishes, copy results from `/scratch/...` back to `/group/PI_NetID/`....
4. If there are further computations, continues utilizing the job input/supporting files
5. When jobs are finished and staged job input/supporting files are no longer needed, delete the job input/supporting files from `/scratch/...`

!!! warning
    Your jobs will fail if you do not follow this procedure. Please note that files in `/scratch/...` that are older than 180 days will be deleted.

## SLURM Concepts

### Nodes, Cores, and Tasks

Before we discuss writing jobs scripts, or submitting jobs, it is important to understand how resources are requested and allocated through SLURM on HPC. SLURM uses three `#SBATCH` directives, `--ntasks`, `--nodes`, and `--cpus-per-task`, which can be used to run . SLURM uses the concept of tasks, which are processes that can use one or more CPU cores to run a copy of a program. Below we'll discuss how nodes, cores, and tasks are used by single-thread, multi-thread, multi-process, and MPI applications.

#### Single-thread

Many applications are only able to use one CPU core. For these single-thread programs, one task running on one core will suffice.

```txt
#SBATCH --ntasks=1
```

Most applications do not use more than one CPU core. Single-thread jobs that request multiple cores, waste resources. If your application does not mention multi-thread, multi-core, or MPI, please do not request more than one CPU core. When in doubt, contact RCC for clarification.

#### Multi-thread

A multi-thread application is able to use multiple cores on a single server by spawning multiple threads from a single process. Each thread uses one CPU core, and all share memory. For a multi-thread application, we can use a single task, and specify multiple cpus per task.

```txt
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
```

In this case, the job requests one task that can use 4 CPU cores. The user's application should then start one process that runs 4 threads.

Since all CPU cores are attached to one process, multi-thread applications cannot use more than one server. Multi-thread jobs that request multiple nodes, waste resources. If your application does not say MPI, please do not request more than one node. If your application does not mention multi-thread, multi-core, or MPI, please do not request more than one CPU core.
When in doubt, contact RCC for clarification.

#### Multi-process

A multi-process application, often referred to as "embarrassingly parallel", is able to use multiple cores by starting multiple independent processes. Each process uses one CPU core. For a multi-process application, we request multiple tasks.

```txt
#SBATCH --ntasks=4
```

In this case, the job requests 4 tasks, which will use 4 CPU cores. Each task can run one copy of the user's application on one CPU core. If your application does not mention multi-thread, multi-core, or MPI, please do not request more than one CPU core. When in doubt, contact RCC for clarification.

#### MPI

A multi-process application that is written to use Message Passing Interface (MPI), is able to use multiple cores across multiple nodes. MPI uses the SLURM task information to distribute the application processing across multiple cores and nodes. Each MPI process is able to communicate with the others using the high-performance, low-latency network. Therefore, an MPI application can scale across cores and nodes with little drop in performance. For a MPI application, we request multiple nodes and multiple tasks.

```txt
#SBATCH --nodes=4
#SBATCH --ntasks-per-node=8
```

In this case, the job requests 4 nodes, each running 8 MPI processes. The total number of MPI processes is equal to `nodes * tasks-per-node`. SLURM will run these MPI processes with the fewest nodes possible. In our case above, we request 4 nodes and 8 tasks-per-node, or 32 total MPI processes. If we run this job on HPC, SLURM will use 1 node and 32 CPU cores, since HPC has 48 cores per node. If we had requested a higher number of tasks-per-node, SLURM would allocate multiple nodes. This is all done transparently by SLURM.

Please note that there are far fewer MPI applications than single- or multi-thread. If your application does not specifically mention MPI in its documentation, it probably does not support MPI. Again, single- or multi-thread jobs that request multiple nodes, waste resources. If your application does not say MPI, please do not request more than one node. When in doubt, contact RCC for clarification.

### Partitions

SLURM partitions (aka queues) organize nodes in groups by type, and allow for scheduling, priority, fairshare, and more. The HPC cluster uses 3 partitions, normal, bigmem, and gpu. The partition configuration is a work in progress and we adjust resource limits as needed.

!!! tip "Default Partition"
    The default **normal** partition is applied automatically and the `--partition` flag is not required for jobs.

#### Normal

The **normal** partition contains the CPU-only nodes, cn01-cn60. This is the default partition, so any job that does not specify a partition will run in normal. Each node in this partition has **48 cores**, **360GB RAM**, and a **480GB SSD** for local scratch. Most jobs should use this partition, especially if you're using an MPI application. A job that does not parallelize, should use **1 core**. A job that does not use MPI, should use **48 cores or less**. Jobs using MPI can use multiple nodes.

#### Bigmem

The **bigmem** partition contains the large memory nodes, hm01-hm02. Each node in this partition has **48 cores**, **1.5TB RAM**, and a **480GB SSD** for local scratch. Use the `#SBATCH --partition=bigmem` directive to have your job run on a large memory server.

#### GPU

The **gpu** partition contains the gpu nodes. Nodes gn01-gn06 each have **48 cores**, **360GB RAM**, **4 V100 GPUs**, and a **480GB SSD** for local scratch. Nodes gn07-gn08 each have **48 cores**, **480GB RAM**, **4 A40 GPUs**, and a **3.84TB NVMe SSD** for local scratch. See [GPU Jobs](#gpu-jobs) below for more details.

### QOS

A Quality Of Service (QOS) modifier combines additional job resource limits and settings to existing partitions. The **dev** QOS is meant to allow interactive, development, and/or debugging jobs to be more responsive on the cluster. This QOS has more restricted limits for these jobs, since dev/debug jobs are not considered production jobs. However, the benefit of **dev** QOS is the higher priority setting, which in many cases will float your job to the top of the queue, potentially allowing it to run quicker. The **dev** QOS can be combined with any partition, so that these jobs can be combined with specialized resources such as high memory or GPU.

## Job Scheduling Policies

Job scheduling policies include resource limits on partitions and QOS's, and a fairshare algorithm. The fairshare configuration is currently in development.

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

## Writing a Job Script

A job script tells the scheduler what resources are required to run a specific set of workload commands. It is a text file using shell script syntax, denoted by the required first line, `#!/bin/bash`. It can be broken into two sections; resource requests and executable commands. Each section will be explained using the following example SLURM job script that can be used as a starting template for your jobs.

=== "test-job.slurm"

```txt
#!/bin/bash
#SBATCH --job-name=test-job
#SBATCH --ntasks=1
#SBATCH --mem-per-cpu=1gb
#SBATCH --time=00:01:00
#SBATCH --account=PI_NetID
#SBATCH --partition=partition
#SBATCH --output=%x-%j.out
#SBATCH --mail-type=ALL
#SBATCH --mail-user=NetID@mcw.edu

echo "Starting at $(date)"
echo "Job name: ${SLURM_JOB_NAME}, Job ID: ${SLURM_JOB_ID}"
echo "I have ${SLURM_CPUS_ON_NODE} CPUs on compute node $(hostname -s)"
```

### Job Requests

This section is comprised of `#SBATCH` directives that tell the scheduler what resources you're requesting. Some of the directives are required, as noted below. The job request section is always required at the top of a job script, but there is no specific order for the `#SBATCH` directives. While more than one directive can be combined on a single line, RCC does recommend a separate line for each.

#### Job Name

A job name is **required** and is set with the `#SBATCH --job-name=` option. Your job will appear in the queue with this name and job output files may be based on it. Letters, digits, underscore, and hyphens are allowed.

```txt
#SBATCH --job-name=myCoolJobName ### REQUIRED
```

#### CPU Resources

A CPU resource request is **required** and tells the job scheduler how to allocate processes. CPU resources can include `--ntasks`, `--cpus-per-task`, `--nodes`, or `--ntasks-per-node`.

```txt
#SBATCH --ntasks=1
```

Please see [Nodes, Cores, and Tasks](#nodes-cores-and-tasks) for more information.

#### Memory

A memory request is **required** and tells the job scheduler how much memory to allocate to processes. SLURM has default- and maximum-memory-per-core settings. If your job is single-thread, please use the `--mem` flag. If your job is multi-thread or MPI, please use the `--mem-per-cpu` flag. Please note that SLURM enforces memory per core and total memory. Your job will fail if your application exceeds the memory you requested, or the total available memory.

```txt
#SBATCH --mem=1gb
```

#### Time

A time request is **required** and tells SLURM how long your job will run. The `--time` flag sets the max time, in DD-HH:MM:SS, that your job can run. If your job exceeds that time limit, it will fail. SLURM is also configured with default and max time limits.

```txt
#SBATCH --time=00:01:00
```

!!! tip "Does your job require more than the maximum 7 day time limit?"
    Email the jobid number and time extension request to {{ support_email }}

#### Account

An account is **required** for your job to run. The `--account` option should be set to your PI's NetID, or the NetID of a collaborator PI. If you need to adjust your account membership, please contact RCC.

```txt
#SBATCH --account=PI_NetID
```

!!! tip "You can easily find your accounts with the `myaccts` command."

    <!-- markdownlint-disable MD046 -->
    ```bash
    $ myaccts
    Account        Partition
    pi             bigmem,dev,gpu,normal
    ```
    <!-- markdownlint-enable MD046 -->

#### Partition

A partition is not required but can be added with `#SBATCH --partition=`. The partition flag is only needed if requesting complex resources, such as the large memory or GPU nodes.

```txt
#SBATCH --partition=<partition>
```

#### Job Output

A job output flag is not required. By default SLURM will send job output to `slurm-%j.out` in the working directory, where `%j` is the SLURM jobid. If the `--output` flag is used, SLURM will output the application's STDOUT to the given filename. In our example script, SLURM output is sent to `%x-%j.out`, where `%x` is the job name.

```txt
#SBATCH --output=%x-%j.out
```

#### Notifications

Notifications are **optional** and can be sent from the job scheduler to an email address of your choice. Many types are supported, but common options include when a job begins `#SBATCH --mail-type=BEGIN`, when a job ends `#SBATCH --mail-type=END`, and when a job fails `#SBATCH --mail-type=FAIL`. Options may be combined, i.e. `#SBATCH --mail-type=BEGIN,END`. There is also an option to send all notifications `#SBATCH --mail-type=ALL` The recipient email is specified with `#SBATCH --mail-user=NetID@mcw.edu`. RCC recommends using notifications during job debugging. Adding notifications as a default may result in email spam, especially if you're submitting many jobs.

```txt
#SBATCH --mail-type=ALL ### OPTIONAL
#SBATCH --mail-user=NetID@mcw.edu ### OPTIONAL
```

### Job Commands

The job commands section always follows the job request section and begins with the first non #SBATCH line. Executable commands commonly include file I/O, software commands, and directory cleanup.

```bash
echo "Starting at $(date)"
echo "Job name: ${SLURM_JOB_NAME}, Job ID: ${SLURM_JOB_ID}"
echo "I have ${SLURM_CPUS_ON_NODE} CPUs on compute node $(hostname -s)"
```

In the example job script, the job commands include several environment variables that are created by SLURM when your job starts. These can be useful to automate tasks or print helpful information about your job to your output file.

## Submit a Batch Job

Most jobs that run on HPC are batch jobs. A batch job is submitted with the `sbatch` command and requires a job script. This is the best method for production job as it allows you to submit many jobs and let SLURM do the work. With a batch job, there is no requirement that you sit and watch the command-line. You can submit the job and come back later.

```bash
sbatch test-job.slurm
```

A job script is required. If you haven't already, please review [Writing a Job Script](#writing-a-job-script).

## Submit an Interactive Job
!!! tip "Interactive jobs are limited to 8 hours max walltime!"
    Make sure to request 8 hours or less or your job will not run!
    
Users may need an interactive session for debugging or related tasks. An interactive job is submitted with the `srun` command. All of the SLURM options that are required in job scripts are required with the `srun` command. In addition, `--qos=dev` is added automatically.

```bash
srun --ntasks=1 --mem-per-cpu=4GB --time=01:00:00 --job-name=interactive --account=PI_NetID --pty bash
```

To stop your interactive job, use the `exit` command.

```bash
exit
```

!!! tip "Open OnDemand"
    We encourage all users to make use of [Open OnDemand](https://ondemand.rcc.mcw.edu/){:target="_blank"} for interactive sessions.

## GPU Jobs

The cluster includes 8 compute nodes that each have 4 GPUs. Nodes gn01-gn06 each have 48 cores, 360GB RAM, 4 V100 GPUs, and a 480GB SSD for local scratch. Nodes gn07-gn08 each have 48 cores, 480GB RAM, 4 A40 GPUs, and a 3.84TB NVMe SSD for local scratch You can add a GPU to your batch or interactive job submission with the `--gres=gpu:N` flag, where ***N*** is the number of GPUs.

In a job script add:

```txt
#SBATCH --gres=gpu:1
#SBATCH --partition=gpu
```

Interactive job command:

```bash
srun --ntasks=1 --mem-per-cpu=4GB --gres=gpu:1 --time=01:00:00 --job-name=interactive --partition=gpu --account=PI_NetID --pty bash
```

Use the `#SBATCH --partition=gpu` and `#SBATCH --gres=gpu:1` directives to have your job run on a gpu node.

### GPU Type

To use a specific GPU type, use `--gres=gpu:type:1`, where ***type*** is either `v100` or `a40`. Please note that most jobs will not benefit from specifying a GPU type, and this may delay scheduling of your job.

### GPU Compute Mode

Your workload may require a specific GPU compute mode (***not common***). If this is required, Research Computing staff will provide instructions for that software either in the software pages on this site, or the software's `module help` output.

For reference, you can specify GPU compute mode with `--gpu_cmode=mode`, where ***mode*** is `shared` or `exclusive`. Run command `srun --help` to see additional information.`

## Debug/Dev Jobs

The cluster includes a special `dev` QOS. Any job using this QOS will have a higher priority than non-QOS jobs. The goal is for a interactive/dev/debug job to start running quicker, to speed up the developer's work. While this QOS will attempt to move your job to the top of the queue, it does not guarantee the job will start immediately. This can be combined with any of the other partition and gres requests.

In a job script add:

```txt
#SBATCH --qos=dev
#SBATCH --partition=<partition>
```

## Array Jobs

A job array is used to submit multiple similar jobs. Each element in the array represents an independent subjob.

Job arrays are submitted with the SLURM option `--array=x-y`. Here we specify an array with index 1-5, or 5 subjobs.

```txt
#SBATCH --array=1-5
```

If the SLURM job number is 1000, then the array subjobs will have ids `1000_1`, `1000_2`, ..., `1000_5`. Each subjob will also have an environment variable `SLURM_ARRAY_TASK_ID` that is set to its array index value.

When submitting many jobs at once, the job array can limit the number of subjobs running at a time. Here we specify an array with index 1-50, limited to 5 running at a time.

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

Example: We want to run the same code on 50 different input files. The input files are conveniently named `input-1`, `input-2`, etc. The following script will process all 50 input files, 5 at a time.

=== "test-array-job.slurm"

```bash
#!/bin/bash
#SBATCH --job-name=array_example
#SBATCH --nodes=1
#SBATCH --ntasks=5
#SBATCH --mem-per-cpu=1gb
#SBATCH --time=00:10:00
#SBATCH --array=1-50%5
#SBATCH --output=array_%A-%a.out

command input-${SLURM_ARRAY_TASK_ID}
```

## Job Scheduling and Maintenance

If you submit a job before a maintenance window and the job is sitting in the queue, first check to see why with `squeue`. In the output, you'll see the last column `NODELIST(REASON)`. If your job is held for maintenance, you'll see `(ReqNodeNotAvail, Reserved for maintenance)`.

Each job has a [walltime request](#time). The walltime request sets the amount of time that your job will be allowed to run on the cluster. On the HPC cluster, the maximum allowed walltime is 7 days.

If you submit your job with X hours walltime request, and the maintenance window starts in less than X hours, the job will be held until after the maintenance window closes. The reason is that the scheduler cannot guarantee your job will finish before maintenance begins.

To make the job run, resubmit your job with a walltime request that ensures the job will finish before the maintenance window. We can use a simple formula to find that walltime.

`walltime = ( $maint_start_time - $current_time )`

To simplify the process, use the `maxwalltime` command, which provides the maximum walltime request that will also allow your job to run before maintenance.

!!! info
    Please note, RCC schedules maintenance windows every first Wednesday of the month from 9PM-12AM.

--8<-- "includes/abbreviations.md"
